## 4 React state

State is an important concept to make apps interactive and reactive. Since React is a declarative approach, we build our UIs to define the target state, instead of building static applications.

Module content:

1. Handling events

1. Updating the UI & Working with "State"

1. A closer look at Components & State


### 4.1 Events & Event handlers

Button declarations in React follow the regular HTML definition ```<button>My button</button>```. We can add handlers as any other HTML component  ```<button onClick="function" />``` where function can be inline or declared in the JSX component (before the return statement) and referenced in the eventHandler declaration.

In order to change elements in the UI by reacting to these components, we could leverage the ```let``` variables. Event handlers can modify those variables and react will update the rendered UI accordingly.

However, the way React works is that, starting with the ```index.js```, React keeps calling component functions which evaluate all the statements and return, through JSX syntax, HTML elements. React keeps calling all the components defined and once done, transform the components into DOM element and renders that. But we want to interactively change those elements. This is done by introducing **states**. They imply that a component should be re-evaluated because something was changed. So trying to update the UI by just declaring let variables does not work for our purpose here.

States have to be imported from 'react' library through a named import **useState**. We will invoke it on the component function through ```useState();``` and providing a default state value (e.g. props). Instead of updating variables with ```=``` handlers will call the useState to reevaluate those variables. ```useState``` gives us access to the "special variable" which is the state. It returns an array with 2 elements: the value itself and the updating function.

```js
const [title, setTitle] = useState(props.title);

const clickHandler = () => {
    setTitle("New title");
};

return (
  <div>
    <h2>{title}</h2>
    <button onClick={clickHandler}>"My Button"</button>
  </div>  
);
```

UseState is registered for every instance the component is called. So we can safely add many components which independently handle state on their own.

React does not initialize the state always. If we subsequently perform handlers that change the state, react will take the last status and keep working with that. This is important because other variables/attributes will not get affected when clicking several times a button.


With forms the process is analog. We define forms regularly in HTML through ```<form />```. The forms will also contain different eventHandlers which will allow modifying component and page state. EventHandlers contain an event object and with that one we can get useful information to handle in that form (e.g. what has the user input in a text field or selected in a date picker) such as ```event.target.value```.

Input values in the forms can be tracked and set as states as well. We handle multiple separate states and the event handlres will keep track of those. Submit function just collects the states and submit them (to create new components, sending through a different page/service, etc). 

### 4.2 Handling multi states

We can call ```useState``` several times inside a component, and each call will be independent from each other. But we can also call useState only once and passing an object which wraps every attribute in the component that we want to register, and each handler will update the specific attribute. Using this approach we need to make sure we maintain the rest of the elements in the previous state. This is done by passing the previous state through the updater function and using spread operator to update the new attribute (we cannot do it directly because React schedules status updates and they are handled async, so we could have race conditions).

There is no better way than other but usually calling state once and passing the array object keeps the code cleaner and better maintained.

For submit button, HTML sends a request to server once we click on those. However and since for SPA we do not always want to communicate with the server there but to let JS handling the submission, HTML allows us to ```event.preventDefault();```.

We can also 2-way bind attributes. This mean that, once we update the state of an object (through a handler) we can modify any other part of the UI back. Imagine we submit the form and collect the information. We can influence and modify the HTML component so that it resets to default by setting the state back to that default value. In this case, a handler will (1) collect latest state of data (2) do further action with that state and (3) reset state to another value.

```js
//With multiple states
const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState("");
  const [enteredDate, setEnteredDate] = useState("");

  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
  };

  const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value);
  };

  const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value);
  };


//With one state
const [userInput, setUserInput] = useState({
      enteredTitle: '',
      enteredAmount: '',
      enteredDate: ''
  });

  const titleChangeHandler = (event) => {
    setUserInput((prevState) => {
        return {...prevState, enteredTitle: event.target.value};
    });    
  };

  const amountChangeHandler = (event) => {
    setUserInput((prevState) => {
        return {...prevState, enteredAmount: event.target.value};
    });    
  };

  const dateChangeHandler = (event) => {
    setUserInput((prevState) => {
        return {...prevState, enteredDate: event.target.value};
    });    
  };

  //Form definition
   return (
    <form onSubmit={submitHandler}>
      <div>
        <div>
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div>
          <label>Amount</label>
          <input
            type="number" min="0.01" step="0.01"
            onChange={amountChangeHandler} />
        </div>
        <div>
          <label>Date</label>
          <input
            type="date" min="2019-01-01" max="2022-12-31"
            onChange={dateChangeHandler} />
        </div>
      </div>
      <div>
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
```

### 4.3 Bottom-up communication

Transferring information from parent to child is done via props.

In order to communicate child to parent, what we do is to define a function in the parent that acts as an event listener, same as we do with regular forms or buttons. We can observe changes in the child and handling them in the parent.

The pattern is the following.

1. On the parent component and when we call the child component, we pass a new event onFunction.

1. On the child, we invoke that function whenever we want to pass the data upwards

1. On the parent, the onEvent function should point a handler which receives the passed data and processes it

```js
//Parent
const processHandler = (dataReceived) => {
  //process dataReceived
};
<ExpenseForm onProcessEvent={processHandler} />

//Child
const submitFunction = () => {
  
  //create data to be submitted

  //sends data upwards
  props.onProcessEvent(dataReceived);
}
```

This can be combined with our previous way of passing data up-down via props so that we can bring data from a component to another one. We do not pass data from peer components but look for the closest parent component.

### 4.4 Controlled components

2-way binding are controlled components. That means that a value which is used in the component is passed on to a parent components through props and received from parent components.

We use a component in a child which has logic residing in their parent.

### 4.5 Stateful and stateless components

Components also can also be stateful (they manage state) or stateless (they don't manage state). Stateless are also called presentational, static.