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

We will use JS's fetch() function within an action-creator function to query the API. Below might seem to be the most straightforward strategy (ensure www.api.com is rendering a JSON response and not HTML): 

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

```
export function fetchData() {
 const data = fetch('http://www.api.com').then(response => {
  return {
	 type: 'FETCH_DATA', 
	 data
	 }
	});
 }
}
```
