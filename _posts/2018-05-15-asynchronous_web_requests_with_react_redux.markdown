---
layout: post
title:      "Asynchronous Web Requests with React/Redux"
date:       2018-05-15 15:38:11 -0400
permalink:  asynchronous_web_requests_with_react_redux
---


When building a React/Redux front end my Rails back end project, WineFinder, I found async web requests to be one of the harder concepts to internalize. Below is my attempt at a guide:  

Let's say our default Redux state is an empty array: 
```
state = [ ]
```

Our goal is to populate the array with objects from our back-end API:
```
state = [obj1, obj2, obj3, ...]
```

We will use JS's fetch() function within an action-creator function to query the API. Below might seem to be the most straightforward strategy. Additionally, ensure www.api.com is rendering a JSON response and not HTML, and ensure you have a Redux reducer function that can handle the action we're returning. 

```
export function fetchData() {
 const data = fetch('http://www.api.com');
 return {
  type: 'FETCH_DATA',
  data
 }
}
```

However, this won't work. Reason is that fetch() is asynchronous - it returns a Promise object which initially doesn't contain the data we want. Only after a few moments, the Promise resolves and contains the useful data. However, if you don't handle this properly, subsequent code will run before the useful data is obtained. The above code sets 'data' equal to an [unresolved] Promise, and proceeds to return this useless Promise within an action. 

To solve this issue, we can use .then(), which takes a function as an argument and executes that function only when the Promise has been resolved and the response received. 

```
export function fetchData() {
 fetch('http://www.api.com')
 .then(response => response.json())
 .then(data => {
  return {
  type: 'FETCH_DATA'
  data: data
  }
 }
}
```

By enclosing the return statement in .then(), we make sure that the returned action data key is pointing to valid data. Although we have solved the issue of asynchrony, there is another objective we'd like to accomplish as well - we want to be able to render our page before the fetchData() function loads data in case there is a lot of data and it takes a while to load. To do this, let's first set up our action function to return a function instead of an action: 

**Import Thunk Middleware:**
By default, the program will return an error if we try to return a function from our action function (it is expecting an action object). Thunk enables our action function to return a function instead of an action object. 
```
import thunk from 'redux-thunk'; //Paste this line at the top of index.js
```

**Connect fetchData() to Redux State:**
The code below gives our action function access to the Redux state dispatch method
```
//In a ReactComponent.js file that will be calling 

import { connect } from 'react-redux'
import { bindActionCreators } from 'redux'
import { fetchData } from '../actions/fetchData' //Simply importing the file that contains fetchData()

//class ReactComponent extends Component {...}

const mapDispatchToProps = (dispatch) => {
 return bindActionCreators({
  fetchData: fetchData
 }
}

export default connect(null, mapDispatchToProps)(ReactComponent)
```

**Back to fetchData():**
The code below was refactored such that (1) it returns a function (instead of an action object) that takes the Redux dispatch as an argument, (2) immediately calls the dispatch function and passes a 'LOADING_DATA' action type, (3) sends a request to the API and uses .then() to handle asynchrony and ultimately pass the desired data to Redux state.
```
export function fetchData() {
 return(dispatch) => {
  dispatch({type: 'LOADING_DATA'}); //Modify your reducer to handle this
	return fetch 
 }
 fetch('http://www.api.com')
 .then(response => response.json())
 .then(data => {
  return {
   type: 'FETCH_DATA'
   data: data
  }
 }
}
```

You can use the 'LOADING DATA' action type to let your state/program know that data is being loaded and design your program to render some specific content while data is being loaded, and then use the 'FETCH_DATA' action to tell the program to render the final content once the data has been loaded. 

