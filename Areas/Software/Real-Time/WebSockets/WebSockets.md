---
tags:
  - areas
  - software
  - real-time
  - websockets
author: Santi Tabbach
created: 2026-05-30
modified: 2026-06-12
---
## Index

1. [[#WebSockets]]
2. [[#Handshake]]
3. [[#Default URI Format]]
4. [[#WebSocket Frames]]
5. [[#Fragments]]
6. [[#Implementation in JS]]
7. [[#Pros and Cons]]
8. [[#Do you have to use WebSockets?]]

## WebSockets

WebSockets are a communication protocol that enables bidirectional, real-time communication between a client and a server over a single TCP connection.

Web Socket allow us to create "real-time" applications which are faster and require less overhead than traditional HTTP protocol.

WebSocket uses a `duplex` protocol for communication between client and server.

#### Some Key Points
- WebSockets are bidirectional in nature.
- Connection developed using the websocket lasts as long as any of the participant (client or server) closes the connection.
- WebSocket makes use of HTTP to initiate connection.

![[Pasted image 20260612010430.png]]

---
## Handshake

WebSocket handshaking is a process that occurs when a client and server establish a WebSocket connection. Unlike traditional HTTP, which follows a request-response model, WebSocket handshaking enables bidirectional communication, allowing both the client and server to send and receive messages asynchronously.

### The WebSocket handshake process

The WebSocket handshake process involves several steps that enable the establishment of a persistent and efficient communication channel:

1. **HTTP Upgrade Request:** The client initiates the handshake by sending an HTTP upgrade request to the server, indicating its intention to switch to the WebSocket protocol.
2. **Server Response:** Upon receiving the upgrade request, the server responds with an HTTP 101 status code, indicating a successful upgrade to the WebSocket protocol.
3. **Headers and Key Generation:** During the handshake, both the client and server exchange headers containing information required for establishing the WebSocket connection. The client generates a random key, which is combined with a predefined WebSocket GUID and then base64-encoded.
4. **Verification and Acceptance**: The server verifies the received key, constructs a response key by concatenating the client key with the GUID, and then base64-encodes it. If the server accepts the client's request, it includes the response key in the response headers.
5. **Connection Establishment:** Once the client receives the server's response, it verifies the response key to ensure the connection is established with the correct server. If the verification is successful, the WebSocket connection is considered open and ready for bidirectional communication.

![[Pasted image 20260531112218.png]]

![[Pasted image 20260531112228.png]]

### Advanced WebSocket Handshaking Concepts

In addition to the basic implementation, there are several advanced concepts related to WebSocket handshaking that are worth exploring, such as authentication and authorization, securing WebSocket connection using encryption, and scaling and load balancing for high-traffic applications.

1. **Authentication and Authorization:** To restrict access to WebSocket connections, you can implement authentication and authorization mechanisms. This could involve validating user credentials during the handshake process or verifying access rights for specific WebSocket endpoints.
2. **Securing WebSocket Connections:** To enhance security, you can enable encryption for WebSocket connections using protocols like Transport Layer Security (TLS) or Secure Sockets Layer (SSL). This ensures that the data exchanged between the client and server remains confidential and secure.
3. **Scaling and Load Balancing:** For applications that require handling a large number of WebSocket connections, scaling and load balancing techniques can be employed. This involves distributing the incoming WebSocket connections across multiple servers or instances to handle the increased load efficiency.

---
## Default URI Format

WebSocket has a default URI Format:

```bash
ws-URI = "ws:" "//" host [ ":" port ] path [ "?" query ]  
wss-URI = "wss:" "//" host [ ":" port ] path [ "?" query ]
```

Port component is optional as default port 80 is used for `ws` and 443 is used for `wss`.

wss is used as a secure URI as secure flag is set and TLS handshake is done between server and client for secure communication.

---
## WebSocket Frames

Frames in WebSocket are the basic units of data that are exchanged between the client and server. In web-socket protocol, data is transmitted using a sequence of frames. Client must mask the frames before sending it to the server and if an unmasked frame is received by the server, the server should close the connection.

> *In this case, a server MAY send a Close frame with a status code of 1002 (protocol error).*

![[Pasted image 20260612010802.png]]

- Fin (1 bit): Indicates if this is the final fragment in a message or not. Value 1 represent `Yes`. First fragment can also be the final fragment.
- `RSV1`, `RSV2`, `RSV3` (1 bit each): This must be a zero value. If this is a non-zero value, it has to define an extension that define the meaning of non-zero value.
- `opcode` (4 bits): It define the payload data, it must have one of the following value:
  ```
	%x0 denotes a continuation frame  
	  
	* %x1 denotes a text frame  
	  
	* %x2 denotes a binary frame  
	  
	* %x3-7 are reserved for further non-control frames  
	  
	* %x8 denotes a connection close  
	  
	* %x9 denotes a ping  
	  
	* %xA denotes a pong  
	  
	* %xB-F are reserved for further control frames
  ```
- `mask` (1 bit): It defines if marking is used or not, if set to 1, it must have a masked key defined as well, which will be used for unmasking the payload data. Every client should send the mark value to 1 and masking-key as well.
- `masking-key` (0 or 4 bytes): Every frame sent from the client is masked using this key. Masked keys are 32 bit values.
- `payload length` (7 bits, 64 bits): The length of the payload data in bytes: if 0-125, that is the payload length. If 126, the following 2 bytes interpreted as a 16-bit unsigned integer are the payload length. If 127, the following 9 bytes interpreted as a 64-bit unsigned integer (the most significan bit MUST be 0) are the payload length.
- `payload data`: The payload data is defined as "Extension data" concatenated with "Application data".

> *The smallest valid **full** WebSocket message is two byte i.e- close message sent by the server with no message.*
> 
> *The longest possible header is 14 bytes for a client-to-server message with a payload larger than 16KB: 8+1 bytes for the length and 4 bytes for the mask (plus the first fin/type byte).*

---
## Fragments

When sending a message in WebSocket, it is common for the message to be split up into multiple frames, especially if the message is large or complex.
**This is where fragments come in**. Fragments allow a message to be split up into smaller pieces, each of which is sent as a separate frame.

When a message is split up into fragments, each fragment is sent with the FIN bit set to 0 for all but the final frame in the sequence. The FIN bit indicates whether the current frame is the final frame in the message or whether more frames will follow.

If the FIN bit is set to 0, it means that there are more frames coming and the receiver should continue to wait for additional frames before processing the message.

When the final fragment of a message is sent, it is sent with the FIN bit set to 1. This signals to the receiver that this is the last frame in the message.

**Conceptually, WebSocket is really just a layer on top of TCP that does the following:**

- Adds a web origin-based security model for browsers.
- Adds an addressing and protocol naming mechanism to support multiple services on one port and multiple host names on one IP address.
- Layers a framing mechanism on top of TCP to get back to the IP packet mechanism that TCP is built on, but without length limits.
- Includes an additional closing handshake in-band that is designed to work in the presence of proxies and other intermediaries.

---
## Implementation in JS

```javascript
// ------------------- SERVER -------------------

const http = require("http");
const WebSocketServer = require('websocket').server
let connection = null;

const httpServer = http.createServer((req, res) => {
  console.log("we have received a request");
})

const webSocket = new WebSocketServer({
  "httpServer": httpServer,
})

webSocket.on("request", request => {
  
  
  connection = request.accept(null, request.origin) // accepting nothing, just an example
  connection.on("open", () => console.log("Opened!!"))
  connection.on("close", () => console.log("Closed!!"))
  connection.on("message", message => {
    console.log(`Received message ${message.utf8Data}`)
  })
  
  // send test message
  connection.send("Hello buddy, I'm the Server")
  
  // send test message - live connection
  sendEveryFiveSeconds();
  
  
})

httpServer.listen(8080, () => console.log("Server listening on port 8080"));

function sendEveryFiveSeconds(){
  connection.send(`Message ${Math.random()}`);
  
  setTimeout(sendEveryFiveSeconds, 5000)
  
}

// ------------------- CLIENT -------------------

let ws = new WebSocket("ws://localhost:8080")

ws.onmessage = message => console.log(`We received a message from the server: ${message.data}`);

ws.send("Hello Server, it's the Client here")
```

---
## Pros and Cons

#### Pros
- full-duplex (no polling) -> el cliente no tiene que estar preguntando todo el tiempo
- http compatible -> gracias al upgrade que hace switching protocols. Sin esto los proxies fallarian, el firewall diria WTF, etc...
- firewall friendly -> lo mismo que antes, es http standar. Firewalls won’t block it doesn’t use a special port

#### Cons
- Proxying is tricky, lots of proxies and transparent proxies don’t support it yet
- Layer 7 load balancing is tricky, timeouts on the load balancer.
- stateful, difficult to horizontally scale -> es literalmente una variable connection en el server, si reinicio el sv, todo muere y se muere la conexion, hay que reestablecerla.
- More complicated to implement (simple telnet use HTTP)
- Not ideal for all use cases - (microservices)

---
## Do you have to use WebSockets?

- NO! rule of thumb - do you absolutely need bidirectional communication?
- Long polling
- Event source 