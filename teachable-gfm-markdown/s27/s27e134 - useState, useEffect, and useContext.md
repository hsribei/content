
## useState, useEffect, and useContext

React comes with [a bunch of basic and advanced
hooks](https://reactjs.org/docs/hooks-reference.html). The core hooks
are:

  - `useState` for managing state
  - `useEffect` for side-effects
  - `useContext` for React’s context API

Here’s how to think about them in a nutshell :point\_down:

### useState

The `useState` hook replaces pairs of state getters and setters.

``` javascript
class myComponent extends React.Component {
    state = {
      value: 'default'
    }
    
    handleChange = (e) => this.setState({
      value: e.target.value
    })
    
    render() {
      const { value } = this.state;
      
      return <input value={value} onChange={handleChange} />
    }
}
```

:point\_down:

``` javascript
const myComponent = () => {
  const [value, setValue] = useState('default');
  
  const handleChange = (e) => setValue(e.target.value)
  
  return <input value={value} onChange={handleChange} />
}
```

Less code to write and understand.

In a class component you: - set a default value - create an `onChange`
callback that fires `setState` - read value from state before rendering
etc.

Without modern fat arrow syntax you might run into trouble with binds.

The hook approach moves that boilerplate to React’s plate. You call
`useState`. It takes a default value and returns a getter and a setter.

You call that setter in your change handler.

Behind the scenes React subscribes your component to that change. Your
component re-renders.

### useEffect

`useEffect` replaces the `componentDidMount`, `componentDidUpdate`,
`shouldComponentUpdate`, `componentWillUnmount` quadfecta. It’s like a
trifecta, but four.

Say you want a side-effect when your component updates, like make an API
call. Gotta run it on mount and update. Want to subscribe to a DOM
event? Gotta unsubscribe on unmount.

Wanna do all this only when certain props change? Gotta check for that.

Class:

``` javascript
class myComp extends Component {
  state = {
      value: 'default'
    }
    
    handleChange = (e) => this.setState({
      value: e.target.value
    })
    
    saveValue = () => fetch('/my/endpoint', {
        method: 'POST'
        body: this.state.value
    })
    
    componentDidMount() {
        this.saveValue();
    }
    
    componentDidUpdate(prevProps, prevState) {
        if (prevState.value !== this.state.value) {
            this.saveValue()
        }
    }
    
    render() {
      const { value } = this.state;
      
      return <input value={value} onChange={handleChange} />
    }
}
```

:point\_down:

``` javascript
const myComponent = () => {
  const [value, setValue] = useState('default');
  
  const handleChange = (e) => setValue(e.target.value)
  const saveValue = () => fetch('/my/endpoint', {
        method: 'POST'
        body: value
    })
    
    useEffect(saveValue, [value]);
  
  return <input value={value} onChange={handleChange} />
}
```

So much less code\!

`useEffect` runs your function on `componentDidMount` and
`componentDidUpdate`. And that second argument, the `[value]` part,
tells it to run only when `value` changes.

No need to double check with a conditional. If your effect updates the
component itself through a state setter, the second argument acts as a
`shouldComponentUpdate` of sorts.

When you return a method from `useEffect`, it acts as a
`componentWillUnmount`. Listening to, say, your mouse position looks
like this:

``` javascript
const [mouseX, setMouseX] = useState();
const handleMouse = (e) => setMouseX(e.screenX);

useEffect(() => {
    window.addEventListener('mousemove', handleMouse);
    return () => window.removeEventListener(handleMouse);
})
```

Neat :okay\_hand:

### useContext

`useContext` cleans up your render prop callbacky hell.

``` javascript
const SomeContext = React.createContext()

// ...

<SomeContext.Consumer>
  {state => ...}
</SomeContext.Consumer>
```

:point\_down:

``` javascript
const state = useContext(SomeContext)
```

Context state becomes just a value in your function. React auto
subscribes to all updates.

And those are the core hooks. useState, useEffect, and useContext. You
can use them to build almost everything. :okay\_hand:
