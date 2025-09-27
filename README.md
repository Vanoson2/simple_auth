# Simple Authentication - Basic Auth

This project demonstrates basic HTTP authentication using Express.js.

## Setup

1. Install dependencies:
```bash
npm install
```

2. Start the server:
```bash
node basic_auth.js
```

The server will run on `http://localhost:3000`

## Testing with Postman

### 1. Test Public Routes (No Authentication Required)

#### Get Home Page
- **Method**: GET
- **URL**: `http://localhost:3000/`
- **Expected Response**: "Welcome! Visit first public resource."

#### Get Public Page
- **Method**: GET
- **URL**: `http://localhost:3000/public`
- **Expected Response**: "Welcome! Visit second public resource."

### 2. Test Protected Route (Basic Authentication Required)

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

## Manual Testing with cURL

You can also test using cURL commands:

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

## Understanding Basic Authentication

Basic Authentication works by:
1. Client sends credentials encoded in Base64 in the `Authorization` header
2. Format: `Authorization: Basic <base64-encoded-username:password>`
3. Server decodes and validates the credentials
4. If valid, access is granted; otherwise, 401/403 is returned

## Security Notes

⚠️ **Warning**: This is a demo implementation. In production:
- Use HTTPS to encrypt credentials in transit
- Store passwords securely (hashed with salt)
- Consider using more secure authentication methods (JWT, OAuth, etc.)
- Implement rate limiting to prevent brute force attacks