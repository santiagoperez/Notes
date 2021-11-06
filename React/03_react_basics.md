## 3. React basics

Module content:

1. React Core Syntax & JSX

1. Working with components

1. Working with data

### 3.1 React fundamentals

React is a framework on top of JS, so we leverage their syntax. It simplifies enormously JS code when building complex, interactive and reactive UIs

Uses **Components** which make an UI. A component is a building block. Combination of JS, HTML, CSS. Can be reusable. You build individual components and tell react how to bring them together.

React allows you to create re-usable and reactive components consisting of HTML and JS. Uses a declarative approach, defining the desired target state. React figures out the elements on the DOM.

React is developed locally over **node.js**, a server to render JS code

### 3.2 React structure

**index.js** is the landing code which starts being executed. It is not main js but it will be transformed when deployed to the browser (that is why it contains html/css statements).

It imports and launches ReactDOM where we inform this is a React app. React is served into 2 libraries, *React* and *ReactDOM*, we need to import both.

ReactDOM launches **index.html** page (as a SPA). This page has a root div. **index.js** asks to render (in JSX syntax) the **App** component into the root div. Managed into **App.js**. The invoked function is also written in JSX.

**JSX** stands for *JavaScript XML*. Developed by React team, works specifically for this framework (not on Vanilla JS).

The HTML code inside App is the final state for our *root* element. We avoid having to get the root element, append new divs, childs, etc (imperative approach). We just need how eventually it looks like. React generates instructions on our behalf.

**App.js** will be the root component. It will next all inner components we will create.

### 3.3 Writing components

A component in React comprises on a JS function. For invoking them in their root, we need to ```export default``` them and import later, where we can state them as a regular HTML element.

Components should only have one root element per JSX code snippet.

We need to explicitly tell to import the CSS styling. Also, instead of changing the style of the HTML element through class, we do it through className.

We can also use JS logic inside component functions. In the HTML div, we can invoke JS logic or reference to valid expressions by surrounding the snippet within ```{}```.

For passing data within components we use **Props**. That means adding an attribute within the component declaration and it will be in the scope of the inner component through the props object.

```js
<App />

//In App.js
 const expenses = {
      id: "e1",
      title: "Toilet Paper",
      amount: 94.12,
      date: new Date(2020, 7, 14)    
  ];
<ExpenseItem title={expenses.title} amount={expenses.amount} date={expenses..date}></ExpenseItem>


//In CourseGoalItem.js
function ExpenseItem(props) {
  return (
    <div>
      <div>{props.date.toISOString()}</div>
      <h2>{props.title}</h2>
      <div>${props.amount}</div>
    </div>
  ); 
}

```

You can nest and pass props further level downwards.

### 3.4 Composition

Composition is about building components inside other components

We can also pass elements not through props but within the tags. For example if we share specific styles within components.

In order to build custom HTML components with elements within the tab we should output **{props.children}** within the Component definition. In the wrapper component we should import the styles from the "parent" component by appending to the classNames the ones in the parent via **props.className**

### 3.5 Advanced syntax

Old React applications needed to import React in all JSX files. So instead of making ```return (<div>...</div>)``` we had to do ```return React.createElement('div', {attributes for div}, React.createElement(element inside div), ... attrs and further nested elements)```. So, for example,

```js
return (
  <div>
    <h2>Let's get started!</h2>
    <Expenses items={expenses} />
  </div>
);

  //is equals to

  return React.createElement(
    'div',
    {},
    React.createElement("h2", {}, "Let's get started!"),
    React.createElement(Expenses, ( items: expenses ))
  );
)
```

also in the createElement approach we cannot return more than one element (that is why we only have a parent div and can nest inside them).

Another syntax change is to transform component functions into arrow functions.