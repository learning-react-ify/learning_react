To scaffold a React project we can use npx or yarn.

# using Yarn

**creating a React app**

```sh
yarn create react-app reactprj
```

**create Nextjs app**

```sh
yarn create next-app nextprj
```

**create Blitz app**

```
yarn create blitz-app blitzprj
```

**Create Redwoodjs**

```sh
yarn create redwood-app redwoodjsprj
```

# In React

**class components**

Returns JSX in the `render` method.

```js
class ScoreCompoennt extends React.Component {
  consrtuctor() {
    this.state = null;
  }

  this.setState() // trigeers re-rendering in a class component.

  // called when the component has already been re-rendered.
  componentDidUpdate() {}

  // component is being mounted for the first time on the DOM
  compoenentOnMount() {}

  // caslle dwhne the compoennt is being destroyed. The componet is being removed from the DOM.
  compoenntDidUmount() {
    // cancel  the network connection code
  }

  // called before the compeont is re-rendered
  shouldComponentUpdate() {
    return Booelan;
    // if retunr true, then React will re-render the compeont
    // if it returns false, React will not re-redner the compoenent.
  }

  render() {
    return <div>JSX here.. {counter}</div>;
  }
}

var instance = new ScoreComponent();
instance.render();
```

**functional components**

With the addition of hooks, we can manaage local state in functional components.

```js
function ScoreCompoennt() {
  return <div>JSX here</div>;
}
```

**useState**
Is used to manage local state in functional components.

Import

```js
import { useState } from "react";
```

Hook names are prefixe with the "use".

```js
useState(0);
```

Returns an array. the first element is the state of the hook, then second element in the array is the upadter function. This upadter function is used to upadte the state.

```js
const result = useState(0);

result[0]; // 0
result[1](98);
result[0]; // 98
```

the upadter function re-renders the component.

```js
function ScoreCompoennt() {
  const result = useState(0);

  return (
    <div>
      State value:{result[0]}
      <button onClick={() => result[1](89)}>Click</button>
    </div>
  );
}
```

**Types of Componensts**

