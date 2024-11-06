

---

## Node.js File System (fs)

In Node.js, the `fs` (File System) module allows you to interact with the file system on your machine. To access this functionality, you need to **require** the `fs` module at the top of your JavaScript file:

```js
const fs = require('fs');
```

### Reading Files

Node.js provides two primary ways to read files: **synchronous** and **asynchronous**. Each method has its use cases, and it’s important to choose the right one based on the context of your application.

---

#### 1. **Synchronous File Reading** (`readFileSync`)

The `readFileSync` method reads files synchronously. This means that your program will wait for the file reading operation to complete before moving to the next line of code. This method is typically used for reading small files or when you need to ensure that the file is fully loaded before continuing execution.

##### Syntax:
```js
fs.readFileSync(path, options);
```

- **`path`**: The path to the file you want to read.
- **`options`**: Optional. You can specify the encoding (e.g., `'utf8'`) or an object with both encoding and flag options.

##### Example:

```js
const fs = require('fs');

try {
  const data = fs.readFileSync('example.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error('Error reading file:', err);
}
```

**Note**: 
- `readFileSync` will block the execution of your program until the file is completely read. This could be problematic for larger files or when reading many files simultaneously, as it may negatively impact performance. For non-blocking operations, consider using asynchronous methods instead.

---

#### 2. **Asynchronous File Reading** (`readFile`)

The `readFile` method is non-blocking and reads files asynchronously. With this method, the program continues executing while the file is being read in the background, which is more efficient when dealing with multiple I/O operations. Once the file is read, a callback function is called with the result.

##### Syntax:
```js
fs.readFile(path, options, callback);
```

- **`path`**: The path to the file you want to read.
- **`options`**: Optional. Specifies the encoding or an object with encoding and flag options.
- **`callback`**: A function that gets executed once the file has been read. The callback takes two arguments: an error (if any) and the data (file content).

##### Example:

```js
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
  } else {
    console.log(data);
  }
});
```

In this example:
- The program continues executing while the file is being read.
- The callback function is executed once the file is read, either with an error or the data.

---

### Summary:

- **`readFileSync`**: Synchronously reads a file and blocks further execution until the file is fully loaded. Ideal for smaller files or cases where you need the file content immediately.
  
- **`readFile`**: Asynchronously reads a file, allowing the program to continue executing while the file is being read. This is preferred for non-blocking operations or when working with large files.

Choosing between synchronous and asynchronous methods depends on the size of the files, your app's performance requirements, and whether blocking the execution is acceptable.

---

# Node.js HTTP Server

## Overview

The server listens on port `8000` and responds with a specific message when the `/overview` URL path is requested. For any other URL paths, the server responds with a `404` error and a "Page Not Found" message.

### Import the HTTP Module

```js
const http = require('http');
```

- The `http` module is a built-in Node.js module that allows you to create HTTP servers and make requests. It provides the necessary functionality to set up and handle HTTP requests and responses.

### Create the HTTP Server

```js
const server = http.createServer((req, res) => {
    const pathname = req.url;
```

- `http.createServer()` is a method that creates an HTTP server. The function passed as a parameter to `createServer()` is a request handler that will be invoked every time a request is made to the server. It receives two arguments: `req` (request) and `res` (response).

- `req.url` is used to access the URL path of the incoming request. This determines which page or resource the client is trying to access.

### Handle the `/overview` Path

```js
if (pathname === '/overview') {
    res.end('This is the overview page');
}
```

- If the request URL (`req.url`) matches `/overview`, the server responds with the message "This is the overview page". The `res.end()` method is used to send the response back to the client and end the request.

### Handle Other Paths (404)

```js
else {
    res.writeHead(404, {
        'Content-type': 'text/html',
    });
    res.end("<h1>Page Not found<h1>");
}
```

- If the URL doesn't match `/overview`, the server responds with a `404 Not Found` error. `res.writeHead(404)` sets the HTTP status code to `404`, and the second argument defines the content type (in this case, HTML).
  
- Then, `res.end()` sends an HTML message indicating that the page was not found.

### Start the Server

```js
server.listen(8000, '127.0.0.1', () => {
    console.log("Listening to port 8000");
});
```

- The `server.listen()` method starts the server on the specified port (`8000`) and host (`127.0.0.1`, which is the local machine). When the server starts, it will log a message to the console: "Listening to port 8000".

## Conclusion

This simple Node.js HTTP server demonstrates how to create a basic server, handle routing for different URL paths, and send HTTP responses with different status codes. You can extend this server by adding more paths, handling different HTTP methods (GET, POST, etc.), or integrating a template engine for dynamic content.

---

