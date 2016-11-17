# Learn-React-Native-3-Redux
A simple app to display titles and descriptions using redux to manage state. 

Reducers are called once on boot up to set an initial state.    

// reducers are always called with the state as the first parameter
// They are called once when the app begins
    const reducer = (state = [], action) => {
      if (action.type === 'split_string') {
        return action.payload.split('');
      }
      return state;
    }

    const store = Redux.createStore(reducer);
    store.getState( );

    const action = {
      type: 'split_string',
      payload: 'asdf'
    };

    store.dispatch(action);

    store.getState();

Actions are Objects and require a ‘type' property

Whenever we make any change in the state, we must return a completely new
object. We do NOT mutate our data. we make an entirely new object.

Therefore: 

    const reducer = (state = [ ], action) => {
          if (action.type === 'split_string') {
            return action.payload.split('');
         } else if (action.type === 'add_character') {
           return [ ...state, action.payload ];
         }
         return state;
    }

NOTE: The above line creates a new array of all the elements from state and appends
            to it: action.payload
            
            
 // app.js
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import { reducers } from './reducers';

// providers facilitate communication between react an redux
<Provider store={createStore(reducers)}>

// src/reducers/index.js
import { combineReducers } from 'redux’;

export default combineReducers({
  libraries: () => []
});

NOTE: Provider can only have 1 direct child element

Reducers provide application state. They must not return undefined

//SelectionReducer.js
    export default () => {
      return null;
    }


          //  CONNECT  //  to get state from provider.

import { connect } from 'react-redux';

class LibraryList extends Component {
  render() {
    console.log(this.props);
    return;
  }
}

const mapStateToProps = state => {
  return { libraries: state.libraries };
};

export default connect(mapStateToProps)(LibraryList);

  // ACTION CREATORS  //

//Component.js

// This gives every action from the index.js inside actions
    import * as actions from '../actions’

    import { connect } from 'react-redux’;

// The first argument to connect here must be the mapStateToProps fn
// the second argument is the actions object
    export default connect(null, actions)(Component);

NOTE:  1. Makes the Action Creator dispatch the action to the redux store
       2. Pass the actions to the Component as ‘props'

                <TouchableWithoutFeedback
          onPress={() => this.props.selectLibrary(id)}
        >

//   src/actions/index.js
    export const selectLibrary = (libraryId) => {
      return {
        type: 'select_library',
        payload: libraryId
      };
    };

// src/reducers/SelectionReducer.js
    export default (state = null, action) => {
      switch (action.type) {
        case 'select_library':
          return action.payload;
        default:
          return state;
     }
 };

NOTE: We set a default value for state in this reducer using es6 notation.

THEN: we can use the mapStateToProps  function with a second argument: ownProps

const mapStateToProps = (state, ownProps) => {
  const expanded = state.selectedLibraryId === ownProps.library.id
  return { expanded };
};

Always a good idea to move logic from render function to mapStateToProps