_Presentational components/Dumb components_
They are used for presenting data. They are pure components(because they don't have intearction with the outside world). They only recive data via their props and display them.

```js
function TodoList(props) {
  return (
    <div>
      {props?.todos?.map((todo) => (
        <li>{todo}</li>
      ))}
    </div>
  );
}

<TodoList todos={[]} />;
```

_Smart components/container components_
They generate their data and alse affect the outside world or their parent components and they affect global varaibels. These components side-effects.

- fetch data from a network
- Open a stream
- Read from a stream
- Write to a stream
- etc

```js
function App() {
  const [todos, setTodos] = useState(0);
  return (
    <div>
      <div>
        <input type="text" />
        <button onClick={() => setTodo([...todos, todoText.value])}>Add</button>
      </div>
      <div>
        <TodosList todos={todos} />
      </div>
    </div>
  );
}
```

# useEffect

Gives us the ability to hook into severeal lifecycle methods found in the class components.

```js
useEffect(() => {
  // write our side-effects code.
  // - fetch data from a network
  //- Open a stream
  //- Read from a stream
  //- Write to a stream
  // - etc
});
```

Basic usage inside a funciton compoennt:

```js
function App() {
  useEffect(()=> {
    //
  })

  return (...)
}
```

The function parameter in the useEffect hook is called after the functional componet has been rendered.

```js
function App() {
  const [todos, setTodos] = useState();

  useEEffect(async () => {
    const data = await axios.get("URL_OF_OUR_TODO_BACKENG_GOES_HERE");
    setTodos([...todos, ...data?.data]);
  });

  return (
    <div>
      <div>
        <input type="text" />
        <button
          onClick={() => {
            axios.post("", todoText?.value);
          }}
        >
          Add
        </button>
      </div>
      <div>
        <TodosList todos={todos} />
      </div>
    </div>
  );
}
```

To hook into the componentDidUnmount in useEffect, the function callback passed to useEffect must return a function.

```js
useEffect(() => {
  //
  return () => {
    // I will be called before my component is destroyed.
  };
});
```

Example:

```js
function App() {
  const [todos, setTodos] = useState();

  useEEffect(async () => {
    const data = await axios.get("URL_OF_OUR_TODO_BACKENG_GOES_HERE");
    setTodos([...todos, ...data?.data]);

    return () => {
      console.log("I am being destroyed.");
    };
  });

  return (
    <div>
      <div>
        <input type="text" />
        <button
          onClick={() => {
            axios.post("", todoText?.value);
          }}
        >
          Add
        </button>
      </div>
      <div>
        <TodosList todos={todos} />
      </div>
    </div>
  );
}
```

useEffect has a second argument. This second argument is called the dependencies.

```js
useEffect(() => {
  // effects code here
}, []);
```

Check for re-rendering.

```js
var counter = 0;
var score = 9;

useEffect(() => {
  // effects code here
}, [counter, score]);
```

On initial render/mount, useEffect calls its callback function without checking its dependencies, but on subsequent re-renders, useEffect will check its dependencies to determine whether to run its callabck function.

**Assignment**

1. Build a counter app using `useState` hook. This app will display the current value of the counter, there will be a button that will increment the counter state. The UI must be updated to display the current value of the counter when the button is clicked.

2. Clone the counter example in `1` above. Here, simulate a mock server and use the `useEffect` hook to fetch the counter value from the mock server.

```js
var counter = 0;
function getCounterValue() {
  return counter;
}
function addCounterValue(newValue) {
  return (counter += newValue);
}

useEffect(() => {
  const data = getCouneterValue();
  // instead of using axios
});
```

## React.Context API and useContext

In React, we build apps using components. React is a tree of components. A component is a single unit of UI. We can pass data between component via props.

```js
App
|
Compa Compb
|       |
Compc  Compd
|
Compe
```

This will result in what is called props drilling. It results in unnecessary props. B/cos most of the component wont make use of the props.

Context can be created anywhere in the component tree.

```js
const CounterContext = React.createContext(0);

.Provider
.Consumer
```

The `.Provider` makes the Context value to be avialbale in a component branch.
The `.Consumer` gets the value of the Context from anywhere in the tree.

```js
function App() {
  const [counter, setCounter] = useState(0);

  return (
    <div>
      <div>
        <button onClick={() => setCounter(counter + 1)}>Incr Counter</button>
      </div>
      <div>
        <RenderDisplayCounter />
      </div>
    </div>
  );
}

function RenderDisplayCounter(props) {
  return <DisplayCounter />;
}

function DisplayCounter(props) {
  return <div>Counter: {props.counter}</div>;
}
```

Context cam be a global variable for a branch in the component tree.
React.createContext should not be called inside a functional component, why? because the Context will be re-created when the functional component re-renders.

We can use the `value` props in the `.Provider` component to pass down the our values dwon the compoent branch.

The value passed to the React.Context can be of any type: primtive type or object type. It can be a string, number, boolean, regexp, function, and objects.

**Assignment on useContext**

```js
function App() {
  const [scores, setScores] = useState({ homePoint: 0, awayPoint: 0 });

  return (
    <div
      style={{
        display: "flex",
        justifyContent: "center",
        marginTop: "10px",
        fontSize: "40px",
        flexDirection: "column",
        alignItems: "center",
      }}
    >
      <div>
        <input type="number" id="homePoint" placeholder="Add Home point..." />
        <button
          onClick={() =>
            setScores({ ...scores, homePoint: window.homePoint.value })
          }
        >
          Set Home point
        </button>
      </div>
      <div>
        <input type="number" id="awayPoint" placeholder="Add Away point..." />
        <button
          onClick={() =>
            setScores({ ...scores, awayPoint: window.awayPoint.value })
          }
        >
          Set Away point
        </button>
      </div>
      <div>
        <RenderScoreDisplay scores={scores} />
      </div>
    </div>
  );
}

function RenderScoreDisplay(props) {
  return <DisplayScores scores={props.scores} />;
}

function DisplayScores(props) {
  return (
    <div>
      <div>Home Point: {props.scores?.homePoint}</div>
      <div>Away Point: {props.scores?.awayPoint}</div>
    </div>
  );
}
```

The above example uses props to pass the `scores` state from the `App` component to the `RenderScoreDisplay` component and to the `DisplayScores` component. Now, I want you to refector the bad practice to use `React.Context` and `useContext` to pass the `scores` state from the `App` component to the `DisplayScores` component without the props drilling.

The `.Consumer` component is used in class components to get the value of the Context.

## useContext hook

The `useContext` hook does away with using `.Consumer` component. The useContext hook is used in functional components to get the value of the Context value.

# useRef

useRef is used to get a reference to a DOM element or to a component instance.

```js
import { useRef } from "react";
```

```js
const ref = useRef();

ref;

{
  current: null;
}
```

```jsx
<div>
  <input type="text" />
  <button>Focus</button>
</div>
```

All DOM elements are based off a super class called `HTMLElement`.A div is an instance of the `HTMLDivElement` class. This class is a subclass of the `HTMLElement` class.

```
div
|
v
HTMLDivElement
|
V
HTMLElement
```

The HRMLElement contains all the methods and properties that can be used to manipulate an element. The methods/properties are focus(), innerHTML, innerText, blur(), value, etc. We can get the instances of element in HTML using the document.getElementById() method, document.getElementsByClassName(), document.getElementsByTagName(), document.querySelector(), document.querySelectorAll(), document.createElement(), document.createTextNode(), etc.

To get instances of elements in React JSX code we use the `ref` prop.

```js
const ref = useRef();
```

```jsx
<div>
  <input type="text" ref={ref} />
  <button>Focus</button>
</div>
```

```js
ref.current;
```

This `ref.current` contains the instance of the element. HTMLInputElement instance.

```js
function App() {
  const ref = useRef();

  const Fn = () => {};

  return (
    <div>
      <input type="text" ref={ref} />
      <button onClick={Fn}>Example</button>
    </div>
  );
}
```

# createRef

This is used in class components to get a reference to a DOM element or to a component instance.

```js
import React, { createRef } from "react";

const ref = createRef();
```

createRef and useRef does the same thing, the difference is that useRef is used in functional components while createRef is used in class components.

**Assignment**

_1_
Create an application that displays a list of fruits, and a button called "Change Theme" This button should change the text color of the fruits when clicked. The color of the fruits shold be in a Context.

The App should look like this:

```
App
|
V
RenderFruits
|
V
Fruits
```

The Fruits component will contain the button and the list of fruits. Create the context in the App component and use it in the Fruits component. The Context will carry the color of the fruits.

_2_

create a compoennt that renders list of animals and a button. This button should change the text color of an animal when clicked. use `useRef` to get the reference to the animal node.

## useReducer

useReducer is used to manage the state of the application. It is an easy way to use Redux state management in our functional components. useReducer is based on the concept of Redux state management pattern.

In Redux, state are kept globally. The state is managed by the store. The state is just an object having properties. The properties are the state of the application. We can subscribe to the state to get or modify the state. We can dispatch actions to change the state. We have reducer function, this reducer function is a pure function that takes the previous state and an action and returns the new state.

This reducer function is where we can modify the state or return the state. It houses the logic of the state. We have dispatch actions, this are actions that are dispatched to the store. When these actions are dispatched, the store calls the reducer function passing it the state of the store and the action.

```js
function reducer(state, action) {
  if (action.type === "INCREMENT") {
    return { ...state, counter: state.counter + 1 };
  }
  if (action.type === "DECREMENT") {
    return { ...state, counter: state.counter - 1 };
  }
  return state;
}

const initialState = {
  counter: 0,
};

const [state, dispatch] = useReducer(reducer, initialState);

state; // { counter: 0 }
dispatch({ type: "INCREMENT" });

state; // { counter: 1 }

dispatch({ type: "DECREMENT" });
state; // { counter: 0 }

dispatch();
```

**Assignment on `useReducer`**

Write a fruits application that lists an array of fruits. We should be able to add new fruit to this list. use the `useReducer` hook to manage this list of fruits. We should be able to add new fruit, remove fruit, and change the name of the fruit.

## Default Props

Default props talks about the initial value of props in a component set by the component itself.

```js
function Counter({ value }) {
  return <div>{value}</div>;
}

// <Counter value={89} />
<Counter />;

Counter.defaultProps = {
  value: 0,
};
```

This `Counter` component has a prop called `value`.

So to assign a default value to this prop, we can use the `defaultProps` property.

**Assignment on Default Props**

We have this component:

```js
function MulitComp({counter, name, animals}) {
  return (
    <div>
      <div>Counter: {counter}</div>
      <div>Name: {name}</div>
    </div>
    <div>
      <div>Animals</div>
      <ul>
        {animals.map((animal) => (
          <li key={animal}>{animal}</li>
        ))}
    </div>
  );
}
```

Set the default props of the component.

## Prop Types

Prop Types is used to validate the data type of props in a component. This prop types helps us to know the type of data type that a props should have. Example, a props can be type of string, object, boolean, number, etc.

We use the `prop-types` package to validate the data type of props.

```js
import PropTypes from "prop-types";

function Counter({ value }) {
  return <div>{value}</div>;
}

Counter.propTypes = {
  value: PropTypes.string,
};

<Counter value="hello">
<Counter value={90}>
<Counter value={true}>
```

**Assignment on Prop Types**

We have the following component:

```js
function MulitComp({counter, name, animals}) {
  return (
    <div>
      <div>Counter: {counter}</div>
      <div>Name: {name}</div>
    </div>
    <div>
      <div>Animals</div>
      <ul>
        {animals.map((animal) => (
          <li key={animal}>{animal}</li>
        ))}
    </div>
  );
}
```

The `counter` should be a number, the `name` a string and the `animals` an array of strings. Type check these props using the `prop-types` package.

## Conditional Rendering

This is rendering sections of the UI based on some conditions. The components can return different JSX code based on some comditions.

It is just to if/else statement:

```js
const cond = true;

if (cond) {
  // do something
} else {
  // do another ting
}

function cond() {
  if (cond) {
    return "Hello";
  } else {
    return "World";
  }
}
```

How we can conditionally render JSX code?

- if/else
- ternary operator
- && operator
- switch statement

**if/else statement**

This just entails using the `if/else` statement in JSX.

```js
function Comp() {
  const cond = true;

  if (cond) {
    return <div>Hello</div>;
  } else {
    return <div>World</div>;
  }
}
```

**ternary operator**
This is the use of the ternary operator to conditionally render elements in our components.

For example:

```js
function Comp() {
  const cond = true;
  return cond ? <div>Hello</div> : <div>World</div>;
}
```

**&& operator**

The && operator is used to check if the LHS statement is a truthy value and the RHS statement is a truthy value. If the LHS of the && operator is true then the RHS is returned. If the LHS is a falsy value then the LHS is returned.

```js
true && false; // false

1 && 4; // 4
0 && 7; // 0
false && 100; // false

function func() {}

if (true) func();

true && func();
```

Taking this into the functional component:

```js
function Comp() {
  const cond = true;
  return (
    <>
      {cond && <div>Hello</div>}
      {!cond && <div>World</div>}
    </>
  );
}
```

**switch statement**

This is the use of the switch statement to conditionally render elements in our components.

```js
switch (cond) {
  case true:
    return 1;
  case false:
    return 2;
  default:
    return 3;
}
```

Exmample:

```js
function Comp() {
  const cond = true;
  switch (cond) {
    case true:
      return <div>Hello</div>;
    case false:
      return <div>World</div>;
  }
}
```

**Assignment on Conditional Rendering**

Write a Fruits app. This app should render a list of fruits. I should be able to remove fruits from the list. This app should conditionally render the list of fruits based on the following conditions:

```
Orange      X
Apple       X
Banana      X
```

- When the fruit list is empty, render a message saying "No fruits in the list"

Use any of the conditional rendering techniques you know.

# Render Props

Props is used to pass data from the outside world to the component. It is usually from a parnet component to a child component. These props can be of any data, they can be a number, string, boolean, object and even a function.

```jsx
function Comp({p1, p2}) {
  // the component will recieve the props passed to it via p1 and p2
}

const obj = {
  q:4
}

<Comp p1={3434} />
<Comp p1="efdf" p2={obj} />
```

We can pass a function to a component via props.

```js
function sum(a,f) { retur a+f}

<Compo p1={sum} />

function Comp({p1, p2}) {
  const v = p1()

  return <>{v}</>
}
```

Render Props is a function props used to share code logic between components. A Render Props is a function that returns a JSX code, the code it returns is rendered by the component.

```js
function fn() {
  return <div>Hello World</div>;
}

function Compo({ fn }) {
  return fn();
}

<Compo fn={fn} />;
```

Attributes of Render Props:

- A Render Props is a function passed as a props to components.
- The component uses the Render Props as it render logic.
- The Render Props is shareble between components.

**Use Case**

```js
function CounterApp({ render }) {
  return render();
}

<CounterApp render={() => <h1>I am a Render Props</h1>}>
```

We can pass data to Render Props:

```js
function CounterApp({ render, counter }) {
  return render(counter);
}

<CounterApp render={(data) => <h1>Counter: {data}</h1>} counter={34} />;
```

**Assignment on Render Props**
Create a fruit app. This app will render a list of fruits. Create a component `FruitsList`, this component will receive a Render Props and the fruits to render. Implement this application, so that when a new fruits is add to the list the FruitsList component displays them.

# Events

```html
<button onclick="fn">Button</button>
```

In React:

```js
<button onClick={}>Button</button>
```

Naming Event Patterns in React

```
HTML        React

onclick     onClick
onblur      onBlur
onfocus     onFocus
onmouseover onMouseOver
ondrag      onDrag
ondragleave onDragLeave
```

```js
function Compo() {
  const fn = () => {
    // will be executed.
  };

  return <button onClick={fn}>Button</button>;
}
```

We can pass inline functions to events in React

```js
function Compo() {
  return (
    <button
      onClick={() => {
        //  will be executed.
      }}
      onBlur={() => console.log("Blur")}
    >
      Button
    </button>
  );
}
```

**Parameters**

Event handlers receieve an object from the brower when they are called due to an event. In HTML this parameter is an instance of EventObject.

In React, this parameter is an instacne of SyntheticEvent.

```js
function Compo() {
  const fn = (e) => {
    // will be executed.
  };

  return <button onClick={(e) => fn(e)}>Button</button>;
}
```

# Forms

Forms are a group of elements that does the same work. The `form` element was created to collate/manage the data in these similiar elements.

Form elements:

- input button
- input boxes (password, text, number, email, submit etc)
- textarea

```html
<form name="form" onsubmit="">
  <input type="text" name="user" />
  <input type="submit" />
</form>
```

In React,

```js
function Form() {
  const submitFn = (e) => {
    const userValue = e.form.user;
  };

  return (
    <>
      <form name="form" onSubmit={submitFn}>
        <input type="text" name="user" />
        <input type="submit" />
      </form>
    </>
  );
}
```

**Uncontrolled components**
The form data is managed by the DOM.

```js
function Form() {
  const submitFn = (e) => {
    const userValue = e.form.user;
  };

  return (
    <>
      <form name="form" onSubmit={submitFn}>
        <input type="text" name="user" />
        <input type="submit" />
      </form>
    </>
  );
}
```

**Controlled components**
The form data are managed by the component.

```js
function Form() {
  /*const [value, setValue] = useState();
  const [password, setPass] = useState();*/

  const [{ username, password, email }, setFormState] = useState({
    username: "",
    pawword: "",
    email: "",
  });

  const submitFn = (e) => {
    const userValue = username;
    const pass = password;
  };

  return (
    <>
      <form name="form" onSubmit={submitFn}>
        <input
          type="text"
          placeholder="Type username"
          value={username}
          onChange={(e) => setFormState({ username: e.target.value + "N" })}
        />
        <input
          type="password"
          value={password}
          onChange={(e) => setFormState({ password: e.target.value })}
        />
        <input
          type="email"
          value={email}
          onChange={(e) => setFormState({ email: e.target.value })}
        />
        <input type="submit" />
      </form>
    </>
  );
}
```

**Assignment on Forms**

1. Write a Login form in React. The form elements should contain username and password. The form should display the values of the elements when the submit is clicked.

2. Write a Register form. The form should have elements: username, password, retype password, email. The form should log the values of the form elements, also it should check that the password is same as the retype password.

# List and Keys

```js
function test(fn) {
  fn("hello", 56);
}

test((v) => console.log(v));
```

```js
{
  [1, 2, 3, 4, 5].map((v) => {
    <h1 key={v}>ITem: {v}</h1>;
  });
}
```

The `key` prop is used by React to track the items in a list, this enables it to know track of the items so to know which items are deleted, or change places in the array order. With this React knows which items to re-render.

DOM

```html
<h1 key="1">ITem: 1</h1>
<h1 key="2">ITem: 2</h1>
<h1 key="3">ITem: 3</h1>
<h1 key="4">ITem: 4</h1>
<h1 key="5">ITem: 5</h1>
```

uniqueness

# Fragment

```html
<div>
  <h1>Hi</h1>
</div>
<h2>Im H2</h2>
```

Porting this to React:

```js
function Comp() {
  return <div>
        <h1>Hi</h1>
      </div>
      <h2>Im H2</h2>
}
```

**A React element must return a single root node.**

No two elements can be the root of the JSX elements returned by a component.

There should be only one root element. Components should return a JSX element with only one lemeent as its root.

```js
function Comp() {
  return (
    <div>
      <div>
        <h1>Hi</h1>
      </div>
      <h2>Im H2</h2>
    </div>
  );
}
```

In a table

```js
function Table() {
  return (
    <table>
      <thead>
        <tr>
          <HeadComp />
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>John</td>
          <td>25</td>
        </tr>
        <tr>
          <td>Jane</td>
          <td>24</td>
        </tr>
      </tbody>
    </table>
  );
}

function HeadComp() {
  return (
    <div>
      <th>Name</th>
      <th>Age</th>
    </div>
  );
}
```

The above will generate wrong HTML for the table element:

```html
<table>
  <thead>
    <tr>
      <div>
        <th>Name</th>
        <th>Age</th>
      </div>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John</td>
      <td>25</td>
    </tr>
    <tr>
      <td>Jane</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
```

This is where `React.Fragment` comes in.

The `React.Fragment` is an empty element that can be used to group a list of children without adding extra nodes to the DOM.

```js
import React, { Fragment } from "react";

function HeadComp() {
  return (
    <Fragment>
      <th>Name</th>
      <th>Age</th>
    </Fragment>
  );
}
```

The table will be this:

````html
```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John</td>
      <td>25</td>
    </tr>
    <tr>
      <td>Jane</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
````

**shorthand fro using Fragment**

Instead of using `React.Fragment` you can use the shorthand `<>`.

```js
function HeadComp() {
  return (
    <>
      <th>Name</th>
      <th>Age</th>
    </>
  );
}
```

# Forwarding Refs

Forwarding Refs is a technique that allows you to pass a ref to a child component.

```js
function Comp() {
  const ref = useRef();
  return (
    <div>
      <input ref={ref} type="text" />
      <ButtonComp />
    </div>
  );
}

function ButtonComp() {
  return <button>Click Me</button>;
}
```

We will the `forwardRef` function to pass refs to child components.

```js
import React, { forwardRef } from "react";

const ButtonComp = forwardRef(function (props, ref) {
  const fn = () => {
    console.log(ref.current);
  };
  return <button onClick={fn}>Click Me</button>;
});

function Comp() {
  const ref = useRef();
  return (
    <div>
      <input ref={ref} type="text" />
      <ButtonComp ref={ref} />
    </div>
  );
}
```

# Error Boundaries

```js
try {
  // do something
} catch (e) {
  // handle error
}
```

```js
const a = [2, 3];

try {
  console.log(a[3]);
} catch (e) {
  console.log("There is index 3 in the a array.");
}
```

The lifecycle used to catch an error in React is the `componentDidCatch` lifecycle.

```js
<CompA>
  <CompB />

  <CompC>
    <CompD />
  </CompC>
</CompA>
```

Let's catch an error in `CompD`:

```js
class CompD extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: false,
    };
  }

  componentDidCatch(err, info) {
    this.setState({
      error: true,
    });
  }

  render() {
    if (this.state.error) {
      return (
        <div>
          <p>Oh shoot! Something went wrong, dont panic.</p>
          <p>Refresh this page.</p>
        </div>
      );
    }
    return (
      <div>
        <h1>Hi</h1>
      </div>
    );
  }
}
```

Wheneever an error occirs in a component, the `componentDidCatch` lifecycle is called in all its children components that implemented the lifecycle.

```js
<CompA>
  <ErrorBoundary>
    <CompB />

    <CompC>
      <CompD />
    </CompC>
  </ErrorBoundary>
