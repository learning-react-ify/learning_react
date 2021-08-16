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