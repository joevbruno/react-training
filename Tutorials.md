# Tutorials

Beyond the obvious thing of exploring the links below, it is really helpful to do to the following:
+ Follow [Dan Abromov](https://twitter.com/dan_abramov?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) on Twitter.
+ Follow the [React Meeting Notes](https://github.com/reactjs/core-notes)
+ Subscribe to [React NewsLetter](http://reactjsnewsletter.com/), [Versioning](http://www.newsletterstash.com/newsletter/versioning), and [Javascript Weekly](http://javascriptweekly.com/) (All Free).

## React
Facebook’s JavaScript library for building UIs.  React allows developers to define reusable components that encapsulate their markup and logic.  Markup (JSX) is written with the JavaScript in the same file (we use the .jsx file extension).  There are a few differences between normal HTML and JSX.  One such example is that class isn’t a valid attribute on HTML elements, since class is a keyword in ES6; instead you should use className.  React uses virtual DOM and a diffing algorithm to determine whether updates to the real DOM should be made.  This makes component updates easier (you don’t have to track whether or not something has change) and faster.

+ [React Playground] (http://www.react.run/HyVJ-QHZe/1)
+ [Egghead React Cheatsheet](https://d3cxrxf04bhbwz.cloudfront.net/cheat-sheets/egghead-react-cheat-sheet-0-14-7.pdf)
+ [Facebook Docs](https://facebook.github.io/react/tutorial/tutorial.html)
+ [Facebook Tutorial](https://facebook.github.io/react/docs/hello-world.html)
+ [How to Learn React](http://bkd705.com/how-to-learn-react/)
+ Corey House has some good video tutorials on Pluralsight (2 in fact)
+ Once you feel comfortable with React in general, [this cheatsheet can be helpful](http://reactcheatsheet.com/)
+ [Tons of other tutorials](https://github.com/timarney/react-faq)
+ More Advanced: [Optimizing React](https://medium.com/@alexandereardon/performance-optimisations-for-react-applications-b453c597b191#.gl0vmwozp)
+ More Advanced: [Higher Order Components](http://engineering.blogfoster.com/higher-order-components-theory-and-practice)
+ More Advanced: [Common Patterns](http://reactpatterns.com/)
+ More Advanced: [Safely Using Context](https://medium.com/@mweststrate/how-to-safely-use-react-context-b7e343eff076#.hiwil5484)

https://tylermcginnis.com/react-aha-moments/

### Advaned Examples
+ [HOC using Context to provide theme](https://github.com/styled-components/styled-components/blob/master/src/hoc/withTheme.js)

## Redux
A state container for JavaScript, where the entire state of your application is stored in a single store.  State in the store can only be changed by emitting actions.  Redux can be described using three main principles:
1. Single source of truth – the state of the entire application is stored in a single data store; there aren’t multiple stores with the same data.
2. State is read-only – state can only be changed by emitting actions, it should not be mutated directly.
3. Changes are made with pure functions – the state tree is transformed by action by writing pure functions (called reducers).  Reducers take the previous state and action as input and returns the next state without mutating the previous state.
+ [Redux Cheatsheet](https://d3cxrxf04bhbwz.cloudfront.net/cheat-sheets/egghead-redux-cheat-sheet-3-2-1.pdf)
+ [Redux Docs](http://redux.js.org/)
+ [Intro Redux on Egghead](https://egghead.io/courses/getting-started-with-redux)
+ [Wes' Intro Videos](https://learnredux.com/)
+ [More Advanced Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
+ [Good written article](https://medium.freecodecamp.com/why-redux-makes-sense-to-me-and-how-i-conceptualize-it-c8a3a9db15ca#.a9xwz8uye)

### Redux Middleware
+ [redux-promise](https://www.npmjs.com/package/redux-promise)
+ [redux-thunk](https://github.com/gaearon/redux-thunk)
+ [redux-observable](https://github.com/redux-observable/redux-observable)
+ [redux-saga](https://github.com/yelouafi/redux-saga)
+ [redux-logic - Jeff](https://github.com/jeffbski/redux-logic)
+ [redux-pack - Leland at Airbnb](https://github.com/lelandrichardson/redux-pack)


## React Router
A routing library for React.  It keeps the UI in sync with the browser’s URL and provides features such as lazy code loading, dynamic route matching, and location transition handling.
+ [Router Docs](https://github.com/ReactTraining/react-router)

### React-Router with Redux
+ [Exponent as Example](https://github.com/exponentjs/ex-navigation/blob/aa327307998a60aa82f4b69988b2fd5660f22e4a/src/ExNavigationProvider.js)

## React Toolbox
React UI components that implement Material Design (mostly).
+ [React Toolbox Docs](http://react-toolbox.com/)

## ES6
This is the version of the ECMAScript spec we are currently using.  All browsers do not fully support ES6 at present, so we have to transpile or polyfill so that browsers can run our code.  The only ESNext feature that we are using is currently the object rest operator.  Some important topics to understand in ES6 are – const, let, scoping, arrow functions, template literals, destructuring, modules, classes, and new built-in string, array, number methods.
+ [Lebab](http://lebab.io/try-it) and conversely, Babel [REPL](http://babeljs.io/repl/)
+ [Eric Elliot How To Learn ES6](https://medium.com/javascript-scene/how-to-learn-es6-47d9a1ac2620#.5l5sjzhwk)
+ [Egghead videos](https://egghead.io/courses/learn-es6-ecmascript-2015)
+ [ES6 Guide](https://mrzepinski.gitbooks.io/es6-guide/content/)
+ [ES6 Cheatsheet](https://github.com/DrkSephy/es6-cheatsheet)
+ [You don't know JS](https://github.com/getify/You-Dont-Know-JS/tree/master/es6%20%26%20beyond)
+ [ES6 Features](http://es6-features.org/)
+ [ES6 Gitbook](https://leanpub.com/understandinges6/read)

## NPM
Helps install and manage external dependencies.
+ [Understanding NPM: A Beginner's Guide](https://www.sitepoint.com/beginners-guide-node-package-manager/)


## SCSS
A pre-processor that extends CSS.  Features of SASS include variables, nesting, extending / inheritance, imports, and mixins.
    + [Getting Started with SCSS](https://scotch.io/tutorials/getting-started-with-sass)
    + [Official Docs](http://sass-lang.com/guide)

## CSS
+ [An Awesome Reference You Should Know About](http://cssreference.io/)
+ [In Case You Are Wondering About Architecture](https://www.ckl.io/blog/css-architecture-first-steps/)
+ [On Breakpoints] (https://medium.freecodecamp.com/the-100-correct-way-to-do-css-breakpoints-88d6a5ba1862#.hffjl13bf)
+ [Visual Guide to Flexbox] (http://jonibologna.com/flexbox-cheatsheet/)
+ [Flexbox Known Issues and How to Fix them](https://github.com/philipwalton/flexbugs#1-minimum-content-sizing-of-flex-items-not-honored)

## CSS Modules
+ [Official Docs] (https://github.com/css-modules/css-modules)
+ [Tutorial](https://glenmaddern.com/articles/css-modules)
+ [What are CSS Modules and why do we need them](https://css-tricks.com/css-modules-part-1-need/)
 
## ESLint
Linter that analyzes our front-end code to check for potential errors, best practices, and code consistency. Use an editor Extension!
+ [Docs](http://eslint.org/)
 
## Promises
+ [Visual Guide to Promises](http://bevacqua.github.io/promisees/)
+ [Promises and Async Await](https://medium.com/@bluepnume/learn-about-promises-before-you-start-using-async-await-eb148164a9c8#.8oesasp5o)

## Other Libs
+ [classnames](https://github.com/JedWatson/classnames)

## Material Design
Design spec developed by Google.
 + [Docs](https://material.google.com/)
 + [Colors](https://material.google.com/style/color.html#color-color-palette)
 + [Icons](https://material.io/icons/)

## Hot Reloading / Time Travel
Hot reloading is a development tool that allow us to inject new versions of files that have been edited in the browser without having to reload.  It allows you to keep the state of your application even after code changed.  Time travel allows you to go backwards to previous points in your application’s state.
+ [Docs](https://www.youtube.com/watch?v=xsSnOQynTHs)
 
## Browsersync
Development tool that monitors files and reloads the browser when they change.
+ [Docs](https://www.browsersync.io/)

# Stuff You Probably Don't Need to Know Right Now

## Babel
JavaScript compiler that allows us to use features that browsers don’t support natively, such as ES6 and JSX.  Babel polyfills and transpiles our code into code that browsers can run.
+ [Official Docs](http://babeljs.io/)

## Webpack (v1.x)
A build tool and module bundler that takes modules with dependencies and generates static assets.
+ [Docs](https://webpack.github.io/docs/what-is-webpack.html)
+ [The Confusing Parts](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.qoubedxij)


Here's another pretty good "Learning React" [guide](https://github.com/ericdouglas/react-learning)

## Appendix

### RxJs
+ [RxJs Playground](https://jsfiddle.net/btroncone/pvj1nbLa/)

### Styled Components
+ [Styled components](https://github.com/styled-components/styled-components)
