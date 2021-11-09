## 10 Effects, Reducers & Context

Module content

1. Working with (Side) Effects

1. Managing more Complex State with Reducers

1. Managing App-Wide or Component-Wide State with Context

### 10.1 Working with (Side) effects

A component's goal is to Render UI & React to User Input

- Evaluate & Render JSX

- Manage State & Props

- React to (User) Events & Input

- Re-evaluate Component upon State & Prop Changes

This is baked into React via the tools and features (e.g. useState, props, etc).

What are the side effects? Anything else happening in our application, such as:

- Store Data in Browser Storage (browser contains a key/value data source where we can store information by using the **localStorage** variable)

- Send HTTP Request to Backend Servers

- Set & Manage Timers

For that we do not need React. Those are tasks happening outside of the normal component evaluation and render cycle - especially since they might block/delay rendering (e.g. HTTP Requests).

By placing a HTTP Request inside a component function, we would start rendering (returns appear later inside the function) after the request has been finalized. This delays the rendering. Even more, if we state our component, with every state change, we would perform the HTTPrequest again.

For that we have useEffect. It is called with 2 arguments, (1) a function that should be executed after every component evaluation (2) the dependencies of the effect; the function will be executed only if those dependencies are met (like a conditional dependency). So, instead of being called as regularly with every component rendering, only under certain conditions are met (dependencies) we will call the effect function.

If we don't add any dependency (meaning we leave an empty array ```[]```) the effect will be called just once.

Typically we want to add in the dependencies the variables that might change in the component by use of the component itself, not any built-in function by react or variables outside of our component.

A very common example for effect is to add delays on user input (for example on login data to check validity of it). We combine the effect with the use of a timeout which will *bounce* the input to only evaluate it after some period of "inactivity". useEffect can return also a callback function.

```js
useEffect(() => {

const identifier = setTimeout(() => { //This is only evaluated if the user has been 500ms without typing, i.e. triggering the state change.
    setFormIsValid(
    enteredEmail.includes('@') && enteredPassword.trim().length > 6
    );
}, 500);

return () => {
    clearTimeout(identifier);
};

}, [enteredEmail, enteredPassword]);
```