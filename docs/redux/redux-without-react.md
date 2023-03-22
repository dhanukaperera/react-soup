# Redux without React

## The Redux API

* Apply Middleware
* Compose
* Create Store
* CombineReducers
* Bind Action Creators

Redux can use in HTML, Angular or Node.

API methods like useSelector, useReducer, useDispatch, they are not in Redux Library. They are in the React Redux library.

The core Redux lib is unaware about React or which framework it is using.

What is different between the Reducer in React and Reducer in React Redux?

Both lib are similar but the React Reducer does not have;
* Middleware build in
* Bindings for higher order components that will take care of memorization
* Avoiding rendering components when possible.

What is the different between the Redux and the Context API?
* The context API is actually used by both of them to kind of thread access to your store through the application. Both use reducer and Redux will use the context API to make your store available throughout the application. They both use the Context API.

### Compose

Take multiple functions and compose a new function out of all these pieces.

```javascript
import {compose} from 'redux'

const makeLouder = string => string.toUpperCase();
const repeatThreeTimes = string => string.repat(3);
const embolden = string => string.bold();

const makeLounderRepeatThreeTimesAndEmbolden = string => embolden((repeatThreeTimes(makeLouder(string))))

const usingCompose = string => compose(makeLouder,repeatThreeTimes,embolden)

makeLounderRepeatThreeTimesAndEmbolden('Hello')
usingCompose('Hello')

// The output same both makeLounderRepeatThreeTimesAndEmbolden & usingCompose
// <b>HELLOHELLOHELLO</b>
```

### Create Store

```javascript
import {createStore} from 'redux';

const initialState = { value: 0 };

const incrementAction = { type: 'INCREMENT' }

const reducer = (state, action) => {
	return state;
}

const store = createStore(reducer); //Store need a reducer

```

What is reducer ?

Reduce take two inputs and produce one output. It take the state of the world , and something that happened and output the NEW state of the world.

The sate of the world is a JavaScript Object that represent everything that's happening in your application. And the next things is things happen which is use users clicked buttons, network requests comeback, change routes, etc. Takeing those both two and produce the new state.


if we `console.log(store)` use reducer we get four functions.

* `replaceReducer(newReducer)` : replace the reducer with a new reducer. using for code splitting.
* `getState()`: return the current state.

What is an action?

The action has multiple inputs, but the type is required. This is required because it the action need to tell the reducer what type of action has happen.

History Lesson : Redux is based on a lib called flux

```javascript
	{
		type : 'INCREMENT' // type is required!
		payload: {} // can be a object or value like 5
		meta : {} // any other information about the actions
		error : {} // Some kind of error you need know about?
	}
```

Why we use call caps for the type in action?

It does not have be a all caps string, but it kind of best practice to reduce the errors, because if you miss spell it the action, it does not trigger right action.

```javascript
const incrementAction  = { type : "INCREMENT"}

const reducer = (state , action) => {
	if (action.type === "INCREMENT") {
		return {state : state.value + 1}
	}
	return state;
}
```

To avoid any typo and spelling errors these actions define as separate constants and use it in the action and reducers.

```javascript
const INCREMENT = "INCREMENT"

const incrementAction {type : INCREMENT}

const reducer =(state,action) => {
	if (action.type === INCREMENT) {
		return { value : state.value + 1}
	}
	return state;
}
```
Note : Redux ToolKit use different style creating the actions.

Reasons not to use Redux Tool Kit
* Level of abstraction (TODO: Review later)
* Depend of the app

### Action Creator

This is fancy way of creating a function.

```javascript

const initialState = { value: 0 };

const INCREMENT = "INCREMENT"
const ADD = "ADD"

const increment = () => ({type:INCREMENT})
const add = (amount) => ({type:ADD, payload:amount })

const reducer = (action, payload) => {
	if(action === INCREMENT) {
		return {value : state.value + 1}
	}else if(action === ADD) {
		return {value: state.value + action.payload}
	}
	return state
}
```

### Dispatch

```javascript
// ...previous code


const store = createStore(reducer,initialState)

// the initial state need to be pass before the dispatch. The initial state can be pass as argument to the createStore or as the default value in reducer

/*
const reducer = (state = initialState, action) => {
	...
}
*/

store.dispatch(increment())

```

