# Yatech.studio-challenge-api
## Project Setup
### 1. Create a new directory for your project and navigate to it in your terminal:
```ruby
mkdir nodejs-auth-api
cd nodejs-auth-api

```
### 2. Initialize a new Node.js project:
```ruby
npm init -y
```
### 3. Install the necessary packages:
```ruby
npm install express jsonwebtoken body-parser
```
## Backend Implementation
Now, let's create the Node.js backend for your authentication API.

Create a file named ***app.js*** or ***server.js*** and add the following code:
```ruby
const express = require("express");
const bodyParser = require("body-parser");
const jwt = require("jsonwebtoken");

const app = express();
const port = process.env.PORT || 3000;

// Secret key for JWT (should be stored securely)
const SECRET_KEY = "your_secret_key";

// Sample user data (replace with your user database)
const users = {
  user1: { password: "password1" },
  user2: { password: "password2" },
};

// Token expiration times
const ACCESS_TOKEN_EXPIRATION = "15m";
const REFRESH_TOKEN_EXPIRATION = "30d";

app.use(bodyParser.json());

// Helper function to generate access tokens
function generateAccessToken(username) {
  return jwt.sign({ username }, SECRET_KEY, { expiresIn: ACCESS_TOKEN_EXPIRATION });
}

// Helper function to generate refresh tokens
function generateRefreshToken(username) {
  return jwt.sign({ username }, SECRET_KEY, { expiresIn: REFRESH_TOKEN_EXPIRATION });
}

// Middleware to protect routes that require authentication
function authenticateToken(req, res, next) {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ message: "Token is missing" });
  }

  jwt.verify(token, SECRET_KEY, (err, user) => {
    if (err) {
      return res.status(403).json({ message: "Token has expired" });
    }

    req.user = user;
    next();
  });
}

// Authentication endpoint
app.post("/login", (req, res) => {
  const { username, password } = req.body;

  if (!username || !password) {
    return res.status(400).json({ message: "Username and password are required" });
  }

  const user = users[username];

  if (!user || user.password !== password) {
    return res.status(401).json({ message: "Invalid credentials" });
  }

  const accessToken = generateAccessToken(username);
  const refreshToken = generateRefreshToken(username);

  res.json({ accessToken, refreshToken });
});

// Protected resource
app.get("/protected", authenticateToken, (req, res) => {
  res.json({ message: `Hello, ${req.user.username}!` });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
This code sets up an Express server with two endpoints: ***/login*** for user authentication and ***/protected*** as a protected resource.
## API Documentation
Here's a basic API documentation template for your ***Node.js*** authentication API:

User Authentication API
## 1. Login
- **URL:** /login
- **Method:** POST
- **Request Body:**
  - **username** _(string)_ - User's username
  - **password** _(string)_ - User's password
-** Response:**
  - **accessToken** _(string)_ - JWT access token _(valid for 15 minutes)_
  - **refreshToken** _(string)_ - JWT refresh token _(valid for 30 days)_
- Authentication: None
## 2. Protected Resource
- **URL:** /protected
- **Method:** GET
- **Response:**
  - **message** _(string)_ - A greeting message for the authenticated user
- **Authentication:** Bearer token with a valid access token
## 3. Error Handling
- 400 Bad Request: Username and password are required.
- 401 Unauthorized: Invalid credentials or missing token.
- 403 Forbidden: Token has expired.
