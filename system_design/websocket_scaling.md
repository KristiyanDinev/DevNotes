# Scaling WebSockets Across Multiple Servers

## The Challenge

WebSocket connections are stateful and persistent, creating a unique challenge when scaling applications across multiple servers. Unlike HTTP requests that can be easily load-balanced, WebSocket connections are tied to specific server instances, making it impossible to directly access a connection from a different server.

## The Solution: Event-Driven Architecture

The key insight is that you cannot store WebSocket connections in a database or share them between servers. Instead, you need to track **which server holds which connection** and use an event-driven approach to trigger messages on the appropriate server.

### Core Concept

1. **Connection Tracking**: Each server maintains its own WebSocket connections in memory

2. **Server Registration**: Database records which server instance handles which client/user

3. **Event Broadcasting**: When a message needs to be sent, an event is published to trigger the correct server

4. **Local Delivery**: The target server receives the event and uses its local WebSocket connection to deliver the message

## Architecture Overview

>Can Client be connected to two servers at the same time?
>- **Yes**, but why? There are most likely two different **topics/events** the user is subscribing to and, since we have a **distributed system** and many servers, we need to keep the client updated on the real-time events which happen in the **database** or received by **third parties**. Now, usually, the client subscribes to only **one topic** and makes **one WebSocket connection**. That **is correct**, but what if that real-time event happens on **Server 2** and the client is connected to **Server 1**? Then **Server 1** needs to send a message to **Server 2** saying: *"Update Client X, Message: Object"*. That is why it is **distributed**.

```
[Client] -> Connects to Server 1

[Server 1] -> (first connection? If so save in database) -> [Database] (Server 1 responsible for Client) (Store connection in memory or anywhere accessible)

Real-time event trigger on backend!  *(User subscribes to see changes in his online order status or reservation made.)*

[Server 1] -> Update Client!

[Server 1] -> If Server 1 has access to Client websocket, then sent it.

[Server 1] -> Checks if Server 2 *(or any other in the database registered)* can deliver the message to Client. If so deliver it *(publish it to the event-system)*.

[Message Queue] ←→ [Server 2] ←→ [Client]
```

### Data Flow

**1.** On startup, the server generates an ID for itself **(GUID / UUID)**, which will be used for the message brokers/event-based system. I will use **Server 1** for example, but keep in mind that **1** is the server generated **ID**.

**2.** Client connects to a server 1

**3.** Server records in database *(if it is the first time)*: "User X is connected to Server 1", in other words: "Server 1 is responsible for User X"

**4.** When a message needs to be sent to User X:
   - **If Server 1 has access to User X websocket**: Direct delivery using local WebSocket connection
   - **Then check if there is any other server responsible for User X. And if there is ANY**: 
     - Server 1 publishes event: "Send message to User X, Server Id: 2, Message: Object"
     - Server 2 receives the event
     - Server 2 uses its local WebSocket connection to deliver the message

## Message Queue Options

### RabbitMQ (Recommended for Most Cases)

**Advantages:**
- Simpler to set up and manage
- Built-in message persistence
- Flexible routing patterns
- Good for moderate traffic volumes
- Mature ecosystem with extensive documentation

**Best for:**
- Applications with moderate message throughput
- Teams preferring simplicity over maximum performance
- Traditional message queue patterns

### Apache Kafka

**Advantages:**
- Extremely high throughput and low latency
- Built-in horizontal scaling
- Excellent for high-volume scenarios
- Strong durability guarantees
- Auto-scaling capabilities

**Considerations:**
- More complex setup and management
- Overkill for simple use cases
- Requires more operational expertise

**Best for:**
- High-throughput applications
- Real-time streaming scenarios
- Applications requiring guaranteed message ordering
- Systems that may need to scale rapidly

## Implementation Strategy

### 1. Connection Management

Each server instance maintains:
- In-memory map of active WebSocket connections
- Registration of its own server identity in the database
- Subscription to relevant message queue topics

### 2. Event Structure

Events should contain:
- Target user/client identifier
- Message payload
- Optional routing information
- Timestamp

### 3. Fault Tolerance

- Handle server disconnections gracefully
- Implement connection cleanup when servers go offline
- Use message queue acknowledgments to ensure delivery
- Consider message expiration for time-sensitive data

### 4. Database Schema

Track the following information:
- User/client identifier
- Server instance identifier
- Connection timestamp (debugging, tracking lifetime, etc.)
- Optional metadata (room, channel, etc.)

## Performance Considerations

- **Connection Limits**: Each server has a maximum number of concurrent connections *(or you can have unlimited connections, which is not preferable, if the server can't handle it)*
- **Message Latency**: Event-driven approach adds small latency overhead
- **Memory Usage**: WebSocket connections consume server memory
- **Network Overhead**: Inter-server communication via message queue

## Conclusion

The event-driven approach using message queues provides a robust, scalable solution for WebSocket distribution. Choose RabbitMQ for simplicity and moderate scale, or Kafka for high-throughput scenarios. The key is understanding that WebSocket connections cannot be shared directly between servers, but events can trigger the appropriate server to use its local connections for message delivery.