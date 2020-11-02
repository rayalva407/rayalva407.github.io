---
layout: post
title:      "Using Fetch for CRUD operations"
date:       2020-11-02 18:18:26 +0000
permalink:  using_fetch_for_crud_operations
---


Starting the Javascript module at Flatiron School was a bit daunting. The fundamentals were easier to understand but learning how to manipulate the DOM and work with APIs was completely new for me. Thankfully there were some concepts that were kind of familiar. One of the things I like learning during this module was how perform CRUD operations using fetch. 
## What is CRUD?
CRUD stands for Create, Read, Update and Delete.

* **Create:** Creates new data
* **Read:** Shows you the data
* **Update:** Updates existing data
* **Delete:** Deletes existing data

## HTTP Request methods
These operations are associated with the HTTP request methods GET, POST, PUT, and DELETE. 

* **GET** This method is used to request data from a resource
* **POST** This method is used to send data to create a resource in the server
* **PUT** This method is used to update a resource in the server
* **DELETE** This method deletes a specified resource

Since we are going to be performing CRUD operations using Fetch we need to use a REST API server. To show you what these operations look like using fetch I will be using JSONPlaceholder. JSONPlaceholder is a free online REST API that can be used to mess around with some fake data. It is a very satisfying and fun way to learn these concepts this way. We will be using the Album and Photos example API.

## Fetch()
This is how a simple fetch is structured.

```
/* We call fetch function and pass the url of the API as a parameter */
fetch(url)
    .then(() => {
		    /* The code we use here will handle the data that you get from the API */
		});
		.catch(() => { 
		    /* This is where we catch any errors if something goes wrong */
		});
```

## GET Albums
```
fetch("https://jsonplaceholder.typicode.com/albums")
    /* Below we take the response and use the .json method. This returns a promise that resolves with parsing the response to JSON*/
    .then(response => response.json())
		.then(data => {
		    console.log(data);
				})
		.catch(error => {
		    console.log(error);
		});

```

By plugging this code into the console we get all of the albums

```
0: {userId: 1, id: 1, title: "quidem molestiae enim"}
1: {userId: 1, id: 2, title: "sunt qui excepturi placeat culpa"}
2: {userId: 1, id: 3, title: "omnis laborum odio"}
3: {userId: 1, id: 4, title: "non esse culpa molestiae omnis sed optio"}
4: {userId: 1, id: 5, title: "eaque aut omnis a"}
5: {userId: 1, id: 6, title: "natus impedit quibusdam illo est"}
6: {userId: 1, id: 7, title: "quibusdam autem aliquid et et quia"}
7: {userId: 1, id: 8, title: "qui fuga est a eum"}
8: {userId: 1, id: 9, title: "saepe unde necessitatibus rem"}
9: {userId: 1, id: 10, title: "distinctio laborum qui"}
```

## Create an Album

This is a little different. In this fetch request we have to specify the HTTP method and pass in the information in the body. Along with the url, we can pass the method, body, and headers as a config object. It would look something like this.

```
  let configObj = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json"
    },
    body: JSON.stringify({
      name: "Album Title",
      userId: 1
    })
  };

  fetch("https://jsonplaceholder.typicode.com/albums", configObj)
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => alert(error.message))
```
First a Promise is returned. Once that is resolved we get the created album

```
Promise {<pending>}
    __proto__: Promise
        [[PromiseState]]: "fulfilled"
        [[PromiseResult]]: undefined

{name: "Album Title", userId: 1, id: 101}
    id: 101
    name: "Album Title"
    userId: 1
    __proto__: Object
```

## UPDATE an Album

Updating actually looks very similar to creating but instead we pass in the PUT HTTP method.

```
  let configObj = {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json"
    },
    body: JSON.stringify({
      name: "Album Title",
      userId: 1
    })
  };

  fetch("https://jsonplaceholder.typicode.com/albums", configObj)
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => alert(error.message))
```

## DELETE an Album
The url here is different. Since we want to specify a specific Album we add /1 to the end of the url.

```
fetch ("https://jsonplaceholder.typicode.com/albums/1", {
    method: "DELETE"
})
```



