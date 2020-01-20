February 10, 2019

 by Robin Wieruch


_Interested in reading this tutorial as one of many chapters in my advanced React with Firebase book? Checkout the entire [The Road to React with Firebase](https://roadtoreact.com/) book that teaches you to create business web applications without the need to create a backend application with a database yourself._

This tutorial is part 10 of 10 in this series.

The previous tutorial series covered a lot of ground for Firebase in React. So far, it was fine to rely only on React's local state and React's Context API. This tutorial dives into using Redux on top of React and Firebase for state management. You will exchange React's local state (e.g. users on admin page, messages on home page) and React's context (e.g. session management for authenticated user) with Redux. It will show you how to accomplish the same thing with Redux in case you want to integrate it into a tech stack.

This section is divided into two parts, the first of which will set up Redux. You will add the state layer separately from the view layer. Afterward, you will connect Redux with React by providing the Redux store with React's Context API to your React components. The second part exchanges the current React state layer with the Redux state layer:

*   Authenticated User in React Local State + React Context -> Authenticated User in Redux Store.
*   Users in React Local State -> Users in Redux Store.
*   Messages in React Local State -> Messages in Redux Store.

If you are not familiar with Redux, I recommend to check out [Taming the State in React](http://roadtoreact.com/). Most of the Redux knowledge about Actions, Reducers, and the Store are required for the following migration from only using React to Redux.

## Redux Setup in React Firebase Application

Let's get started by installing [redux](https://redux.js.org/) and [react-redux](https://github.com/reactjs/react-redux) on the command line:

npm install redux react\-redux

We focus on the Redux setup without worrying about Firebase or React. First is the Redux store implementation. Create a folder and file for it using the _src/_ folder type:

mkdir store

cd store

touch index.js

Second, add the store in the new file as singleton instance, because there should be only one Redux store. The store creation takes a root reducer which isn't defined.

```js
import  { createStore }  from  'redux';

import rootReducer from  '../reducers';

const store \=  createStore(rootReducer);

export  default store;
```

Third, create a dedicated module for the reducers. There's a reducer for the session state (e.g. authenticated user) and reducers for the user and message states (e.g. list of users and messages from the Firebase realtime database). There's an entry point file to the module to combine those reducers as root reducer to pass it to the Redux store, like the previous step. Again, from your _src/_ folder type:

mkdir reducers

cd reducers

touch index.js session.js user.js message.js

First, add the session reducer which manages the `authUser` object. The authenticated user represents the session in the application. The reducer deals only with one incoming action which either sets the `authUser` to the actual authenticated user or `null`:

```js
const  INITIAL\_STATE  \=  {

 authUser:  null,

};

const  applySetAuthUser  \=  (state, action)  \=>  ({

  ...state,

 authUser: action.authUser,

});

function  sessionReducer(state \= INITIAL\_STATE, action)  {

  switch  (action.type)  {

  case  'AUTH\_USER\_SET':  {

  return  applySetAuthUser(state, action);

  }

  default:

  return state;

  }

}

export  default sessionReducer;

The user reducer deals with the list of users from the Firebase realtime database. It sets either the whole object of users as dictionary, or a single user identified by a unique identifier:

const  INITIAL\_STATE  \=  {

 users:  null,

};

const  applySetUsers  \=  (state, action)  \=>  ({

  ...state,

 users: action.users,

});

const  applySetUser  \=  (state, action)  \=>  ({

  ...state,

 users:  {

  ...state.users,

  \[action.uid\]: action.user,

  },

});

function  userReducer(state \= INITIAL\_STATE, action)  {

  switch  (action.type)  {

  case  'USERS\_SET':  {

  return  applySetUsers(state, action);

  }

  case  'USER\_SET':  {

  return  applySetUser(state, action);

  }

  default:

  return state;

  }

}

export  default userReducer;
```

The message reducer deals with the list of messages from the Firebase realtime database. Again, it sets the whole object of messages as dictionary, but also a limit for the pagination feature we implemented earlier:

```js
const  INITIAL\_STATE  \=  {

 messages:  null,

 limit:  5,

};

const  applySetMessages  \=  (state, action)  \=>  ({

  ...state,

 messages: action.messages,

});

const  applySetMessagesLimit  \=  (state, action)  \=>  ({

  ...state,

 limit: action.limit,

});

function  messageReducer(state \= INITIAL\_STATE, action)  {

  switch  (action.type)  {

  case  'MESSAGES\_SET':  {

  return  applySetMessages(state, action);

  }

  case  'MESSAGES\_LIMIT\_SET':  {

  return  applySetMessagesLimit(state, action);

  }

  default:

  return state;

  }

}

export  default messageReducer;
```

Finally, combine all reducers into a root reducer in the _src/reducers/index.js_ file to make it accessible for the store creation:

```js
import  { combineReducers }  from  'redux';

import sessionReducer from  './session';

import userReducer from  './user';

import messageReducer from  './message';

const rootReducer \=  combineReducers({

 sessionState: sessionReducer,

 userState: userReducer,

 messageState: messageReducer,

});

export  default rootReducer;
```

You have passed the root reducer with all its reducers to the Redux store creation, so the Redux setup is done. Now you can connect your state layer with your view layer. The Redux store can be provided for the component hierarchy using Redux's Provider component. This time, the Provider component from the Redux library passes down the whole store instead of only the authenticated user. In _src/index.js_ file:

```js
import React from  'react';

import ReactDOM from  'react-dom';

import  { Provider }  from  'react-redux';

import store from  './store';

import App from  './components/App';

import Firebase,  { FirebaseContext }  from  './components/Firebase';

ReactDOM.render(

  <Provider  store\={store}\>

  <FirebaseContext.Provider  value\={new  Firebase()}\>

  <App  />

  </FirebaseContext.Provider\>

  </Provider\>,

 document.getElementById('root'),

);
```

That's it for connecting both worlds, so we'll refactor almost everything from React's local state to Redux. We want to have everything in the Redux store that should be persisted when we navigate from route to route. This includes users, messages, and the authenticated user, but maybe not the loading states.

## Manage Firebase's authenticated User in Redux Store

We are managing the authenticated user with React's Context API. We provide the authenticated user in a Provider component and consume it wherever we want with a Consumer component. Let's change this by storing the authenticated user in the Redux store instead and connecting all components that are interested in it to the Redux store. In the authentication higher-order component, we make the dispatchable action that stores the authenticated user in the Redux store, which is available as function in the props of the connected component:

```js
import React from  'react';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withFirebase }  from  '../Firebase';

const  withAuthentication  \=  Component  \=>  {

  class  WithAuthentication  extends  React.Component  {

  ...

  }

  const  mapDispatchToProps  \=  dispatch  \=>  ({

  onSetAuthUser:  authUser  \=>

  dispatch({ type:  'AUTH\_USER\_SET', authUser }),

  });

  return  compose(

 withFirebase,

  connect(

  null,

 mapDispatchToProps,

  ),

  )(WithAuthentication);

};

export  default withAuthentication;
```

Next, use the function to set the authenticated user in the Redux store by setting it to React's local state like before. We don't need to provide the authenticated user anymore with React's Context Provider component, because it will be available for every component that connects to the store:

```js
const  withAuthentication  \=  Component  \=>  {

  class  WithAuthentication  extends  React.Component  {

  constructor(props)  {

  super(props);

  this.props.onSetAuthUser(

  JSON.parse(localStorage.getItem('authUser')),

  );

  }

  componentDidMount()  {

  this.listener \=  this.props.firebase.onAuthUserListener(

  authUser  \=>  {

 localStorage.setItem('authUser',  JSON.stringify(authUser));

  this.props.onSetAuthUser(authUser);

  },

  ()  \=>  {

 localStorage.removeItem('authUser');

  this.props.onSetAuthUser(null);

  },

  );

  }

  componentWillUnmount()  {

  this.listener();

  }

  render()  {

  return  <Component  {...this.props}  />;

  }

  }

  ...

};

export  default withAuthentication;
```

That's it for storing and providing the authenticated user for the Redux store. Let's see how we can consume it in the Navigation component for the conditional rendering of the routes without React's Context, and with the Redux store instead:

```js
import React from  'react';

import  { Link }  from  'react-router-dom';

import  { connect }  from  'react-redux';

import SignOutButton from  '../SignOut';

import  \*  as  ROUTES  from  '../../constants/routes';

import  \*  as  ROLES  from  '../../constants/roles';

const  Navigation  \=  ({ authUser })  \=>

 authUser ?  (

  <NavigationAuth  authUser\={authUser}  />

  )  :  (

  <NavigationNonAuth  />

  );

...

const  mapStateToProps  \=  state  \=>  ({

 authUser: state.sessionState.authUser,

});

export  default  connect(mapStateToProps)(Navigation);
```

We can do the same in our other components that are interested in the authenticated user. For instance, the authorization higher-order component can rely on the Redux store as well:

```js
import React from  'react';

import  { withRouter }  from  'react-router-dom';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withFirebase }  from  '../Firebase';

import  \*  as  ROUTES  from  '../../constants/routes';

const  withAuthorization  \=  condition  \=>  Component  \=>  {

  class  WithAuthorization  extends  React.Component  {

  ...

  render()  {

  return  condition(this.props.authUser)  ?  (

  <Component  {...this.props}  />

  )  :  null;

  }

  }

  const  mapStateToProps  \=  state  \=>  ({

 authUser: state.sessionState.authUser,

  });

  return  compose(

 withRouter,

 withFirebase,

  connect(mapStateToProps),

  )(WithAuthorization);

};

export  default withAuthorization;

import React from  'react';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withFirebase }  from  '../Firebase';

...

const  withEmailVerification  \=  Component  \=>  {

  class  WithEmailVerification  extends  React.Component  {

  ...

  render()  {

  return  needsEmailVerification(this.props.authUser)  ?  (  ...  )  :  (

  <Component  {...this.props}  />

  );

  }

  }

  const  mapStateToProps  \=  state  \=>  ({

 authUser: state.sessionState.authUser,

  });

  return  compose(

 withFirebase,

  connect(mapStateToProps),

  )(WithEmailVerification);

};

export  default withEmailVerification;
```

And last but not least, the AccountPage component which displays the authenticated user but also renders the component that manages all the sign in methods for the user:

```js
import React,  { Component }  from  'react';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withAuthorization, withEmailVerification }  from  '../Session';

import  { withFirebase }  from  '../Firebase';

import  { PasswordForgetForm }  from  '../PasswordForget';

import PasswordChangeForm from  '../PasswordChange';

...

const  AccountPage  \=  ({ authUser })  \=>  (

  <div\>

  <h1\>Account:  {authUser.email}</h1\>

  <PasswordForgetForm  />

  <PasswordChangeForm  />

  <LoginManagement  authUser\={authUser}  />

  </div\>

);

...

const  mapStateToProps  \=  state  \=>  ({

 authUser: state.sessionState.authUser,

});

const  condition  \=  authUser  \=>  !!authUser;

export  default  compose(

  connect(mapStateToProps),

 withEmailVerification,

  withAuthorization(condition),

)(AccountPage);

Now you can remove the React Context for providing and consuming the authenticated user in the _src/components/Session/context.js_ and _src/components/Session/index.js_ files:

import withAuthentication from  './withAuthentication';

import withAuthorization from  './withAuthorization';

import withEmailVerification from  './withEmailVerification';

export  {

 withAuthentication,

 withAuthorization,

 withEmailVerification,

};
```

That's it for storing the authenticated user in the Redux store, which takes place in the authentication higher-order component and for consuming the authenticated user in every component which is interested in it by connecting the Redux store.

## Manage Firebase's Users in Redux Store

We implemented the session management with the authenticated user with Redux instead of React's local state and context API. Next, we will migrate the user management over to Redux. The users are mainly used in the AdminPage component's UserList and UserItem components. Our goal here is to navigate from UserList to UserItem and back with React Router without losing the state of the users. The UserList component fetches and shows a list of users, while the UserItem component fetches and shows a single user entity. If the data is already available in the Redux store, we only keep track of new data with the realtime feature of the Firebase database, starting with the UserList component:

```js
import React,  { Component }  from  'react';

import  { Link }  from  'react-router-dom';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withFirebase }  from  '../Firebase';

import  \*  as  ROUTES  from  '../../constants/routes';

class  UserList  extends  Component  {

  ...

}

const  mapStateToProps  \=  state  \=>  ({

 users: Object.keys(state.userState.users ||  {}).map(key  \=>  ({

  ...state.userState.users\[key\],

 uid: key,

  })),

});

const  mapDispatchToProps  \=  dispatch  \=>  ({

  onSetUsers:  users  \=>  dispatch({ type:  'USERS\_SET', users }),

});

export  default  compose(

 withFirebase,

  connect(

 mapStateToProps,

 mapDispatchToProps,

  ),

)(UserList);
```

React Redux's connect higher-order component is used to marry React with Redux. We can tell what state of Redux should be mapped to props for the React component in the `mapStateToProps` function, and we can pass dispatchable Redux actions as functions to the React component as props with the `mapDispatchToProps` function. In our case, we are interested in a user object that encapsulates all users in the Redux store. We transform this user object--which is the Firebase representation of all users--into an array, to make it easier for us to render them. The point is to dispatch an action that sets the user object as state in the Redux store. Check t he _src/reducers/user.js_ to see how our reducer deals with this action. Both `users` and `onSetUsers` are received as props in the UserList component.

Next, make sure the users are fetched from Firebase's realtime database and persisted in the Redux store with our new dispatchable action:

```js
class  UserList  extends  Component  {

  componentDidMount()  {

  this.props.firebase.users().on('value',  snapshot  \=>  {

  this.props.onSetUsers(snapshot.val());

  });

  }

  componentWillUnmount()  {

  this.props.firebase.users().off();

  }

  ...

}
```

Each time the Firebase listener is called, or when a user was added, edited, or removed from the list, the most recent user object that has all users from Firebase is stored with the `onSetUsers()` function to the Redux store. Another UX improvement is the loading indicator when there are no users in the Redux store. Every other time, when there are users in the store but the Firebase listener is updating the Redux store with a new user object, no loading indicator is shown:

```js
class  UserList  extends  Component  {

  constructor(props)  {

  super(props);

  this.state \=  {

 loading:  false,

  };

  }

  componentDidMount()  {

  if  (!this.props.users.length)  {

  this.setState({ loading:  true  });

  }

  this.props.firebase.users().on('value',  snapshot  \=>  {

  this.props.onSetUsers(snapshot.val());

  this.setState({ loading:  false  });

  });

  }

  ...

}
```

The users are no longer managed in the local state of the component, but are now handled in Redux. You set the users with a dispatchable action from `mapDispatchToProps` and access them again in `mapStateToProps`. Both state and actions are passed as props to your component.

The users and loading indicator are rendered as before, but only the loading state comes from the local state. The Link component only navigates to the UserItem component, but it doesn't send any user objects. We wanted the user at our disposal via the Link component, and we want to let Redux handle it.

```js
class  UserList  extends  Component  {

  render()  {

  const  { users }  \=  this.props;

  const  { loading }  \=  this.state;

  return  (

  <div\>

  <h2\>Users</h2\>

  {loading &&  <div\>Loading ...</div\>}

  <ul\>

  {users.map(user  \=>  (

  <li  key\={user.uid}\>

  <span\>

  <strong\>ID:</strong\>  {user.uid}

  </span\>

  <span\>

  <strong\>E\-Mail:</strong\>  {user.email}

  </span\>

  <span\>

  <strong\>Username:</strong\>  {user.username}

  </span\>

  <span\>

  <Link  to\={\`${ROUTES.ADMIN}/${user.uid}\`}\>

 Details

  </Link\>

  </span\>

  </li\>

  ))}

  </ul\>

  </div\>

  );

  }

}
```

The UserList component renders a list of users as before, fetches the recent user object, which has all users, from Firebase with a realtime connection, but stores the result into the Redux store this time instead of React's local state. Let's continue with the UserItem component that shall be connected to the Redux store too:

```js
import React,  { Component }  from  'react';

import  { connect }  from  'react-redux';

import  { compose }  from  'recompose';

import  { withFirebase }  from  '../Firebase';

class  UserItem  extends  Component  {

  ...

}

const  mapStateToProps  \=  (state, props)  \=>  ({

 user:  (state.userState.users ||  {})\[props.match.params.id\],

});

const  mapDispatchToProps  \=  dispatch  \=>  ({

  onSetUser:  (user, uid)  \=>  dispatch({ type:  'USER\_SET', user, uid }),

});

export  default  compose(

 withFirebase,

  connect(

 mapStateToProps,

 mapDispatchToProps,

  ),

)(UserItem);
```
