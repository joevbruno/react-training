
# Troubleshooting

## React / Redux

### `Uncaught Error: Invariant Violation: ...A valid ReactComponent must be returned. You may have returned undefined, an array or some other invalid object.`

This errors means there is a problem with one of your React components or *with one of its children*. 

Diagnosing the problem
+ Make sure you are only returning **one** component. If you are return more than one, wrap it in an outer `div`.
+ Comment out all of the React child components, or perhaps the most recently added component and save, rebuilding the bundle. If all goes well, the problem is in one of the these components.
+ Check your `import` statements, and verify the syntax is correct. Should it be `{ Button }` instead of `Button` or vice versa? Was the component exported in the `index.js` file? Was it exported in the main component file (e.g. Button.jsx)? Console.log (or set a breakpoint) the imported component to verify it is not `undefined`. This is usually the problem.
+ Are you missing a render `method`?
+ Are you *returning* JSX within your render method?
+ Verify your component is extending React.Component or Component (check the spelling), and verify you have correctly imported both at the top of the file.
+ If you have a `constructor`, are you passing both `props` and `context` into the `constructor` and into `super`?

### `max callstack exceeded`: Im in an infinite loop of death!

Usually this happens because you are *calling* a functions that calls set state in the render method of a component instead of *passing* the function.
```
updateState() {
    this.setState({ stuff: true })
}

render() {
    return (
        <div>
            <div onClick={this.updateState}>stuff</div> // this is ok
            <div onClick={this.updateState()}>stuff</div> // infinite loop of death
        </div>
    );
}

```
### Something about needing a unique `key`?
You need to tell React that each item in array is unique. This is done with a special `prop` called a `key`. You use it like this:

```
myArray.map((item, index) => {
    const key = index; // most of the time setting the key to just the array index works great
    return (<div key={key}></div>);
});
```

If you want to know what you need this, checkout this [link] (https://www.reddit.com/r/reactjs/comments/4mjdcf/why_cant_i_put_key_in_default_props_or_define_the/d3xwa6m/)


### Hot reloading doesn't appear to be working?

Hot reloading won't work on things like ComponentDid / WillMount, initialization (like inside the constructor) or mapStateToProps. When in doubt, just refresh the page.
