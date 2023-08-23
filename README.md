# Repo for asynkron programmering

## How can i make async calls using http from a html page to my node.js server

To make asynchronous calls from an HTML page to a Node.js server, you can use JavaScript's fetch() API or other similar methods. Here's a step-by-step guide on how to achieve this:

**1. Set Up Your Node.js Server:**

Make sure you have a Node.js server running that can handle HTTP requests. You can use a framework like Express.js for this purpose.

**2. Create an API Endpoint:**

Set up an API endpoint on your Node.js server that the HTML page can send requests to. For example, using Express.js, you might define a route like:

```javascript
const express = require('express');
const app = express();

app.get('/api/data', (req, res) => {
  // Process the request and send back a response
  res.json({ message: 'Data from server' });
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

**3. Write JavaScript in HTML:**

In your HTML file, you can use the fetch() API to make asynchronous requests to your Node.js server.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Async HTTP Call</title>
</head>
<body>
  <button id="getDataBtn">Get Data</button>
  <div id="dataDisplay"></div>

  <script>
    document.getElementById('getDataBtn').addEventListener('click', async () => {
      try {
        const response = await fetch('/api/data'); // Make the API request
        const data = await response.json(); // Parse the response as JSON
        document.getElementById('dataDisplay').textContent = data.message;
      } catch (error) {
        console.error('Error:', error);
      }
    });
  </script>
</body>
</html>
```

**Run the Server and Open the HTML Page:**

Run your Node.js server using the appropriate command (e.g., node server.js). Then, open the HTML page in a web browser and click the "Get Data" button. You should see the data returned from the server displayed on the page.

## Show me also an example with POST and it must be async

Certainly! Here's an example that demonstrates how to make an asynchronous POST request from an HTML page to a Node.js server using the fetch() API:

**1. Set Up Your Node.js Server:**

Follow the same steps as before to set up your Node.js server with Express.js.

**2. Create an API Endpoint for POST:**

Define a route on the server that handles POST requests and returns a response.

```javascript
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.post('/api/submit', (req, res) => {
  const { message } = req.body;
  console.log('Received:', message);
  res.json({ received: true });
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

**3. HTML with Asynchronous POST Request:**

Create an HTML file with a form that allows the user to input a message and submit it to the server using a POST request.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Async POST Request</title>
</head>
<body>
  <form id="messageForm">
    <input type="text" id="messageInput" placeholder="Enter your message" />
    <button type="submit">Submit</button>
  </form>
  <div id="responseDisplay"></div>

  <script>
    document.getElementById('messageForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      const message = document.getElementById('messageInput').value;

      try {
        const response = await fetch('/api/submit', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ message }) // Send the message as JSON in the request body
        });

        const responseData = await response.json();
        if (responseData.received) {
          document.getElementById('responseDisplay').textContent = 'Message sent successfully!';
        }
      } catch (error) {
        console.error('Error:', error);
      }
    });
  </script>
</body>
</html>
```

**4. Run the Server and Open the HTML Page:**

Start your Node.js server and open the HTML page in a web browser. Enter a message in the input field and click the "Submit" button. You should see a success message indicating that the message was sent successfully to the server.

This example demonstrates how to make an asynchronous POST request from an HTML page to a Node.js server using the fetch() API. 

## Why is try catch good for async calls

Using a 'try' ... 'catch' block around asynchronous calls is a good practice because it helps you manage errors that might occur during the execution of the asynchronous code. Asynchronous operations, such as fetching data from a server, are not guaranteed to be successful due to various factors like network issues, server errors, or incorrect data handling. Here's why using try...catch is beneficial:

**1. Error Handling:**

Asynchronous operations can fail for various reasons, and if not properly handled, they can cause your application to break or behave unexpectedly. Wrapping asynchronous calls in a try...catch block allows you to catch and handle errors that might occur during the execution of the code.

**2. Prevent Unhandled Promise Rejections:** 