</CompA>
```

```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: false,
    };
  }

  componentDidCatch(err, info) {
    this.setState({
      error: true,
    });
  }

  render() {
    if (this.state.error) {
      return (
        <div>
          <p>Oh shoot! Something went wrong, dont panic.</p>
          <p>Refresh this page.</p>
        </div>
      );
    }
    return this.props.children;
  }
}

class CompD extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
        <h1>Hi</h1>
      </div>
    );
  }
}
```

# Portals

```js
<CompA>
  <CompB />

  <CompC>
    <CompD />
  </CompC>
</CompA>
```

Portals are used to render DOm nodes in any place in the DOM without follwoing the linear structure of the DOM.

To createa a Protal in React we use the `React.createPortal` function.

```js
React.createPortal(
  <div>
    <p>Hi</p>
  </div>,
  document.body
);
```

```js
function Portal() {
  return React.createPortal(
    <div>
      <p>Hi</p>
    </div>,
    document.body
  );
}
```

```js
<CompA>
  <CompB />

  <CompC>
    <CompD />
    <Portal />
  </CompC>
</CompA>
```

Thats why, Portals are used to web UIs that do not follow the structure of a webpage.

Example:

- Modal dialogs
- Aside navigation bars
- Popups
- Tooltips
- Dropdowns
- etc.

Let's create Modal dialog using Portals:

```js
function Modal() {
  return React.createPortal(
    <div>
      <div className="modal-backdrop"></div>
      <div className="modal-body">
        <p>Hello, I'm Modal</p>
      </div>
    </div>,
    document.body
  );
}
```

We can still pass props to Portals, this is because they are still React components even though they break away from the noral flow of React.

# Data fetching in React

Data fetching entails fetch data from a server and then update the UI with the data in an Recat compoennt.

the key things to do is:

- Call the server
- Get the response from the server
- Extract the data from the response
- then Render the data in the component.

**class components**

```js
function fetch() {
  return ["apple", "banana", "orange"];
}

