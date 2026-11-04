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

**login POST request**

POST /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded

username=aparna&password=example123


**Example of an HTTP response**

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1200

<html>
<body>
Welcome!
</body>
</html>

**POINTS**

HTTP/1.0 → New TCP connection for every request.
HTTP/1.1 → Reuses TCP connections.
HTTP/2 → Multiple requests simultaneously using multiplexing.
HTTP/3 → Uses QUIC instead of TCP to avoid transport-level head-of-line blocking.

**HTTP/1.0**
Imagine you open a website containing:
- 1 HTML page
- 3 images
- 2 CSS files
Your browser needs 6 resources.With HTTP/1.0, it typically establishes 6 separate T
CP connections.

Every new TCP connection requires:
1. TCP handshake.
2. Connection establishment.
3. Request and response.
4. Connection closure.

Takes additional time and network resources.
More connection establishment overhead.
More resources consumed by repeated connections.

**HTTP/1.1 – Persistent connections**
multiple HTTP requests can reuse the same TCP connection instead of creating a new connection for every request.

Instead of creating 6 TCP connections, the browser can reuse one TCP connection.
This feature is called **Keep-Alive.**
**A. Chunked Transfer Encoding**
Imagine a server needs to send a very large file, but it doesn't know the complete response size in advance.
Instead of waiting to calculate the entire content length, it can send the response in smaller chunks.

Incorrect handling of Transfer-Encoding and Content-Length can contribute to HTTP Request Smuggling vulnerabilities.

B. Caching
Caching means storing a copy of a resource so that it can be reused without downloading it again every time.
Two important headers:
Cache-Control: max-age=3600
ETag: "abc123"

**Cache-Control:** Defines how long a resource can be cached.
**ETag:** Identifies a particular version of a resource

**C. Range Requests**
Suppose you are downloading a 100 MB file, but your internet disconnects after 80 MB.[Range: bytes=80000000-]
Without range requests, you may need to download the file again. 
With range requests, you can request only the remaining portion.

**What is Head-of-Line (HOL) Blocking in HTTP/1.1?**
HTTP/1.1 supports request pipelining, but responses must be returned in request order on a connection.

Imagine:
1. Request 1 takes 10 seconds.
2. Request 2 takes 1 second.
3. Request 3 takes 1 second.
Even if requests 2 and 3 are ready, their responses must wait behind response 1

**3. HTTP/2 – Multiplexing**

Multiplexing allows multiple request and response streams to be active simultaneously over a single TCP connection.
HTTP/2 divides HTTP messages into smaller units called frames.These frames belong to different streams.

1. Binary framing
HTTP/1.1 uses textual message formatting, whereas HTTP/2 uses binary frames.

2. HPACK compression
HTTP/2 compresses HTTP headers using HPACK to reduce repeated header data.

But HTTP/2 still has a problem!
It still uses TCP.
What happens if one TCP packet gets lost?
TCP ensures that data arrives reliably and in order.If a packet is lost, TCP may need to retransmit it. Until the missing data is recovered, later data cannot be delivered to HTTP/2 streams through that TCP connection.This can temporarily affect all streams sharing that connection.This is called transport-layer Head-of-Line Blocking.

**4. HTTP/3 – QUIC**

HTTP/3 addresses the TCP-level head-of-line blocking issue by replacing TCP with QUIC.
QUIC stands for Quick UDP Internet Connections.
It runs over UDP instead of TCP.
Why UDP?
TCP provides ordered, reliable delivery at the transport layer.

QUIC provides reliable delivery and congestion control too, but manages independent streams separately.

For example, imagine there are three streams:
- Stream 1: HTML
- Stream 2: CSS
- Stream 3: Image
Suppose a packet belonging to Stream 1 is lost.

With HTTP/3:
- Stream 1 waits for retransmission of its missing data.
- Stream 2 can continue receiving its data.
- Stream 3 can continue receiving its data.
One stream's packet loss does not block delivery of data on the other independent streams.

