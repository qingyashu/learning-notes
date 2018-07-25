[tutorial link](https://reactjs.org/tutorial/tutorial.html)
[demo link](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)
#react.Component class
- Takes in parameter `props`
- Render view via `render` method
  - Using JSL to describe output view
  - Include js expressions between braces
- To make `this` pointer valid at calling time, remember to bind `this` to event handling methods (such as `onClick`) in constructor. 
- When changing data state, keeping immutability is important, because it helps tracking history, detecting re-rendering. 
  - Immutability means replacing data with copied and changed data, instead of directly changing the values of original data. 
- Differences when handling events between DOM and React:
  - React uses JSX between braces, while DOM uses string
  - React uses camel case, while DOM use lowercase
- For simple components with only render function, simply write a function that takes `props` as input parameters and returns what should be rendered. 
- When creating a list, React take `key` value to store and detect which data need update. `key` is a special property, and cannot be reached by `props` values.
- User-Defined Components Must Be Capitalized
- Props are Read-Only. Whether you declare a component [as a function or a class](https://reactjs.org/docs/components-and-props.html#functional-and-class-components), it must never modify its own props. All React components must act like pure functions with respect to their props.
- The `componentDidMount()` hook runs after the component output has been rendered to the DOM.
- If the component is ever removed from the DOM, React calls the `componentWillUnmount()`
- Do Not Modify State Directly
For example, this will not re-render a component:
```javascript
// Wrong
this.state.comment = 'Hello';
Instead, use setState():

// Correct
this.setState({comment: 'Hello'});
```
- Do not use direct state value for reference because values in `state` might be asynchronous. Use `previousState` instead: 
```javascript
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```
# Other Tips

