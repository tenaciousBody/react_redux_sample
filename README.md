To implement redux in any of the react application, we need to take care of following things.
In Redux, we need to implement three things, 
1.Actions, 
2.ReducerFunction
3.Store

Store is a container which holds the entires state of your application. Reducer function is the one which describes how the state of the application will get changed
based on the action disapteched by the application. 
As applications subscribes the changes happening for the states reside in the redux store. Application is aware of these state changes.

we need to add following dependencies in our react application - 
    "redux": "^4.0.4",
    "redux-logger": "^3.0.6",
    "redux-thunk": "^2.3.0",
    "react-redux": "^8.0.1",
    
Once Dependencies are added, we should follow below steps;
* create action types within you application for the action creators.
* Obce action types are created, create action creators by using the action types. Action creators are actions based on which state of your application will be updated.
Below is the sample code for action creators and action types. 
/* action types*/

export const BUY_CAKE = 'BUY_CAKE'

/*action creators*/
export const buyCake = (number = 1) => {
  return {
    type: BUY_CAKE,
    payload: number
  }
}

* once you are done with the action creators, you need to create reducer function for your component, it takes two argument, initial state and the actions. reducer function describes how state of your application will be changing based on the actions dispatched by the react component. 

Below is the sample code for a reducer function

/* reducer*/
const cakeReducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_CAKE: return {
      ...state,
      numOfCakes: state.numOfCakes - action.payload
    }

    default: return state
  }
}

*Post that, Store needs to be created and below is the sample code for that. 

import { createStore, applyMiddleware } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import logger from 'redux-logger'
import thunk from 'redux-thunk'

import rootReducer from './rootReducer'

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(logger, thunk))
)

*Once store is created we need to provide it to the component wraaped in App.js like below
  <Provider store={store}>
      <div className='App'>
        <UsersContainer />
      </div>
    </Provider>
    
*Till this point, we have defined actions, reducer function as well as redux store for our application. 
* Now, we need to connect our components to redux store & below is the code for the same. 

/*code to connect your component with the redux store */
//mapStateToProps return a variable which represents the current state of your application
const mapStateToProps = state => {
  return {
    userData: state.user
  }
}
//mapDispatchToProps also returns a variable via which you can dispatch actions(the actions you defined earlier) from your component.
const mapDispatchToProps = dispatch => {
  return {
    fetchUsers: () => dispatch(fetchUsers())
  }
}
we can access userData and fetchUsers as props in our component.

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(UsersContainer)
