# HTTP Status Codes Cheat Sheet

This document provides a quick reference for common HTTP status codes used in APIs.

---

## ✅ Success Responses

### **200 OK**
The request has succeeded.  
Example: Fetching data via `GET /users`.

### **201 Created**
A new resource has been successfully created.  
Example: Creating a new user via `POST /users`.

### **202 Accepted**
The request has been accepted for processing, but the processing is not yet complete.  
Example: Queuing a background job.

### **204 No Content**
The request was successful, but there is no content to send in the response.  
Example: Successful `DELETE /users/123`.

---

## ⚠️ Client Error Responses

### **400 Bad Request**
The server cannot process the request due to invalid syntax or missing parameters.  
Example: Sending malformed JSON.

### **401 Unauthorized**
Authentication is required or has failed.  
Example: Missing or invalid access token.

### **403 Forbidden**
The client is authenticated but does not have permission to access the resource.  
Example: Authenticated user’s access token does not include required scopes.

### **404 Not Found**
The requested resource could not be found.  
Example: Accessing `/users/999` when the user does not exist.

### **405 Method Not Allowed**
The HTTP method is not supported for this resource.  
Example: Sending `GET` to an endpoint that only supports `POST`.

### **429 Too Many Requests**
The client has sent too many requests in a given time period (rate limit exceeded).  
Example: API throttling after exceeding request quota.

---

## ❌ Server Error Responses

### **500 Internal Server Error**
A generic server-side error occurred.  
Example: Unhandled exception in server code.

### **502 Bad Gateway**
The server, acting as a gateway or proxy, received an invalid response from an upstream server.  
Example: Reverse proxy receiving an invalid reply from a microservice.

### **503 Service Unavailable**
The server is temporarily unable to handle the request.  
Example: Server overload or maintenance.

### **504 Gateway Timeout**
The upstream server took too long to respond.  
Example: Database query timeout.

---

