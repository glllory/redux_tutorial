# redux_tutorial

- How to create actions and action creators? 
- How to create a Redux store?
- How to dispatch my actions against the store?
- How to design state updates with pure reducers?
- How to manage complex state with reducer composition and handle asynchronous actions? 
**EXAMPLE :**
```js
const INCREMENT = 'INCREMENT'; // define a constant for increment action types
const DECREMENT = 'DECREMENT'; // define a constant for decrement action types

const counterReducer = (state = 0, action) => {
  switch(action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
}; // define the counter reducer which will increment or decrement the state based on the action it receives

const incAction = () => {
  return {type: INCREMENT};
} // define an action creator for incrementing

const decAction = () => {
  return {type: DECREMENT};
}; // define an action creator for decrementing

const store = Redux.createStore(counterReducer); // define the Redux store here, passing in your reducers
```
## Three Principles of Redux:
- Single source of truth:  
The state of your whole application is stored in an object tree within a single **store**.
- State is read-only:  
The only way to change the state is to emit an **action**, an object describing what happened.
- Changes are made with pure functions:  
To specify how the state tree is transformed by actions, you write pure **reducers**.

## How to Combine Multiple Reducers?
**EXAMPLE :**
```js
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const counterReducer = (state = 0, action) => {
  switch(action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
};

const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';

const authReducer = (state = {authenticated: false}, action) => {
  switch(action.type) {
    case LOGIN:
      return {
        authenticated: true
      }
    case LOGOUT:
      return {
        authenticated: false
      }
    default:
      return state;
  }
};

const rootReducer = Redux.combineReducers({
  count: counterReducer,
  auth: authReducer
});

const store = Redux.createStore(rootReducer);

```
## How to Send Action Data to the Store?
**EXAMPLE :**
```js
const ADD_NOTE = 'ADD_NOTE';

const notesReducer = (state = 'Initial State', action) => {
  switch(action.type) {
    // change code below this line
    case ADD_NOTE:
    return action.text;
    break;
    // change code above this line
    default:
      return state;
  }
};
// this is action creator 
const addNoteText = (note) => {
  // change code below this line
  return {type: ADD_NOTE,
  text: note};
  // change code above this line
};

const store = Redux.createStore(notesReducer);

console.log(store.getState());
store.dispatch(addNoteText('Hello!'));
console.log(store.getState());
```
## How to Use "Redux Thunk" as Middleware to Handle Asynchronous Actions?
**EXAMPLE :**
```js
const REQUESTING_DATA = 'REQUESTING_DATA'
const RECEIVED_DATA = 'RECEIVED_DATA'

const requestingData = () => { return {type: REQUESTING_DATA} }
const receivedData = (data) => { return {type: RECEIVED_DATA, users: data.users} }

const handleAsync = () => {
  return function(dispatch) {
    // dispatch request action here
    dispatch(requestingData());
    setTimeout(function() {
      let data = {
        users: ['Jeff', 'William', 'Alice']
      }
      // dispatch received data action here
      dispatch(receivedData(data));
    }, 2500);
  }
};

const defaultState = {
  fetching: false,
  users: []
};

const asyncDataReducer = (state = defaultState, action) => {
  switch(action.type) {
    case REQUESTING_DATA:
      return {
        fetching: true,
        users: []
      }
    case RECEIVED_DATA:
      return {
        fetching: false,
        users: action.users
      }
    default:
      return state;
  }
};

const store = Redux.createStore(
  asyncDataReducer,
  Redux.applyMiddleware(ReduxThunk.default)
);
```
