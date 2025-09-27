## Testing with Postman

### 1. Test Public Routes (No Authentication Required)

#### Get Home Page
![alt text](img/image.png)

#### Get Public Page
![alt text](img/image-1.png)

### 2. Test Protected Route (Basic Authentication Required)

#### Access Secure Route Without Authentication
![alt text](img/image-2.png)

#### Access Secure Route With Correct Credentials
![alt text](img/image-3.png)

#### Access Secure Route With Wrong Credentials
![alt text](img/image-4.png)

## 2. Cookie Authentication (cookie_auth.js - Port 3001)

### Login (Create Cookie Session)
![alt text](img/image-5.png)
![alt text](img/image-6.png)
![alt text](img/image-7.png)

### Access Protected Profile (With Valid Cookie)
![alt text](img/image-8.png)

### Logout (Clear Cookie Session)
![alt text](img/image-9.png)
![alt text](img/image-10.png)
![alt text](img/image-12.png)

### Access Protected Profile (Without Cookie)
![alt text](img/image-11.png)

### Testing Invalid Credentials
![alt text](img/image-13.png)

