# Basic Concepts

#### Request / Response Data Flow

In a standard redux project data flows like this:

1. Dispatch an `attempting` action that will attempt to `POST`/`GET` data from a server. This action will usually flip an `attempting: true` flag in it's domain-related reducer in case we want to display some sort of UI to demonstrate to the user that a request is in process. Note - we don't always need to have an attempting action. This depends on the UI generally.

2. A saga is listens for this `attempting` action, and then runs when it receives it along with it's params (*it will have passed through the reducer first*). The saga handles all side-effects associated with this action. The saga will be the place where the api call is actually made. We've been using [Apisauce](https://github.com/skellock/apisauce) as our API library to deal with RESTful APIs. Depending on it's success or failure a `success` or `failure` action will be dispatched to the reducer. If it's a `success` we dispatch the received data as well, if it's a `failure`, we display the received error messages.

3. After the `success` action is dispatched from the reducer, the data will get stored as is or [normalized](https://github.com/paularmstrong/normalizr) and stored. We try to normalize all deeply nested data to make it much easier to work with. If the data is't deeply nested, nor relational, we do not need to normalize. The `success` or `failure` action will also flip that `attempting: true` flag mentioned above back to `attempting: false` to show the user that the request has completed.

4. Now that we have attempted to fetch our data, successfully received our data, and stored (or normalized) it, depending on how we are using it, we may need to *transform* the data before displaying or performing calculations with it.
In a perfect world, we would not need to do many transformations since the API would return to us the data exactly as we needed *cough GraphQL*. However, often transforms are necessary to coerce the data into a more workable form. For example, List Views in React Native require very specific data structures, and we often have to transform our data to fit a List View's requirements.  

We may also want to add [Reselect](https://github.com/reactjs/reselect) to our transforms. Reselect makes our transforms way more efficient since they are memoized and will not have to be computed multiple times upon component-rerender if thats happening a lot (as it usually does with React).

5. The cycle is now complete. To recap: we first dispatch a redux action which may trigger an attempting flag. The saga that is listening for this action receives it and any params passed in and makes the actual API call. Regardless of success or failure, the saga will dispatch an action back to the reducer with the result of the API call. If it was a success we may normalize the data before we store it. If it was a failure, we may display the error messages. Finally, depending on how we want to use the data, we may transform it before using it in the UI.

Sagas help make the async environment of JS a bit more bearable when dealing with things like APIs since it helps to coordinate all the different APIs in tandem.

With sagas, we can allow certain function calls to be 'blocking' (and therefore synchronous ie. call) so that it is more predictable, but we could also dispatch async calls (ie. put) as well.

#### Apisauce

We use [Apisauce](https://github.com/skellock/apisauce) as our API library. Apisauce wraps Axios and makes it easier to work with sagas. Apisauce provides a standardized response object to all API calls.

#### Redux Saga

[Redux Sagas](https://github.com/yelouafi/redux-saga) are middleware for handling side-effects in Redux.

##### Handling API calls with Sagas

All API calls are handled by Sagas.

There are generally three parts to handling an api call in react-native.

1. The first phase is the request **attempt** itself.
2. The second is to handle the **success** of the call with the successful payload response.
3. The third is to handle the **failure** of the call, with an error payload response.

All three of these cases involve separate state changes.

In order to handle these three aspects, each API call generally has three different `action creators` associated with it. Read more about action creators here: [Action Creators](http://redux.js.org/docs/basics/Actions.html)

For example, in fetching and loading an application we have `attemptCurrentApplicationGet`, `currentApplicationGetSuccess`, and `currentApplicationGetFailure`.

##### Example

Here is an example of fetching and loading an application.

##### Attempting the API Call

The first action we dispatch when we want to load an application is the **attempt** action. Since this action also modifies the global state, we are able transition other parts of the app at the same time. By listening to the **attempting** state slice, we display a loading animation when it is true, since we know that we are **attempting** to load data.

##### Authorizing the API Call

The saga listens for the `attemptCurrentApplicationGet` action. When it receives it, it checks if the user is authorized to make the API call by determining if they have been inactive for too long or not (see Authorization of Users). If the user is authorized to make the call, the call is made. After the call is made, we check if there have been any errors related to authorization or connection errors. If there has been a connection error, we dispatch a flash message which allows the user to **retry** the request again (see Retrying).

##### Handling the Success or Failure of the API Call

If there has been no server errors (i.e no connection errors, and no authorization errors), we check if there were any client errors. If there were no client errors, this means we have successfully made an api call. With the payload, we would then dispatch a success action `currentApplicationGetSuccess` (and whatever else we want) and load the data into our reducer.

If there has been a client error (the payload was malformed or something), we can handle the error by dispatching a flash message, and `currentApplicationGetFailure`, or anything else we might want to do in this case.

##### Summary

Sagas are where we make all API calls, handle the loading of all data, and determine if the user is authorized to load any of this data. Sagas work in tandem with reducers and action creators to create a pipeline of dispatching actions, waiting for the success & failure, and then proceeding accordingly: displaying the new data, or displaying an error flash message, etc.
