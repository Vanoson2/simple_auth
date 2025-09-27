# Simple Authentication

This project demonstrates two different authentication methods using Express.js:
1. **Basic HTTP Authentication** (`basic_auth.js`)
2. **Cookie-based Authentication** (`cookie_auth.js`)

## Setup

1. Install dependencies:
```bash
npm install
```

2. Make sure MongoDB is running (required for cookie authentication):
```bash
mongod
```

3. Start the desired server:

### For Basic Authentication:
```bash
node basic_auth.js
```
Server runs on `http://localhost:3000`

### For Cookie Authentication:
```bash
node cookie_auth.js
```
Server runs on `http://localhost:3001`

## Testing with Postman

## 1. Basic Authentication (basic_auth.js - Port 3000)

### Test Public Routes (No Authentication Required)

![alt text](image.png)
#### Get Public Page
- **Method**: GET
- **URL**: `http://localhost:3000/public`
- **Expected Response**: "Welcome! Visit second public resource."

### Test Protected Route (Basic Authentication Required)

#### Access Secure Route Without Authentication
- **Method**: GET
- **URL**: `http://localhost:3000/secure`
- **Expected Response**: 
  - Status Code: 401 Unauthorized
  - Body: "Authentication required."

#### Access Secure Route With Correct Credentials
- **Method**: GET
- **URL**: `http://localhost:3000/secure`
- **Authentication**: 
  - Type: Basic Auth
  - Username: `admin`
  - Password: `12345`
- **Steps in Postman**:
  1. Go to the "Authorization" tab
  2. Select "Basic Auth" from the Type dropdown
  3. Enter Username: `admin`
  4. Enter Password: `12345`
- **Expected Response**:
  - Status Code: 200 OK
  - Body: "You have accessed a protected resource 🎉"

#### Access Secure Route With Wrong Credentials
- **Method**: GET
- **URL**: `http://localhost:3000/secure`
- **Authentication**: 
  - Type: Basic Auth
  - Username: `wrong`
  - Password: `credentials`
- **Expected Response**:
  - Status Code: 403 Forbidden
  - Body: "Access denied."

## 2. Cookie Authentication (cookie_auth.js - Port 3001)

### Login (Create Cookie Session)
- **Method**: POST
- **URL**: `http://localhost:3001/login`
- **Headers**: 
  - Content-Type: `application/json`
- **Body** (JSON):
```json
{
  "username": "admin",
  "password": "12345"
}
```
- **Expected Response**: 
  - Status Code: 200 OK
  - Body: "Logged in!"
  - Cookie: `auth_cookie_token` will be set

### Access Protected Profile (With Valid Cookie)
- **Method**: GET
- **URL**: `http://localhost:3001/profile`
- **Prerequisites**: Must login first to get the cookie
- **Expected Response**:
  - Status Code: 200 OK
  - Body: "Welcome user 1, your cookie is valid."

### Access Protected Profile (Without Cookie)
- **Method**: GET
- **URL**: `http://localhost:3001/profile`
- **Prerequisites**: Clear cookies or use new request without login
- **Expected Response**:
  - Status Code: 401 Unauthorized
  - Body: "No cookie found"

### Logout (Clear Cookie Session)
- **Method**: POST
- **URL**: `http://localhost:3001/logout`
- **Expected Response**: 
  - Status Code: 200 OK
  - Body: "Logged out."
  - Cookie: `auth_cookie_token` will be cleared

### Testing Invalid Credentials
- **Method**: POST
- **URL**: `http://localhost:3001/login`
- **Body** (JSON):
```json
{
  "username": "wrong",
  "password": "password"
}
```
- **Expected Response**:
  - Status Code: 401 Unauthorized
  - Body: "Invalid credentials"

## Manual Testing with cURL

### Basic Authentication (Port 3000)
```bash
# Test public routes
curl http://localhost:3000/
curl http://localhost:3000/public

# Test secure route without auth (should fail)
curl http://localhost:3000/secure

# Test secure route with correct credentials
curl -u admin:12345 http://localhost:3000/secure

# Test secure route with wrong credentials
curl -u wrong:password http://localhost:3000/secure
```

### Cookie Authentication (Port 3001)
```bash
# Login and save cookies
curl -c cookies.txt -X POST -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"12345"}' \
  http://localhost:3001/login

# Access protected route with cookies
curl -b cookies.txt http://localhost:3001/profile

# Logout
curl -b cookies.txt -c cookies.txt -X POST http://localhost:3001/logout

# Try to access protected route after logout (should fail)
curl -b cookies.txt http://localhost:3001/profile
```

## Understanding Authentication Methods

### Basic Authentication
Basic Authentication works by:
1. Client sends credentials encoded in Base64 in the `Authorization` header
2. Format: `Authorization: Basic <base64-encoded-username:password>`
3. Server decodes and validates the credentials
4. If valid, access is granted; otherwise, 401/403 is returned

### Cookie Authentication
Cookie Authentication works by:
1. User logs in with username/password via POST request
2. Server validates credentials and creates a unique session token
3. Session token is stored in MongoDB with user details and expiration
4. Server sends the token back as an HTTP-only cookie
5. For subsequent requests, browser automatically sends the cookie
6. Server validates the cookie token against the database
7. If valid and not expired, access is granted

## Database Requirements

The cookie authentication uses MongoDB to store session data:
- **Database**: `cookieApp`
- **Collection**: `cookies`
- **Document Structure**:
  ```json
  {
    "cookie_token": "unique-uuid",
    "userId": "1",
    "userRole": "adsys",
    "createdAt": "2025-09-27T...",
    "expires": "2025-09-27T..." // 5 minutes from creation
  }
  ```

## Security Notes

⚠️ **Warning**: These are demo implementations. In production:
- Use HTTPS to encrypt credentials and cookies in transit
- Store passwords securely (hashed with salt using bcrypt)
- Consider using more secure authentication methods (JWT, OAuth, etc.)
- Implement rate limiting to prevent brute force attacks
- Use secure cookie settings (Secure, SameSite)
- Implement proper session management and cleanup
- Use environment variables for sensitive configuration