class DataFetch extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      fruits: [],
    };
  }

  componentDidMount() {
    // here is where we get our fruits data from the server.
    fetch("https://jsonplaceholder.typicode.com/fruits").then((data) =>
      this.setState({ fruits: data })
    );
    //this.setState(Array.from(["apple", "banana", "orange"]))

    axios.get("https://jsonplaceholder.typicode.com/fruits").then((res) => {
      this.setState(res.data);
    });
    // https://627f6e73be1ccb0a465fa1dc.mockapi.io/:endpoint
  }

  render() {
    return this.state.fruits.map((fruit) => (
      <div>
        <p>{fruit}</p>
      </div>
    ));
  }
}
```

**Note**: Data fetching(network requests, etc) is done in React Component when the component has beeen rendered. In class component, the `componentDidMount` lifecycle is the place where the data fetching is done.

**functional components**

```js
function DataFetch() {
  const [{ dogs, loading, error }, setOpts] = useState({
    dogs: [],
    loading: true,
    error: false,
  });

  useEffect(() => {
    async function fetchDogs() {
      // here is where we get our fruits data from the server.
      try {
        const data = await axios.get(
          "https://627f6e73be1ccb0a465fa1dc.mockapi.io/dogs"
        );
        setOpts({ dogs: data.data, loading: false });
      } catch (error) {
        setOpts({ error: true, loading: false });
      }
    }
    fetchDogs();
  }, []);

  if (error) {
    return <p>Something went wrong ...</p>;
  }
  if (loading) {
    return <p>Loading ...</p>;
  }
  if (dogs.length === 0) {
    return <p>No dogs yet.</p>;
  }
  return dogs.map((dog) => {
    return (
      <div>
        <p>{dog.name}</p>
        <img src={dog.avatar} alt={dog.avatar} />
      </div>
    );
  });
}
```

# Routing

Routing is the mapping of URLs to components. A URL will load a certain component.

```
/ -> Home component
/profile -> Profile compoennt
/about -> About component.
```

To add routing to our React project, we use the `react-router-dom` package.

```js
<BrowserRouter>
  <App>
    <CompA>
      <CompB />

      <CompC>
        <CompD />
      </CompC>
    </CompA>
  </App>
