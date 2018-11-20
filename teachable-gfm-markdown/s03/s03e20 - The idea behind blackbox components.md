
# Quickly integrate any D3 code in your React project with Blackbox Components

Blackbox components are the quickest way to integrate D3 and React. You
can think of them as wrappers around D3 visualizations.

With the blackbox approach, you can take any D3 example from the
internets or your brain, wrap it in a React component, and it Just
Works™. This is great when you’re in a hurry, but comes with a big
caveat: You’re letting D3 control some of the DOM.

D3 controlling the DOM is *okay*, but it means React can’t help you
there. That’s why it’s called a Blackbox – React can’t see inside.

No render engine, no tree diffing, no dev tools to inspect what’s going.
Just a blob of DOM elements.

Okay for small components or when you’re prototyping, but I’ve had
people come to my workshops and say *“We built our whole app with the
blackbox approach. It takes a few seconds to re-render when you click
something. Please help”*

🤔

Here’s how it works: - React renders an anchor element - D3 hijacks it
and puts stuff in

You manually re-render on props and state changes. Throwing away and
rebuilding the entire DOM subtree on each render. With complex
visualizations this becomes a huge hit on performance.

Use this technique sparingly.
