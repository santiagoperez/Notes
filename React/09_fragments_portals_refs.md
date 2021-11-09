## 9 Fragments, portals and refs

Module content

1. JSX Limitation & Fragments

1. Getting a cleaner DOM with Portals

1. Working with Refs

### 9.1 JSX Limitations & Fragments

JSX has certain limitations in React. One of those is the fact that we cannot return two JSX elements next to each other without being contained in a main JSX.

The typical workaround to overcome this is to wrap all our returning elements in a *div* (or another analog component). We can also contain our elements in an array and return it. If we do it like that, we need to add a key to each element in the array.

Wrapping elements inside div end up complicating our code unnecessarily which add no semantic meaning, just because it is a JSX Requirement. 

Another workaround is to create a Wrapper component which just returns props and use this as a root.

```js
//main.js
return <Wrapper>
    <h2>Title</h2>
    <p>Body</p>
</Wrapper>

//Wrapper.js
const Wrapper = (props) => {
    return props.children;
}

```

This wrapper component is already built-in, it's called **Fragment** component. It is an empty wrapper component; it doesn't render any real HTML element to the DOM. But it fulfills React's/JSX requirement. It is represented with either ```<React.Fragment>``` or ```<>```.

### 9.2 Getting a cleaner DOM with Portals

Portals also contribute to clean our DOM when we compose many React elements.

For example, if we have a Fragment with 2 react components where one of them is a modal we want to show to the front:

```js
return (
    <React.Fragment>
        <MyModal />
        <MyInputForm />
    </React.Fragment>
);
)
```

this is translated to:

```html
<section>
    <h2>Some other content ... </h2>
    <div class="my-modal">
        <h2>A Modal Title!</h2>
    </div>
    <form>
        <label>Username</label>
        <input type="text" />
    </form>
</section>
```

Semantically and from a "clean HTML structure" perspective, having this nested modal isn't ideal. It is an overlay to the entire page after all (that's similar for side-drawers, other dialogs, etc). We want the modal to appear over the rest, and this looks inner in our regular HTML code.

In order to overcome this, we can use **Portals**. Portals allow us to bring up the components we want to show over the rest instead of being "embedded" in the DOM element. For example having an error message as a child of the body. 

Portals lie in **ReactDOM** library. CreatePortal needs 2 arguments, (1) the component we want to port, and (2) the id where this component will be ported.

```js
{ReactDOM.createPortal(<Backdrop onConfirm={props.onConfirm}/>, document.getElementById('backdrop-root'))}

//Where backdrop is a new component containing the layout/style/warning. backdrop-root is a new div we have created at the top of the main page

//index.html
<body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="backdrop-root"></div>
    <div id="root"></div>
</body>
```

### 9.3 Working with Refs

**Ref** stands for references. They allow us to get access to other DOM elements and work with them. They set a connection between a specific HTML element and the JS code which utilizes it.

This simplifies some functions where we rely on states, for example forms. Instead of defining a state and creating handler for onChange situation of the components, we can ref the HTML component and invoke them in the form submit handler (for example we are interested on the values only when they hit the submit action)

```js
 const nameInputRef = useRef();

 //in handler
 const enteredName = nameInputRef.current.value;
 //process further with enteredName

 // in JSX
<input id="username" type="text" ref={nameInputRef} />
```

Depending on the use case we want to use state or ref. If we never want to change a value, we can go directly with ref (otherwise we would be using a state with a key logger). States are cleaner but more code, Refs have less code but we have edge cases where we would have to manipulate DOM.

Refs make components uncontrolled because we lose the binding we got through states.