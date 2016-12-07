
#Tips and Tricks
Here are some tips and best practices I have found helpful over the last couple of years.
They are not requirements. They are tips, and they assume you know the basics of at least react to appreciate them.

## Use the Devtools (Browser Extensions) and your life will be better!
+ [React Devtools](https://github.com/facebook/react-devtools)
+ [Redux Devtools](https://github.com/gaearon/redux-devtools)

## JSX comments
In JSX, `//` style comments won't work the majority of the time. Instead, write your comments like this:
```
    <div>
        {/* my comment does here */}
        <button>Click Me<button>
    </div>
```

## Snippets, Snippets, Snippets!
React is consistent. Every file has the same basic structure. Create or use a snippet to automatically generate the repeating elements.
It will be a HUGE time saver!

## Keep your render method simple

If your component consists of additional helper methods beyond simply a render method, 
its helpful to split out functionality into separate 'render' (i.e., 'renderButton') methods or components.

## Deconstruct state and props at the beginning of any method using them

Props and state are at the heart of React. Consequently, their usage is repetitive, and has a tendency to grow.
Begin each method with a variable deconstruction statement, grabbing the needed props or state -- even if there is only one.
This not only helps with consistent variable naming (because the deconstructed name is the prop name), it also allows for better scalability
because you can easily add additional props or items from state as needed.

```
renderTitle() {
    // good
    const { name, title } = this.props;
    const { isActive } = this.state;
    const titleComp = (isActive) ? (<h5>{title}</h5>) : null;

    return (
        <div>
            <h1>{name}</h1>
            {titleComp}
        </div>
    );
}

// bad
renderTitle() {
    const titleComp = (this.state.isActive) ? (<h5>{this.props.title}</h5>) : null;

    return (
        <div>
            <h1>{this.props.name}</h1>
            {titleComp}
        </div>
    );
}
```

## Assign any conditional components to variables outside of the JSX, rather than 'cluttering' the JSX with conditionals.

See `titleComp` in the above example.

## Use Array .map and similar methods rather than (for) loops
It is less code, its immutable, and its better aligned as far as paradigm goes with React's functional bent.
Learn the Javascript [array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) methods (like filter, sort, etc).

```

// ehh
var rows = [];
for (var i=0; i < numrows.length; i++) {
    rows.push(<ObjectRow />);
}
return <tbody>{rows}</tbody>;

// yes

const rows = numrows.map(row => (<ObjectRow {...row} />));
return <tbody>{rows}</tbody>;
```


## Debugging

If you work with Webpack and Chrome debugger, you can add the following to removed npm dependencies from teh call stack and when blackbox list:

`^webpack:///\./~/.*/.*\.js$`


## Flexbox

In most cases, `margin: auto;` on flex-items does teh same thing as `justify-content: center; align-items: center`

## Redux
Redux actions are the place for side-effects. Here, you make API calls for JSON or would do things like filtering or sorting.
Thus, if you need to massage the parameters for an API call or data from an API call before setting into the Redux store, you would do that here.

Also, Redux is where you store data (the stuff you retrieved from your API call). That usually shouldn't be in component state.