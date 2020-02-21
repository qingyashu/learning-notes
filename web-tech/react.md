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
# React Router
- 3 Types of components in React Router
  - Router components
  - Route matching components
  - Navigation components
- Router components:
  - `<BrowserRouter>`: use this if you have a server that responds to requests
  - `<HashRouter>`: for static file server
  - Both of these create specialized history object. 
- Route matching components: `<Route>` and `<Switch>`
  - `<Route>` compares `path` prop to the current location's pathname. When match, it renders. When not match, it renders null. A `<Route>` with no `path` prop will always match and render. 
```
// when location = { pathname: '/about' }
<Route path='/about' component={About}/> // renders <About/>
<Route path='/contact' component={Contact}/> // renders null
<Route component={Always}/> // renders <Always/>
```
  - `<Switch>` will iterate all its `<Route>` children and render the first one that matches current location. It can used to for animation transitions between routes, or 404 pages. 
```
<Switch>
  <Route exact path='/' component={Home}/>
  <Route path='/about' component={About}/>
  <Route path='/contact' component={Contact}/>
  {/* when none of the above match, <NoMatch> will be rendered */}
  <Route component={NoMatch}/>
</Switch>
```
  - Route rendering props: `component`, `render`, and `children`
    - `component`: for existing components
    - `render`: takes in inline function, to pass in-scope variables to the components. 
    - Don't use `component` with inline functions. 
```
const Home = () => <div>Home</div>

const App = () => {
  const someVariable = true;
  
  return (
    <Switch>
      {/* these are good */}
      <Route exact path='/' component={Home} />
      <Route
        path='/about'
        render={(props) => <About {...props} extra={someVariable} />}
      />
      {/* do not do this */}
      <Route
        path='/contact'
        component={(props) => <Contact {...props} extra={someVariable} />}
      />  
    </Switch>
  )
}
```
- Navigation
  - `<Link>`: render an `<a>` tag
```
<Link to='/'>Home</Link>
// <a href='/'>Home</a>
```
  - `<NavLink>`: render an `<a>` with active state when current location matches path. 
```
// location = { pathname: '/react' }
<NavLink to='/react' activeClassName='hurray'>React</NavLink>
// <a href='/react' className='hurray'>React</a>
```
  - `<Redirect>`: force navigation
```
<Redirect to='/login'/>
```
