# Hooks, Getting in a New Relationship
##### Introducing React Hooks

### INTRO
Once upon a time!!! 
In 2018, at the React Conference "Hooks" was officially Introduced to React.

That's all for GK.

Hooks arrived as a savior for developers who were struggling in maintaining hundreds of states for hundreds of components.

They let you use state and other React features without writing a class. Now, you can kick out classes from your components.
>No need to worry, There are no plans to remove classes from React permanently, _yet_

You can adopt Hooks gradually,
Hooks work side-by-side with existing code so there is no rush to migrate to Hooks. 
>You don’t have to learn or use Hooks right now if you don’t want to.
### WHY GO FOR HOOKS?
You might be thinking why you need to learn one more feature? The answer is here: 
* It helps when you need to maintain too many components and states. 
* Completely opt-in. 
You can try Hooks in a few components without rewriting any existing code.
* A “_wrapper hell_” of components surrounded by layers of providers, consumers, higher-order components, render props, and other abstractions. While we could filter them out in DevTools, this points to a deeper underlying problem: React needs a better primitive for sharing stateful logic, here Hooks made an appearance.
* With Hooks code Reusability is improved, you can extract stateful logic from a component so it can be tested independently and reused. Hooks allow you to reuse stateful logic without changing your component hierarchy[](https://reactjs.org/docs/hooks-intro.html#motivation). This makes it easy to share Hooks among many components or with the community.
* render props and higher-order components try to solve some problems but make code harder to follow, because it requires to restructure your components.
* components might perform some data fetching in componentDidMount and componentDidUpdate. However, the same componentDidMount method might also contain some unrelated logic that sets up event listeners, with cleanup performed in componentWillUnmount. Mutually related code that changes together gets split apart, but completely unrelated code ends up combined in a single method. This makes it too easy to introduce bugs and inconsistencies.
* It’s not always possible to break these components into smaller ones because the stateful logic is all over the place. It’s also difficult to test them. This is one of the reasons many people prefer to combine React with a separate state management library.
* class components can encourage unintentional patterns that make these optimizations fall back to a slower path

### How Hooks Affect the Coding Style

1. Say bye! to class
##### Without Hooks:
_Class Components_
```
class Clock extends React.Component {
    ...
    ...
    render() {
	    return (
			<div>
				<h1>...something...</h1>
			</div>
		);
	}
}
```
##### With Hooks:
_Function Components_
```
function Example() {
    ... // Hooks can be used here
    ...
    render() {
	    return (
			<div>
				<h1>...something...</h1>
			</div>
		);
	}
}
```
###### OR like this:
```
function Example = () => {
    ... // Hooks can be used here
    ...
    render() {
	    return (
			<div>
				<h1>...something...</h1>
			</div>
		);
	}
}
```
*> you can also pass props to the function:* 
```
function Example(props) {
    ... // Hooks can be used here
    ...
}
```
###### OR like this:
```
function Example = (props) => {
    ... // Hooks can be used here
    ...
}
```
>props can be accessed like this -> `const v = props.value`

2. Creating a local state
##### Without Hooks:
```
const state = {
    x: 10,
    y: 'hello',
    z: {
        word: "world!"
    }
}
```
##### With Hooks:
>useState is used to set the initial value for a local state.
```
// this is How we declare a new state variable
const [color, setColor] = useState('Yellow');

// declaring multiple state variables
const [x, setX] = useState(10);
const [y, setY] = useState('hello');
const [z, setZ] = useState([{
    word: "world!",     
}]);
```

3. Accessing state: a Breakup With `this`

##### Without Hooks:
```
constructor(props) {
    this.state = { text: 'demo' };
}

render() {
    return (
        <div>
            <h1>This is { this.state.text }</h1>
        </div>
    );
}
```

##### With Hooks:
>While using hooks, state variables can be accessed directly
```
const [text, setText] = useState('demo');

render() {
    return (
        <div>
            <h1>This is { text }</h1>
        </div>
    );
}
```

4. Changing the State

##### Without Hooks:
```
...
this.state = {
    a: 1,
    b: 2,
    fruit: 'apple'
}

...
{
    this.setState({
        ...state,
        fruit: 'orange'
    });
}
```

##### With Hooks:
```
const [fruit, setFruit] = useState('apple');

...
{
    setFruit('orange')
}
```

5. Effect of the Effect Hook

>- React runs the effects after every render, including the first render.
>- With useEffect() we can run a script after each update or after a particular change.
>- Lifecycle methods _componentDidMount, componentDidUpdate or componentWillUnmount_ can be replaced with useEffect()

```
// To run with each Update
useEffect(() => {
    // Do something
});


// run only when value of "color" is changed
useEffect(() => {
    // Do something
}, [color]);


// run only on first render
useEffect(() => {
    // Do something
}, []);
```
Let's see some usages, in lifecycle methods  
--- ComponentDidMount
##### Without Hooks:
```
componentDidMount() {
    // do something
    const cat = "tom";
    this.setState({
        ...state,
        animal: cat
	});
}
```

##### With Hooks:
```
useEffect(() => {
    // Do Something
    const cat = "tom";
    setAnimal(cat);
}, []);
```
--- ComponentDidUpdate
##### Without Hooks:
```
componentDidUpdate() {
    // do something
    const cat = "tom";
    this.setState({
        ...state,
        animal: cat
	});
}
```
##### With Hooks:
```
useEffect(() => {
    // Do Something
    const cat = "tom";
    setAnimal(cat);
})
```
above snippet will run the code at every update including the first render acting as a combination of _componentDidMount and componentDidUpdate_, if you want to prevent it from running on first render, then it can be done by keeping a check of first render, like this: 
```
const [isFirstRender, setIsFirstRender] = useState(true);

useEffect(() => {
    if (isFirstRender) {
        setIsFirstRender(false);
    } else {
        // do Something
        const cat = "tom";
		setAnimal(cat);
    }
}
```

--- ComponentWillUnmount

#### Without Hooks:
```
componentWillUnmount() {
    // Do Something
}
```
##### With Hooks:
>Just return a function ( named or anonymous ) for cleanup, that we do in ComponentWillUnmount
```
useEffect(() => { 
    return () => {
        // Do something
    }
});
```

6. Getting the context with the Context Hook

`useContext()` takes a context object as the parameter and returns the corresponding context values at that time. Refer to the example below for a better understanding.
```
// for example, We have
const flowers = {
	sunflower: {
		petals: 25,
		color: "yellow"
	},
	daisy: {
		petals: 5,
		color: "white"
	},
	rose: {
		petals: 30,
		color: red
	}
};


// Creating our context
const MyContext = React.createContext( flowers.rose );


// Wrappin the component with <MyContext.Provider>
function App() {
	return (
		<MyContext.Provider value={ flowers.sunflower }>
			<MyComponent />
		</MyContext.Provider>
	)
}
```
The current context value is determined by the value of the `value` prop passed in the nearest `<MyContext.Provider>` in which the component is wrapped.
```

// ... somewhere in our function component ...
const flower = useContext(MyContext);

```
Now the flower will have the value of rose:
`{
petals: 30,
color: "red"
}`
and can be used as 
`<p>Colour of rose is { flower.color }.</p>`   
It will run each time when the context is updated

>You must have got the '_context_' of this blog if you are still here, kindly have a look at "Some rules to remember" below:
### Some rules to remember
- _never be conditional with Hooks_: 
    don't call hooks inside loops or conditions, call Hooks at the _Top level_
- _don't call hooks from nested functions_:
    call only _from React Function components_ or _custom hooks_
>More details can be found in official React docs, available [here](https://reactjs.org/docs/hooks-rules.html)

### More about Hooks
##### More Hooks
Some other commonly used Hooks are: 
- [Basic Hooks](https://reactjs.org/docs/hooks-reference.html#basic-hooks)
    
    -   [useState](https://reactjs.org/docs/hooks-reference.html#usestate)
    -   [useEffect](https://reactjs.org/docs/hooks-reference.html#useeffect)
    -   [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext)
-   [Additional Hooks](https://reactjs.org/docs/hooks-reference.html#additional-hooks)

    -   [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer)
    -   [useCallback](https://reactjs.org/docs/hooks-reference.html#usecallback)
    -   [useMemo](https://reactjs.org/docs/hooks-reference.html#usememo)
    -   [useRef](https://reactjs.org/docs/hooks-reference.html#useref)
    -   [useImperativeHandle](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)
    -   [useLayoutEffect](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)
    -   [useDebugValue](https://reactjs.org/docs/hooks-reference.html#usedebugvalue)   
You can see a list of React Hooks [here](https://reactjs.org/docs/hooks-reference.html).
##### Custom Hooks
A custom Hook is a function whose name starts with ”`use`” and that may call other Hooks and, lets you extract component logic into reusable functions.

Let's create a custom Hook `useColor` that returns the color of the flower whose ID is passed as argument:
```
function useColor(flowerID) {
    const [color, setColor] = useColor(null);

    useEffect(() => {
        /*    Extract the value of colour of the flower from the database and set the value of color using setColor()    */
    });

    return color;
}
```
Now, we can use our custom hook,
```
{
    // To get the colour of the flower with ID = 10
    const color = useColor(10);
}
```
Learn more about how to create the [custom hooks](https://reactjs.org/docs/hooks-custom.html) in detail.

[see](https://reactjs.org/docs/hooks-intro.html) official docs for React Hooks.