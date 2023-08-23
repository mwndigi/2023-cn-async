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

Remember that due to security restrictions, your HTML page and Node.js server should be on the same domain or handle CORS (Cross-Origin Resource Sharing) properly to avoid issues with cross-origin requests.

This example demonstrates a simple way to make asynchronous HTTP calls from an HTML page to a Node.js server. Depending on your requirements, you might need to handle more complex scenarios such as different HTTP methods (e.g., POST), sending data in the request body, handling authentication, and so on.