If an error occurs in an asynchronous operation and is not caught using a try...catch block or by handling the promise's rejection, it will result in an "unhandled promise rejection" error. This error can cause unintended consequences and may not provide helpful information about the root cause of the issue.

**3. User Experience:**

By catching errors, you can provide a better user experience by displaying relevant error messages or performing fallback actions. This way, users are informed about what went wrong instead of facing a broken or unresponsive interface.

**4. Debugging:**

When errors occur in asynchronous code, it can sometimes be challenging to pinpoint the exact cause. By catching and logging errors, you can gather more information about what went wrong, making it easier to diagnose and fix issues.

Here's an example demonstrating how a 'try' ... 'catch' block can be used with asynchronous code:

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log('Data:', data);
  } catch (error) {
    console.error('Error:', error);
    // Perform error handling, display user-friendly message, etc.
  }
}

fetchData();
```

In this example, if an error occurs during the fetching or parsing of data, the catch block will be triggered, allowing you to handle the error gracefully. This approach ensures that your application continues to function even if certain parts encounter errors.

In summary, using a try...catch block around asynchronous calls is an important practice to ensure proper error handling, prevent unhandled promise rejections, improve user experience, and aid in debugging.

## What does async fetch call return

An async fetch call using the fetch API returns a Promise. This Promise resolves to the Response object representing the response to the network request. The Response object contains information about the response, such as its status, headers, and the body of the response.

You can then use methods on the Response object to extract data from the response body. The most common methods used are .json(), .text(), and .blob(), depending on the type of data you're expecting from the server.

Here's a breakdown of the typical usage:

**1. Using .json() Method:**

If you expect the response to be JSON data, you can use the .json() method to parse the response body and return a Promise that resolves to the parsed JSON data.

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json()) // Parse response as JSON
  .then(data => {
    console.log('JSON Data:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

**2. Using .text() Method:**

If you expect the response to be plain text, you can use the .text() method to retrieve the response body as text.

```javascript
fetch('https://api.example.com/text')
  .then(response => response.text()) // Get response as text
  .then(text => {
    console.log('Text Data:', text);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

**3. Using .blob() Method:**

If you expect the response to be binary data (such as images), you can use the .blob() method to retrieve the response body as a Blob.

```javascript
fetch('https://api.example.com/image')
  .then(response => response.blob()) // Get response as Blob
  .then(imageBlob => {
    console.log('Image Blob:', imageBlob);
    // You can use this Blob to display the image, for example
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

In all these cases, the then() method is used to chain onto the Promise returned by the fetch call. If the fetch operation encounters an error (such as a network issue or invalid response), the catch() block is executed.

Remember that the **async** and **await** keywords can also be used with fetch calls to handle asynchronous operations in a more synchronous and readable manner:

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json(); // Parse JSON response
    console.log('Data:', data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

fetchData();
```

## What is .then hell

The **".then hell"** refers to a situation where you have multiple chained .then() callbacks in asynchronous code, leading to nested and potentially hard-to-read code. This can happen when dealing with multiple asynchronous operations sequentially. To address this issue and make your code more readable, you can use async and await syntax. Here's how you can refactor the code to avoid the ".then hell":

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log('Data:', data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

fetchData();
```

In the example above, the async function fetchData uses the await keyword to pause execution until the asynchronous operation (such as the fetch call) completes. This way, you don't need to chain multiple .then() callbacks, making the code more linear and easier to read.

Here's a comparison between the traditional .then chaining and the async/await version:

**Using .then Chaining:**

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => {
    console.log('Data:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

**Using async/await:**

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log('Data:', data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

fetchData();
```

The **async/await** version avoids the nesting of callbacks and provides a more intuitive flow of control. This approach is especially helpful when you have multiple asynchronous operations that need to be executed sequentially.

Keep in mind that the **async/await** syntax is not always suitable, especially when you have to run asynchronous operations in parallel or handle complex control flows. In such cases, you might still use **.then** chaining or other techniques, like **Promise.all** for parallel execution. The choice depends on the specific requirements of your code.

