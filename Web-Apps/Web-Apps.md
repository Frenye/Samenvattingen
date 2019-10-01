# Web App Architecture

## Introduction

How do web clients and web servers talk to one another?

Our goal is to build a web application that clients around the globe can access.

## Client/Server

### Web Server

Takes a client request and gives something back to the client.

### Client

Lets the user request something on the server, and shows the user the result of the request.

![](./images/1.1clientserver.png)

## Clients and servers know HTML anh HTTP

#### HTML

1. Server answers request
2. Server sends content to browser
3. Browser displays

Servers oftend send the browser a set of instructions written in HTML. All web browsers know what to do with html.

#### HTTP

1. Client sends HTTP request
2. Server answers with HTTP response

When a web server sends an HTML page to the client, it sends it using HTTP.

### What is the HTTP protocol?

#### Key elements of the request stream

* HTTP method: the action to be performed
* The page to access: a URL
* Form parameters: like arguments to a method

#### Key elements of the response stream 

* A status code: for whether the request was succesfull
* Content-type: text, picture, ...
* The content; the actual HTML, images , ...


