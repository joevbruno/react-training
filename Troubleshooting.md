
# Troubleshooting

## React / Redux

+ `Uncaught Error: Invariant Violation: ...A valid ReactComponent must be returned. You may have returned undefined, an array or some other invalid object.`

This errors means there is a problem with one of your React components or *with one of its children*. 

Diagnosing the Problem
1. Comment out all of the React child components, or perhaps the most recently added component and save, rebuilding the bundle. If all goes well, the problem is in one of the these components.
2. Check your `import` statements, and verify the syntax is correct. Should it be `{ Button }` instead of `Button` or vice versa? Was the component exported in the `index.js` file? Was it exported in the main component file (e.g. Button.jsx)? Console.log (or set a breakpoint) the imported component to verify it is not `undefined`. This is usually the problem.
3. Are you missing a render `method`?
4. Are you *returning* JSX within your render method?
5. Verify your component is extending React.Component or Component (check the spelling), and verify you have correctly imported both at the top of the file.
6. If you have a `constructor`, are you passing both `props` and `context` into the `constructor` and into `super`?

+ Hot reloading doesn't appear to be working?

Hot reloading won't work on things like ComponentDid / WillMount, initialization (like inside the constructor) or mapStateToProps. When in doubt, just refresh the page.