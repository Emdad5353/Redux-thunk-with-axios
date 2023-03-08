# Redux-thunk-with-axios

A practice project of redux with redux thunk and axios api call

Redux Thunk is a middleware library for Redux that allows you to write asynchronous logic that interacts with the Redux store. 
Here are some common use cases for Redux Thunk:

1. Asynchronous actions: Redux Thunk allows you to write action creators that return functions instead of plain objects. This makes it easy to write asynchronous logic such as API calls or timers in your Redux actions.

2. Conditional logic: With Redux Thunk, you can write action creators that perform different logic based on certain conditions. For example, you could write an action creator that dispatches a certain action if a condition is met, or a different action if the condition is not met.

3. Dispatching multiple actions: Redux Thunk allows you to dispatch multiple actions from a single action creator. This can be useful if you need to dispatch several actions in response to a single event.

4. Accessing the Redux store: With Redux Thunk, you can access the current state of the Redux store and dispatch actions based on that state. This can be useful if you need to perform complex logic based on the current state of your application.


npm install redux

npm install axios

npm install redux-thunk




  ```js
  // async actions - api calling
  // api url - https://jsonplaceholder.typicode.com/todos
  // middleware- redux-thunk
  // axios api

  const { default: axios } = require("axios");
  const { createStore, applyMiddleware } = require("redux");
  const reduxThunk = require("redux-thunk").default;

  // define constants
  const GET_TODOS_REQUEST = "GET_TODOS_REQUEST";
  const GET_TODOS_SUCCESS = "GET_TODOS_SUCCESS";
  const GET_TODOS_FAILED = "GET_TODOS_FAILED";
  const TODOS_URL = "https://jsonplaceholder.typicode.com/todos";

  // define state
  const initialTodosState = {
    todos: [],
    isLoading: false,
    error: null,
  };

  const getTodosRequest = () => {
    return {
      type: GET_TODOS_REQUEST,
    };
  };

  const getTodosSuccess = (todos) => {
    return {
      type: GET_TODOS_SUCCESS,
      payload: todos,
    };
  };
  const getTodosFailed = (error) => {
    return {
      type: GET_TODOS_FAILED,
      payload: error,
    };
  };

  const todosReducer = (state = initialTodosState, action) => {
    switch (action.type) {
      case GET_TODOS_REQUEST:
        return {
          ...state,
          isLoading: true,
        };
      case GET_TODOS_SUCCESS:
        return {
          ...state,
          todos: action.payload,
          isLoading: false,
        };
      case GET_TODOS_FAILED:
        return {
          ...state,
          isLoading: false,
          error: action.payload,
        };

      default:
        state;
    }
  };

  // async action creator
  // thunk-middleware allows us to return a function isntead of obejct
  const fetchData = () => {
    return (dispatch) => {
      dispatch(getTodosRequest());
      axios
        .get(TODOS_URL)
        .then((res) => {
          const todos = res.data;
          const titles = todos.map((todo) => todo.title);
          dispatch(getTodosSuccess(titles));
        })
        .catch((err) => {
          const error = err.message;
          dispatch(getTodosFailed(error));
        });
    };
  };

  const store = createStore(todosReducer, applyMiddleware(reduxThunk));

  store.subscribe(() => {
    console.log(store.getState());
  });

  store.dispatch(fetchData());
  ```
