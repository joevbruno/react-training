# Hello World

Perhaps you're here because you want to learn "React." Perhaps you're here because someone else told you to be here; however, (surprise!)
you're not really going to be learning React - or better yet, you're not going be learning *just* React. In truth, React itself is really very
simple. Seriously - it's 10 functions (called lifecycle methods), 3 key terms (props, state, refs), and two validation properties (propTypes, defaultProps).
That's it. Done. Every component is structured the same. If you know how to read one file, you can ready anyone's code, but React is kind of known for being hard. You will see things like `#JavascriptFatigue` everywhere.
And that right there is the problem: the learning curve tied to React is predominantly tied to two things: 1) the conceptual paradigm (that is, learning to think in components with data flowing one direction instead of two-way data-binding) and 2) the tooling, workflow, and third-party libraries associated with the Javascript community.

The latter part is true of any modern Javascript framework including Angular 2, Aurelia, Vuejs, or Ember to name the most popular (if you are wondering where the industry as a whole is at, see [here](https://ashleynolan.co.uk/blog/frontend-tooling-survey-2016-results)). All of these frameworks require or highly recommend build tools, transpiling, linting, bundling, and testing, and the larger your application (in terms of size, scale, or the more you choose to be opinionated about the implementation details), the more complex the tooling requirements become.

React itself is a very modular framework (and you can see other frameworks following suit. For instance, Angular 1 came with the kitchen sink. Angular 2 is entirely modularized as well with routing, http, and more all being separate packages). One of the initial catch-phrases that followed React closely in its infancy was "Just the V in the View layer (of MVC)";
however, while technically true, the statement not only really misses the [point of React](https://medium.com/@dan_abramov/youre-missing-the-point-of-react-a20e34a51e1a#.h5po9f5wc), but also slightly screws the truth because React doesn't really fit well into a client-side MVC or MVVM framework at all - hence the conceptual paradigm differences noted above.

Similarly, React doesn't really fit well into Object Oriented patterns either. React (along with libraries like Redux) really has it's roots in patterns and paradigms chiefly found in functional
languages (like Elm), and even with the advent of certain ES6 features like classes in Javascript that perhaps make it look more like C#, it's pretty functional through and through and the classes are merely syntactical sugar for functions implementing prototypical inheritance. So let's start writing some functions!

```
function renderButton(label) {
    return "<button>" + label + "</button>";
}
```

We have a super simple function `render`. It has a single argument or property (let's just call it a `prop`), `title`. *It has a return value.* By default, in Javascript, everything has a return value - the value may be `undefined`. We need to ensure that it isn't. Return null or something else.
Currently, this function doesn't utilize or manipulate any value outside of itself. Thus, it is a [`pure function` without `side-effects`](https://egghead.io/lessons/javascript-redux-pure-and-impure-functions). These kinds of functions will be really important later.

We could, of course, pass other arguments / props. We could pass whatever we want - an object, an array, a number. We could pass multiple arguments or we could pass other *functions*. Whatever we need.


```
function renderButton(label) {
    return "<button>" + label + "</button>";
}

function render(label) {
    return "div" + renderButton(label) + "div";
}
```

Notice, that we could say that `renderButton` is in some sense a *child* of it's parent: `render`. Thus, we have some sense of hierarchy with each level handling its own stuff.
In fact, we could think of each function as a piece of our UI. *It is a component.* In fact, every piece of UI can be comprised of a single component or a group of components (and thus a hierarchy), which ultimately have a single parent.

Components don't have to always return UI either. A component just needs to perform a function. That function could be checking permissions or getting data from an API, and then returning whatever components were passed to it!

This kind of pure functional composition will get us a long way, but chances are we are going to need someplace to store *stateful* data about our UI too.
Is there an error? Is a drawer open? Is that thingy being shown?

```
var state = { 
    showMessage: false
};

function renderButton(label) {
    return "<button>" + label + "</button>";
}

function render(label) {
    return "div" + renderButton(label) + "div";
}
```

OK, maybe something like that, but we need some way to connect what is happening in the render functions with our state. More specifically,
we need to connect something like our `onClick` button event with a function that will alter the `state`. Something like this should do:

```
var state = { 
    showMessage: false
};

function toggleMessage() {
    state.showMessage = true;
}

function renderButton(label) {
    return "<button onClick="toggleMessage">" + label + "</button>";
}

function render(label) {
    return "div" + renderButton(label) + "div";
}
```

So how could we make this better or more specifically, how could we make this more pure? Let's pass our `toggleMessage` event handler into
our `renderButton` function along with state.


```
var state = { 
    showMessage: false
};

function toggleMessage(state) {
    state.showMessage = true;
}

function renderButton(label, toggleMessage, state) {
    return "<button onClick='toggleMessage(state)'>" + label + "</button>";
}

function render(label, state) {
    return "div" + renderButton(label, state) + "div";
}

render("My Button", state);
```

Notice how we have a tendency to pass arguments or props *down*, but events will actually call functions that were passed down,
effectively causing things to travel back *up* to their original point of origin and context.

So to recap, here's more or less what we've covered:

+ *Component:* a function or class that takes props as input and returns the elements as output
+ *Props:* immutable attributes or arguments passed to a React component
+ *State:* mutable attributes or values available to the React component.
+ *`render`:* the fundamental method of a React component, which will return a string of html. Every React component must have one, and it cannot return undefined.

Congratulations, you now know about 80% of React even though you haven't written any yet! 

Here's the other 20% we haven't really talked about:

1. Lifecycle methods. Every *component* (which is just a function) in React has a lifecylce. You can do stuff initially. You can do stuff when the component mounts.
You can do stuff when the component updates. You can do stuff right before the component is destroyed. They are just hooks into different stages of the component's life.
One of these method's - the most important, and the only one that is required is called `render`, and it works exactly like the examples above. Here's a few:

+ `constructor`
+ `componentWillMount`
+ `render`
+ `componentDidMount`
+ `componentWillReceiveProps`
+ `componentDidUpdate`
+ `shouldComponentUpdate`
+ `setState`
+ `componentWillUnmount`

The name of the method pretty well communicates its purpose.

2. There is this thing called [refs](https://facebook.github.io/react/docs/refs-and-the-dom.html) in React. You won't use them often, but they serve as a hook to grab the actual DOM. In general, you don't want to do this.
React is all about the [*virtual DOM*](https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060#.mj0uvfojd). In other words, in creates an in-memory copy of the DOM, runs a diff algorithm comparing the actual DOM to the in-memory copy, and updates
the real DOM behind the scenes in the most optimized and optimistic fashion possible. Directly manipulating the DOM is expensive. Ultimately, React has to use the same underlying DOM APIs as any other framework to do this;
thus, in a small app, it is often more performant to directly manipulate the DOM; however, at scale, particularly in cases will large number of repeating component structures, such as lists, rows, or cards,
React's virtual DOM shows it's true benefits. Also note, you can only grab the actual DOM using a ref at certain times and in certain places, such as in `componentDidMount` or in an event handler.

If you want to keep learning more React without React, you can check out this [tutorial](https://medium.com/javascript-inside/learn-the-concepts-part-1-418952d968cb#.k4ww3ymln)


## Summary
So let's recap what we've learned so far.
+ React is all about pure functions that return stuff
+ React likes separate functions for distinct pieces of functionality. In other words, it likes to be modular. It likes components.
+ Functions can and often will be hierarchical with parent - child relationships
+ You will often pass arguments / properties (props) down the hierarchy to the children.
+ You can pass anything you want - functions (other components), arrays, objects, numbers, strings. Ultimately in react, these options/functions, arrays, numbers, etc will all be part of a single object called `props` that is passed down to the child component.
+ `Props` that are strings will often be things like HTML classes (no jQuery here folks) or text that does between HTML tags.
+ React components can have state - which usually consists of things like: are there errors? is the form dirty? Is the message shown? or form data.
+ React components must have a render function / method (or consist only of a functions - called stateless functional syntax, which we are not currently using).
+ Anytime you want to do something (say, on the `onClick` event), you will do this with an event handler, which is just a function, that usually was passed down to the component. More on this next.
+ Don't touch the real DOM often (when you need to, you will use `refs`). When you do, be extremely careful, and only do things like measure the width or bounding client rectangle. Avoid manipulating the DOM directly.

Well, that's it. That *is* React. **The hardest part about React is learning to think about things in a new way**. No more two way data-binding. 
Every thing flows one way and everything is explicit and declarative, and if you want to update an input, you actually need to do it (using the `onChange` event handler). It won't automatically happen.
We can almost start writing some real React, but first, say hello to JSX.


## JSX

Truly static HTML doesn't work for most applications. We need dynamic elements. Most Javascript frameworks attempt to solve this problem by introducing
a **DSL** into HTML to make it more dynamic. In other words, *we're writing Javascript as strings in our HTML*. Knockout, Angular, Vue -- all follow this pattern.

React does exactly the opposite. There are no HTML files. Instead, your HTML is written along side and as a part of your Javascript functions. This is extremely
powerful, but may be feel philosophically wrong at first. It will definitely feel different, and you may have an objection that we're not separating concerns; however, the *concern* is the 
specific area of functionality. It is the component, not the language, and that component takes care of all of its own concerns in the way of functionality (Javascript), content (Markup), and presentation (CSS).

So here's JSX:

```
<button id="test" className="my-class">My Button</button>
<Button id="test" className="my-class">My Other Button</button>
``` 

Both buttons are using JSX. Not bad right? A couple of things to note:
+ Remember, this is Javascript. You're writing your HTML (as JSX) in a `.jsx` or `.js` file not a `.html`. `class` is a keyword in Javascript, so instead we need to use `className`. HTML attributes that are two words (like SVG's stroke-width) are written using camelCase (strokeWidth because stroke-width isn't a valid way to write a Javascript variable)
+ `id`, `className` and any other HTML attribute are `props`. In other words, they form an `object` of options that is passed to the function that creates the button element.
+ `<button>` and `<Button>` are different. Elements that begin with a lowercase word are treated as HTML elements. Capitalized elements are treated as React components.
+ JSX can appear anywhere inside of a .js or .jsx file, not just the `render` method - though any method that returns actual JSX usually beings with `render` (like `renderButton`), but only as a matter of preference and communication. It isn't a requirement.
+ You can add event handlers directly to JSX elements:  `<button onClick={this.handleOnClick}>My Button</button>`. A listener is automatically created.
+ `{` in JSX means, "I want to write Javascript here". Any singular Javascript expression will work. That means that `if` won't work, but `ternary` expressions will or if you have a callback, you can use `if` inside of the callback.
+ if you see double mustaches like this `{{ }}`, the outer set is saying, "This is Javascript", and the inner set is the standard outer brackets of a Javascript object.

If you really want to see what JSX is actually transpiling to (we do automatically using [Babel](https://babeljs.io/)), you can see that [here](https://jsx-live.now.sh/)

We're almost ready to see it action, but the last thing to briefly cover before writing our first real React component is ES6.

## ES6 Intro
[Ecmascript](https://www.ecma-international.org/publications/standards/Ecma-262.htm) is the official version of Javascript. Beginning in 2015, they made the decision to release yearly upgrades to the Javascript language itself.
It had been **YEARS** since Javascript had any real changes. Thus, ES6 or ES2015 (same thing) came STUFFED with tons of new features. Since we are now past 2015, there has been another official release (ES7 or ES2016) and ES8 is due in Jan. 2017;
however, these latter two releases only include a handful of changes. ES2015 contains over 20. It's important to recognize that these changes are not part of a library.
This is official Javascript, and is the recommended way of writing Javascript going forward. Also, we don't really need to worry about Browser compatibility because Babel's transpiling it down to ES5 or ES3. That means we could support all the way back to IE6 or IE8 if we had to do so.
In other words, we can freely write modern or even futuristic Javascript now without worry about Browser compatibility or running of to [CanIUse](http://caniuse.com/).

Finally, let me say that if you are wondering, "How do I write this in ES6?", checkout [Lebab](http://lebab.io/try-it) and conversely, the Babel [REPL](http://babeljs.io/repl/). These are great places to play.

Now on to the good stuff:


1. Let and Const: Don't use `var`. Use `let` if the value or type is going to change and `const` if it isn't. Const is not immutable; however, certain values in Javascript (Strings and numbers) are by definition inherently immutable in Javascript.
Thus, `const` effectively is immutable in those cases; however, in the case of `Objects` and `Arrays`, `const` merely states that they type is not going to change. These key words also do effect scope slightly differently than that traditional `var` keyword.

2. Arrow Functions: In general, don't write your anonymous functions like this, `function() {  // do stuff }`. You can now do this: `() => { // do stuff }`
    ```
    const x = [1, 2, 3];
    const y = x.map(number => number + 1)
    // y = [2, 3, 4]
    // like Lambda in C#
    // automatically returns value
    // `this` is lexically bound to the parent scope
    // no parens necessary around 'number'
    ```

3. Object Spread: Works with Arrays and Objects. Think of it like "throw all of this into that -- flatly";

    ```
        const a = [1, 2, 3]
        const b = [4]
        const c = [...a, ...b]; // [1, 2, 3, 4] throw all of a and b flatly into c;

        const x = { a: true, c: false }
        const y = { b: false, c: true }
        const z = { ...x, ...y } // { a: true, b: true, c: true } throw all of x and y flatly into z. If any keys are the same, the latter ones will override the earlier ones 
    ```

4. Deconstruction: Works on Arrays and Objects. Think of it like "Give me this out that"

    ```
    const a = [1, 2, 3, 4]
    const [firstNumber, secondNumber, ...others] = a;
    // firstNumber = 1, secondNumber = 2, others = [3, 4]

    const x = { a: true, c: false }
    const { a, c } = x;
    // a = true, c = false - you are pulling a and c out of x and assigning the value to variables named exactly the same thing: a and c.
    ```

5. Shorthands

    ```
    {
        x: function() {
        // this way can be shortened
        },
        y: function() {
            // notice the comma separating the functions
        }
    }

    // shorter

    {
        x() {
        // no function or colon required
        }

        y() {
            // notice no comma separating the functions
        }
    }

    // shorter variable names:

    const x = true;
    const y = { x }; // y = { x: true }
    // this shorthand means create a new object that has a key of x and the value there is the value of x.

    // you can combine this with other ES6+ features like the spread.
    const z = { a: true, b: false }
    const y = { x, ...z } // { x: true, a: true, b: false }
    ```

6. Imports and Exports: We don't really write Javascript in the global scope anymore. Every file is a module. Everything by default within that file is `private`; however, we can make functions or variables as exportable with `export`;
   Any exported values can be imported (using `import`) by other modules (files).

   ```
   // file1.js
   const x = true; // private
   export const y = false // public
   export default function myReducer() { // public default export
       // do stuff
   } 


   // file2.js
   import { y } from "./file1.js"; // y = false - uses deconstruction
   import myReducer from "./file1.js" // default export doesn't need deconstruction
   import * as allExports from "./file1.js" // { y: false, default: myReducer() { } }

   // all of the above could appear on the same line
   import myReducer, { y } from "./file1.js"
   ``` 
   // quick note: notice the "." to start "./file1.js". This is required for local files. Without it, the compiler will look for the file in a special folder called `node_modules`, which is where all third-party libraries live.

7. Classes: ES6 introduced the `class` keyword. They are not like classes in C#. It's just syntatical sugar for `prototypical inheritance`. Ultimately, a `class` is still just a `function`, which is an `object`;

8. Template Strings
```
const foo = "test";
const bar = `This is a ${foo}`;
// bar = This is a test

```

You should also be aware of [Promises](https://coligo.io/javascript-promises-plain-simple/), and be sure to checkout the [Tutorials](https://github.com/joevbruno/react-training/blob/master/Tutorials.md) page here for other helpful links on promises.

There is a lot of other awesome features in [ES6+](https://github.com/joevbruno/react-training/blob/master/Tutorials.md), but these will get you through most of the stuff in our codebase. Also, you can use any of the standardized ES6+ plus features,
without worrying about things like browser compatibility. Since everything, including the JSX, is transpiled to ES5 (or even IE6 compatible ES3), we can use anything we want anywhere.
This is all thanks to Babel.

## On to React
We've covered a lot so far. React without React. JSX. ES6. Now it is time for React with a little ES6 sprinkling.

```
class MyButton extends Component {
    render() {
        return (
            <button>My Button</button>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton /> 
```

OK, now let's make this more dynamic by passing `props`;


```
class MyButton extends Component {
    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <button>{title}</button>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```


In React, there is also a special `prop` called `children`. Children is a generic term for anything between the component's opening and closing tags. It looks like this:
 
 ```
 <Button onClick={this.showMessageOnClick}>{children}</Button>
```
Since `children` is present between the opening and closing `Button` tags, anything we could write something like this:

 
 ```
 <Button>
    <h1>Hello</h1>
    <h5>Helllloooo</ht>
</Button>
```

Anything you want, that is valid HTML nesting, can go in the place of `children`. Child is just a prop.


`props` AKA, arguments, AKA options, AKA the component API AKA ... you get the point... is always available on `this` at `this.props`;
`state` is available at `this.state`. Let's see how we could add state.

```
class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = {};
    }
    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <button>{title}</button>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```

State is always initialized in the constructor. Let's see how we could manipulate it.

```
class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <button onClick={this.showMessageOnClick}>{title}</button>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```
`onClick` calls our helper method `showMessageOnClick`, which calls a React method `setState`. You never reassign state directly (`this.state.showMessage = true`) because 
`setState` will batch updates and re-renders asynchronously, so bad things can happen when you do assign to the state directly. Don't.

Now, we aren't showing a message yet, let's see how we could do this:


```
class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    render() {
        const { showMessage } = this.state;
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        const message = (showMessage) ? (<h1>Hi!</h1>) : null;
        
        return (
            <div>
                {message}
                <button onClick={this.showMessageOnClick}>{title}</button>
            </div>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```

`render` can only return a single Element. So since we have two now - message and `button` - we need to wrap them in a `div` or something (`section`, `span`, `aside`, something).
React components can return `null`, markup or other components. They **cannot** return `undefined`, an `array`, `string` or `object`.
Notice the `{` around `message` (`{message}`). This means we are saying, "JSX, treat this as Javascript - in this case a JS variable".

Like so many other things, there is often more than one way to do things. Here are a couple of other variations of the same thing.

```
class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    renderMessage() {
        const { showMessage } = this.state;

        return (showMessage) ? (<h1>Hi!</h1>) : null;
    }

    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <div>
                {this.renderMessage()}
                <button onClick={this.showMessageOnClick}>{title}</button>
            </div>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```

or - and this one contains two changes: 

```
class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    renderMessage() {
        const { showMessage } = this.state;

        if (showMessage) {
            return (<h1>Hi!</h1>);
        }

        return null; // always return something when the possible return value is JSX.
    }

    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <div>
                {this.renderMessage()}
                <button onClick={() => this.showMessageOnClick()}>{title}</button>
            </div>
        );
    }
} 

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```

So we've covered most of React now. Actual React. Two quick things: 1) PropTypes and 2) Imports.

Javascript is not a strictly typed language. While you could use something like Flow or Typescript for this, React does provide a generic way to run type validations on `props`: `PropTypes`;
Here's what that looks like:


```
const propTypes = {
    title: PropTypes.string
};

class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    renderMessage() {
        const { showMessage } = this.state;

        if (showMessage) {
            return (<h1>Hi!</h1>);
        }

        return null; // always return something when the possible return value is JSX.
    }

    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <div>
                {this.renderMessage()}
                <button onClick={() => this.showMessageOnClick()}>{title}</button>
            </div>
        );
    }
} 

MyButton.propTypes = propTypes;

// Now we could use MyButton like this (notice it's self closing by default):
<MyButton title="My Button" /> 
or <MyButton title={"My Button"} /> 
```

This provides not only some basic validation, but a typed, self-documented API declaration at the top of every React component / file. In other words, if you want to known how to use a component,
just look at the the propTypes at the top of the file!

Second, we need to import some variables. At the top of the component, we are declaring that the MyButton function / class is extending `Component`. We don't keep things in the global scope anymore,
so `Component` needs to be imported. So does `PropTypes`. Also, any file containing JSX also need to have `React` in scope. So let's import all that and export our component (see the bottom).

```
import React, { PropTypes, Components } from "react";

const propTypes = {
    title: PropTypes.string
};

class MyButton extends Component {
    constructor(props, context) {
        super(props, context);
        this.state = { 
            showMessage: true
        };
    }

    showMessageOnClick() {
        this.setState({ showMessage: false })
    }

    renderMessage() {
        const { showMessage } = this.state;

        if (showMessage) {
            return (<h1>Hi!</h1>);
        }

        return null; // always return something when the possible return value is JSX.
    }

    render() {
        const { title } = this.props; // using deconstruction = const title = this.props.title  - props is always an object containing any passed options / arguments
        
        return (
            <div>
                {this.renderMessage()}
                <button onClick={() => this.showMessageOnClick()}>{title}</button>
            </div>
        );
    }
} 

MyButton.propTypes = propTypes;

export default MyButton; // we can now import and use MyButton in any component we want.
```

That is a complete React component, and most components will look very, very similar to this! Don't believe me? Here's a complete one:


This is the layout that the majority of your react components will follow:


```
import React, { PropTypes, Component } from "react"
import { autobind } from "HOCs";
import styles from "./MyComponent.scss";

// provides type validation
const propTypes = {
    title: PropTypes.string
};

const defaultProps = {
    title: "My Title"
};

class MyComponent extends Component { // es6 class
    constructor(props, context) {
        super(props, context);
        this.state = { active: false };
        autobind(this);
    }
    onTestClick() { // enhanced object literal 
        console.log("Clicked", this.state, this.props);
        const active = true;

        this.setState({ active }) // shorthand 
    }
    render() {
        const { active } = this.state; // deconstruction
        const { title } = this.props;
        const displayText = `${title} is active: ${active}`; // template string

        return (
            <div
                className={styles.container}
                onClick={this.onTestClick}
            >
                {displayText}
            </div>
        );
    }
}

MyComponent.propTypes = propTypes;
MyComponent.defaultProps = defaultProps;

export default MyComponent;

``` 

So how do we get React? In other words, how does `import React from "react"` work? Where does "react" come from? Answer: [NPM](https://www.npmjs.com/).

When you install [Node.js](https://nodejs.org/en/) - version 4+ or higher (and preferably using something like [NVM](https://github.com/coreybutler/nvm-windows)), Node includes NPM (originally named Node Package Manager). Most of the time it is as simple as running `npm install` in the terminal / powershell at your project's root directory. This will install all of the third party libraries listed in your `package.json` file into the project in a directory called `node_modules`. On Windows, you will want to make sure you are aware of a couple of things:
    1. You NEED NPM v3+ (`npm -v`) or you will run into MAX_PATH issues
    2. [use `rimraf` to delete node_modules](https://github.com/isaacs/rimraf), don't attempt to delete them using the delete button or you will regret it.
    
Lastly with regard to NPM, the `package.json` not only can contain information about which packages your project is using (and what versions), it also can contain configuration information and scripts to run.

For instance, you can run `npm start` at the root of almost any Node Project containing a `package.json` file and something will start running. In most cases, this starts the default development environment.

## Final words

There is an industry standard linter for modern Javascript called ESLint. It will enforce writing code a certain way. You will probably hate it at first, but it will make reading everyone's code consistent, avoid bugs, and actually will catch a lot of syntax errors for you.
Second, when diving through tutorials, you will see a ton on Webpack and Babel. You should learn this stuff, but you won't need to interact with it in G2 (at least not much) because all of this has already been set up for you. So put this on the back-burner; however, in G2 you will see references to `Controls`, `Utilities` and `HOCs`. These are webpack aliases to certain files. 

Lastly, if you are accustomed to writing `use strict` at the top of your Javascript files, that is no longer necessary. When Babel transpiles code, it automatically inserts a `use strict` statement at the top of every module / file.

Now, we all know Rome wasn't built in a day. There's still a long road ahead. So go back to the main page and continue moving on to the next section! But first, a coffee break...

Happy hacking.