</BrowserRouter>
```

We jhave to import the `BrowserRouter` component from the `react-router-dom` package.

```js
// App.js
import { BrowserRouter } from "react-router-dom";

function App() {
  return <BrowserRouter></BrowserRouter>;
}
```

Next, we need to add the `Route` component from the `react-router-dom` package.

```js
import { Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Router exact path="/" component={Home} />
      <Router exact path="/profile" component={Profile} />
      <Router exact path="/about" component={About} />
    </BrowserRouter>
  );
}

function Home() {
  return <h1>Home</h1>;
}

function Profile() {
  return <h1>Profile</h1>;
}

function About() {
  return <h1>About</h1>;
}
```

**Dynamic Routing**

Dynmaic routing maps a changing URL to a page.

```
/profile
```

This page is a static route mapped to a page.

```
/profile/1232
/profile/232323
```

The above URLs are structurally the same. The values of the URLs changes.

To set a dynamic route in React Router, we use the `:` to tell the React Router that a particular is a dynamic route.

```
/profile/:id
```

# Higher-Order components

Higher-order functions are function returned by other functions.

```js
function H(base) {
  return (number) => {
    return number * base;
  };
}

const fn = H(3);

fn(4); // 12
fn(9); // 27
```

a higher-order component is a function that takes a component and returns a new component.

# Composition

# Suspense/React.lazy

Suspense is a React feature that allows us to load component asynchronously.

```js
<CompC>
  <CompD />
</CompC>
```

# React.memo

# Lazy Loading