Notes: When using redux you need to have one store, one big store that use all the data.

If your api send you deeply nested objects then in FE you can add transformer layer that convert the API to response that the UI need and convert back to when sending to post. In this way even if the API breaks change the data structure you only need to update the trasformer.

### Subscribe

Listen for the state change and call for the function

```javascript

const subscriber = () => console.log('SUBSCRIBER',store.getState())

store.subscribe(subscriber);

```

### Bind Action Creators

Bind actions take a argument of object of functions and bind them.

```javascript
const actions = bindActionCreators({increment,add},store.dispatch)

actions.add(1000)
actions.increment()

```

### Combined Reducers

```javascript
const initialSate = {
	users : [
		{ id:1, name : "Steve"}
		{ id:2 	name : "Eric"}
	],
	tasks: [
		{ title: "File the TPS reports"}
		{ title: "Order more energy drinks"}
	]
};

const ADD_USER = "ADD_USER";
const ADD_TASK = "ADD_TASK";

const addTask = (title) => ({title:ADD_TASK , payload: title})
const addUser = (name) => ({type: ADD_TASK , payload: name})

const userReducer = (users = initialSate.users , action) => {
	if (action.type === ADD_USER) {
		return [...users, action.payload]
	}
	return users;
}

const taskReducer = (tasks = initialSate.tasks, action) => {
	if(action.type === ADD_TASK) {
		return [...tasks, action.payload]
	}
	return tasks
}

const reducer = combineReducers({users:userReducer,task:taskReducer})

const store = createStore(reducer);
```


### Enhancers

Add additional functionality to a redux store. Redux dev tool is a enhancer. enhancer warp every single call to the monitorReducer. This feature useful when developing plugins. Role of Enhancers is like a middleware.

Enhancers can use for many things, adding middleware, using the Redux DevTools, any kind of plugin that you wanna do or functionality that you need to add to Redux.

```javascript
const reducer = (state = {count :1}) => state;

const monitorEnhancer = (createStore) => (reducer, initialState, enhancer) => {

	const monitorReducer = (state, action) => {
		const start = performance.now();
		const newState = reducer(state,action)
		const end = performance.now()
		const diff = end - start;
		console.log(diff)
		return newState
	}
	return createStore(monitorReducer,initialState,enhancer )
}


const logEnhancer = (createStore) => (reducer, initialState, enhancer) => {
		const logReducer = (state, action) => {
			console.log('olf state',state, action)
			const newState = reducer(state,action)
			console.log('new state',newState, action)
			return newState;
		}

		return createStore(logEnhancer,initialState,enhancer )
	}

// There is only 1 enhancer argument. logEnhancer?
//const store = createStore(reducer, monitorEnhancer)

// To pass multiple enhancer pass the compose function
const store = createStore(reducer, compose(monitorEnhancer,logEnhancer))

```
### Apply Middleware

applyMiddleware takes a whole bunch of different middleware and create one composed enhancer out of all that middleware.

We use middleware to log customer behaviour to segment or something. middleware is really great way to modify how dispatching actions works in the application just as they flow thorough.

use logging actions data reporting tools

```javascript
const logMiddleware = store => next => action => {
	console.log('old state', store.getState(), action);
	next(action)
	console.log('new state', store.getState(), action);
}

const store = createStore(reducer, applyMiddleware(logMiddleware))
```

Redux Thunk is a usecase for the middleware.

multiple pisces of middleware

```javascript
const logMiddleware = store => next => action => {
	console.log('old state', store.getState(), action);
	next(action)
	console.log('new state', store.getState(), action);
}

// Snack is way to remember store => next => action => {}
const monitorMiddlewareMiddleware = (store) => next => action => {
	const start = performance,now();
	next(action);
	const end  = performance.now();
	const diff = end - start;
	console.log(diff);
}

const store = createStore(reducer, applyMiddleware(logMiddleware,monitorMiddlewareMiddleware))

```

Applying middleware is a abstraction over enhancers involves less boilerplate and is chainable out of all the middleware.

A reducer is a pure function that takes two inputs and make on output.