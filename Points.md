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
