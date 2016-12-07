# Hello World

Perhaps you're here because you want to learn "React." Perhaps you're here because you're someone else told you to be here; however, (surprise!),
you're not really going to be learning React - or better yet, you're not going be learning *just* React. In truth, React itself is really very
simple. Seriously - it's 10 functions(called lifecycle methods), 3 key terms (props, state, refs), and two validation properties (propTypes, defaultProps).
That's it. Done. Every component is structured the same. If you know how to read one file, you can ready anyone's code, but React is kind of known for being hard. You will see things like #JavascriptFatigue everywhere.
And that right there is the problem: the learning curve tied to React is predominantly tied to two things: 1) the conceptual paradigm and 2) the tooling, workflow, and third-party libraries.

The latter part is true of any modern Javascript framework including Angular 2, Aurelia, Vuejs, or Ember to name the most popular (if you are wondering where the industry as a whole is at, see [here](https://ashleynolan.co.uk/blog/frontend-tooling-survey-2016-results) and here). All of these frameworks require or highly recommend
build tools, transpiling, linting, bundling, and testing and the more custom your application or the bigger your application, the more customization is required.


React itself is very modular framework. One of the initial catch-phrases that followed it closely was "Just the V in the View layer (of MVC)";
however, while technically true, it really misses the [point of React](https://medium.com/@dan_abramov/youre-missing-the-point-of-react-a20e34a51e1a#.h5po9f5wc) and React doesn't really fit well into a client-side MVC framework at all - hence the conceptual paradigm differences noted above.
Similarly, React doesn't really fit well into Object Oriented patterns either. React (along with libraries like Redux) really has it's roots patterns chiefly found in functional
languages. So let's start writing some functions!

```
function renderButton(label) {
    return "<button>" + label + "</button>";
}
```

We have a super simple function `render`. It has a single argument or property (let's just call it a prop), `title`. It has a return value.
Currently, this function doesn't utilize or manipulate any value outside of itself. Thus, it is a pure function without side-effects. These kinds of functions will be really important later.

We could, of course, pass other arguments / props. We could pass whatever we want - an object, an array, a number. We could pass multiple arguments or we could pass other *functions*. Whatever we need.


```
function renderButton(label) {
    return "<button>" + label + "</button>";
}

function render(label) {
    return "div" + renderButton(label) + "div";
}
```

Notice, that would say that `renderButton` is in some sense a *child* of it's parent: `render`. Thus, we have some sense of hierarchy with each level handling its own stuff.
In fact, we could think of each function as a piece of our UI. It is a component.
This kind of pure functional composition will get us a long way, but chances are we are going to need someplace to store *stateful* data about our UI.
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

Yay, maybe something like that, but we need some way to connect what is happening in the render functions with our state. More specifically,
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

Congratulations, you now know about 80% of React even though you haven't written any yet! 
Here's a high level summary:

+ *Component:* a function or class that takes props as input and returns the elements as output
+ *Props:* immutable attributes or arguments passed to a React component
+ *State:* mutable attributes or values available to the React component.
+ *`render`:* the fundamental method of a React component, which will return a string of html. Every React component must have one, and it cannot return undefined or null.


So here's the other 20% we haven't really talked about:

1. Lifecycle methods. Every *component* (which is just a function) in React has a lifecylce. You can do stuff initially. You can do stuff when the component mounts.
You can do stuff when the component updates. You can do stuff right before the component is destroyed. They are just hooks into different stages of the component's life.
One of these method's - the most important, and the only one that is required is called `render` and it works exactly like the examples above.

2. There is this thing called refs in React. You won't use them often, but they serve as a hook to grab the actual DOM. In general, you don't want to do this.
React is all about the *virtual DOM*. In other words, in creates an in-memory copy of the DOM, runs a diff algorithm comparing the actual DOM to the in-memory copy, and updates
the real DOM behind the scenes in the most optimized and optimistic fashion possible. Directly manipulating the DOM is expensive. Ultimately, React has to use the same underlying DOM apis as any other framework to do this;
thus, in a small app, it is often more performant to directly manipulate the DOM; however, at scale, particularly in cases will large number of repeating component structures, such as lists, rows, or cards,
React's virtual DOM shows it's true benefits.

If you want to keep learning more React without React, you can check out this [tutorial](https://medium.com/javascript-inside/learn-the-concepts-part-1-418952d968cb#.k4ww3ymln)


## Summary
So let's recap what we've learned so far.
+ React is all about pure functions that return stuff
+ React likes separate functions for distinct pieces of functionality. In other words, it likes to be modular. It likes components.
+ Functions can and often will be hierarchical with parent - child relationships
+ You will often pass arguments / properties (props) down the hierarchy true to the children.
+ You can pass anything you want - functions (other components), arrays, objects, numbers, strings.
+ These strings will often be things like HTML classes (no jQuery here folks) or text that does between HTML tags.
+ React components can have state - things, which usually consists of things like: are there errors? is the form dirty? Is the message shown?
+ React components must have a render function / method (or consist only of a functions - called stateless functional syntax, which we are currently not using).
+ Anytime you want to do something (say, on the `onClick` event), you will do this with an event handler, which is just a function, that usually was passed down to the component. More on this next.
+ Don't touch the real DOM often (when you need to, you will use refs). When you do, be extremely careful, and only do things like measure the width or bounding client rectangle. Avoid manipulating the DOM directly.

Well, that's it. That *is* React. **The hardest part about React is learning to think about things in a new way**. No more two way data-binding. 
Every thing flows one way and everything is explicit and declarative, and if you want to update an input, you need to do it.
We can almost starting writing some real React, but first, say hello to JSX.


## JSX

Truly static HTML doesn't work for most applications. We need dynamic elements. Most Javascript frameworks attempt to solve this problem by introducing
a DSL into HTML to make it more dynamic. In other words, *we're writing Javascript as strings in our HTML*. Knockout, Angular, Vue -- all follow this pattern.

React does exactly the opposite. There are no HTML files. Instead, your HTML is written along side and as a part of your Javascript functions. This is extremely
powerful, but may be feel philosophically wrong at first. It will definitely feel different, and it may have an objection that like we're not separating concerns; however, the *concern* is the 
specific area of functionality. It is the component, not the language, and that component takes care of all of its own concerns in the way of functionality (Javascript), content (Markup), and presentation (CSS).

So here's JSX:

```
<button id="test" className="my-class">My Button</button>
<Button id="test" className="my-class">My Other Button</button>
``` 

Both buttons are using JSX. Not bad right? A couple of things to note:
+ Remember, this is Javascript. `class` is a keyword, so instead we need to use `className`. HTML attributes that are two words (like SVG's stroke-width) are written as camelCase (strokeWidth because stroke-width isn't a valid way to write a Javascript variable)
+ `id`, `className` and any other HTML attribute are `props`. In other words, they form an object of options that is passed to the function that creates the button element.
+ `<button>` and `<Button>` are different. Elements that begin with a lowercase word are treated as HTML elements. Capitalized elements are treated as React components.
+ JSX can appear anywhere inside of a .js or .jsx file.
+ You can add event handlers directly to JSX elements:  `<button onClick={this.handleOnClick}>My Button</button>`
+ `{` in JSX means, "I want to write Javascript here". Any singular Javascript expression will work. That means that `if` won't work, but `ternary` expressions will or if you have a callback, you can use `if` inside of the callback.

If you really want to see what this is all transpiling to, you can [here](https://jsx-live.now.sh/)

We're almost ready to see it action, but the last thing to briefly cover before writing our first real React component is ES6.

## ES6 Intro
Ecmascript is the official version of Javascript. Beginning in 2015, they made the decision to release yearly upgrades to the Javascript language itself.
It had been YEARS since Javascript had any real changes. Thus, ES6 or ES2015 (same thing) came STUFFED with tons of new features. Since we are now past 2015, there has been another offical release(ES7 or ES2016) and ES8 is due in Jan. 2017;
however, these latter two releases only include a handful of changes. ES2015 contains over 20. It's important to recognize that these changes are not part of a library.
This is official Javascript, and is the recommended way of writing Javascript going forward. Here are a few of the new features or syntax differences you should be aware of:

First, let me say that if you are wondering, "How do I write this in ES6?", checkout [Lebab](http://lebab.io/try-it) and conversely, the Babel [REPL](http://babeljs.io/repl/). These are great places to play.
Now on to the good stuff:


1. Let and Const: Don't use `var`. Use `let` if the value or type is going to change and `const` if it isn't.
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
6. Imports and Exports: We don't really write Javascript in the global scope anymore. Every file is a module. Everything by default within that file is private; however, we can make functions or variables as exportable with `export`;
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

You should also be aware of [Promises](https://coligo.io/javascript-promises-plain-simple/).


Guides and Playgrounds:
+ [Formidable Labs Playground](http://stack.formidable.com/es6-interactive-guide/#/spread-operator)
+ [Eric Elliot's How to Learn Es6](https://medium.com/javascript-scene/how-to-learn-es6-47d9a1ac2620#.wihmp8mnl)
+ [Egghead videos](https://egghead.io/courses/learn-es6-ecmascript-2015)

There is a lot of other awesome features in ES6+, but these will get you threw most of the stuff our codebase. Also, you can use any of the standardized ES6+ plus features,
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

`render` can only return a single Element. So since we have two now - message and `button` - we need to wrap them in a `div` or something.
React components can return `null`, markup or other components. They cannot return `undefined`, an `array`, `string` or `object`.
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

So we've covered most of React now. Actual React. Two quick things: 1) PropTypes and 2) Imports
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


This is layout that the majority of your react components:


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

So how do we get React? In other words, how does `import React from "react"` work? Where does "react" come from? Answer: [NPM](https://www.npmjs.com/). When you install [Node.js](https://nodejs.org/en/) - version 4+ or higher (and preferably using something like [NVM](https://github.com/coreybutler/nvm-windows)), Node includes NPM (originally named Node Package Manager). Most of the time it is as simple as running `npm install` in the terminal / powershell at your project's root directory. This will install all of the third party libraries
listed in your `package.json` file into the project in a directory called `node_modules`. On Windows, you will want to make sure you are aware of a couple of things:
    1. You NEED NPM v3+ (`npm -v`) or you will run into MAX_PATH issues
    2. [use `rimraf` to delete node_modules](https://github.com/isaacs/rimraf), don't attempt to delete them using the delete button or you will regret it.
    
Lastly with regard to NPM, the `package.json` not only can contain information about which packages your project is using (and what versions), it also can contain configuration information and scripts to run. For instance, you can run `npm start` at the root of almost any Node Project containing
a `package.json` file and something will start running. For G2, as in most apps, this starts the default development environment.

## Final words

There is an industry standard linter for modern Javascript called ESLint. It will enforce writing code a certain way. You will probably hate it at first, but it will make reading everyone's code consistent.
Second, when diving through tutorials, you will see a ton on Webpack and Babel. You should learn this stuff, but you won't need to interact with it in G2 (at least not much) because all of this has already been set up for you.

Now, we all know Rome wasn't built in a day. There's still a long road ahead. Here's where you should go from here (and read the entire list before clicking on anything):

Happy hacking.













