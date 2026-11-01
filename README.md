The client-server model is a communication system in which one device requests a service and another device provides that service.
- **Client**: The device or application requesting information.
-** Server**: The system that receives the request, processes it and sends a response.

  **HTTP** defines how a client sends a request and how a server returns a response.
  **Example of an HTTP request**

GET /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html

Request breakdown

**Request line**
GET /login HTTP/1.1
Method + Resource path + HTTP version

**Headers**
Host: example.com

User-Agent: Mozilla/5.0
This tells the server information about the client making the request.

Accept: text/html
Additional information about the request



**Request body**
Not present in this example.
