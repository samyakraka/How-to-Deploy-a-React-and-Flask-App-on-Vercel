![DALL·E 2025-01-28 00 58 01 - A visually engaging digital illustration of a developer's workspace featuring two computer monitors  One monitor displays a React app with a colorful,](https://github.com/user-attachments/assets/8b7987b3-1354-410b-82b0-7eb970a58524)

# React + Flask App on Vercel

This repository demonstrates how to deploy a **React** frontend and a **Flask** backend on **Vercel**, allowing you to build and deploy a full-stack web application seamlessly.

## Table of Contents

1. [Project Structure](#project-structure)
2. [Frontend Setup (React)](#frontend-setup-react)
3. [Backend Setup (Flask)](#backend-setup-flask)
4. [Deploying on Vercel](#deploying-on-vercel)
5. [Testing the App](#testing-the-app)
6. [Handling CORS Issues](#handling-cors-issues)
7. [Conclusion](#conclusion)

## Project Structure

```plaintext
react-flask-vercel/
│
├── flask-backend/
│   ├── app.py
│   └── vercel.json
│
├── react-frontend/
│   ├── public/
│   ├── src/
│   │   ├── App.js
│   ├── package.json
│   └── vercel.json
└── README.md
```

## Frontend Setup (React)

### 1. **Create a React App**
   You can create a new React app using the `create-react-app` command:
   ```bash
   npx create-react-app react-flask-vercel
   ```

### 2. **Modify `App.js`**
   In the `src/App.js` file, replace the default content with the following code:
   ```javascript
   import React, { useState } from "react";

   function App() {
     const [message, setMessage] = useState("");

     const fetchMessage = async () => {
       try {
         const response = await fetch("/api/hello");
         const data = await response.json();
         setMessage(data.message);
       } catch (error) {
         console.error("Error fetching message:", error);
       }
     };

     return (
       <div style={{ textAlign: "center", marginTop: "50px" }}>
         <h1>React + Flask App</h1>
         <button onClick={fetchMessage}>Get Message</button>
         {message && <p>{message}</p>}
       </div>
     );
   }

   export default App;
   ```

### 3. **Set Up Vercel Rewrite Rules**
   In the `react-frontend/` folder, create a `vercel.json` file to define the API route rewrites:
   ```json
   {
     "rewrites": [
       {
         "source": "/api/(.*)",
         "destination": "https://flask-backend-url.vercel.app/api/hello"
       }
     ]
   }
   ```

   **Note**: Replace `flask-backend-url.vercel.app` with the actual URL of your deployed Flask backend.

## Backend Setup (Flask)

### 1. **Create Flask App**
   Inside the `flask-backend/` folder, create a file called `app.py` with the following content:
   ```python
   from flask import Flask, jsonify
   from flask_cors import CORS

   app = Flask(__name__)
   CORS(app)  # Allow all origins (use this for development only)

   @app.route('/api/hello', methods=['GET'])
   def hello():
       return jsonify(message="Hello from Flask backend!")

   if __name__ == "__main__":
       app.run(debug=True)
   ```

### 2. **Set Up Vercel Configuration for Flask**
   Create a `vercel.json` file inside the `flask-backend/` directory:
   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "app.py",
         "use": "@vercel/python"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "/app.py"
       }
     ]
   }
   ```

## Deploying on Vercel

### 1. **Deploy the React App**
   - Go to the [Vercel Dashboard](https://vercel.com/dashboard) and click "New Project".
   - Select your React app repository (or upload it if it’s not already linked).
   - Vercel will automatically detect that it's a React app and deploy it.

### 2. **Deploy the Flask Backend**
   - Go to the Vercel Dashboard again and click "New Project".
   - Select your Flask backend repository and deploy it.

### 3. **Update API URL**
   Once your Flask backend is deployed, update the `vercel.json` file in the React app to use the correct backend URL.

## Testing the App

Once both your frontend and backend are deployed on Vercel, open your React app URL. You should see a button that says "Get Message". When you click it, it will fetch a response from your Flask backend and display the message: **"Hello from Flask backend!"**.

## Handling CORS Issues

To avoid CORS issues during development, the `flask_cors` package is used in the backend:

```python
from flask_cors import CORS
CORS(app)  # Allow all origins
```

For production, restrict CORS to only the React app’s URL:
```python
CORS(app, origins=["https://your-react-app-url.vercel.app"])
```

## Conclusion

By following this guide, you’ve learned how to deploy a **React** frontend and a **Flask** backend to **Vercel** with minimal configuration. This setup allows you to deploy a full-stack web application with ease. Now, your app is live and can be accessed globally, all with zero hassle!

Happy coding and enjoy deploying on Vercel! 🎉

