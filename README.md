#  Multithreaded TCP Based Live Auction Server (C++)

This repository contains a **multithreaded TCP-based Live Auction Server and Client** built entirely in **C++** using socket programming. Multiple clients can connect simultaneously to participate in a **real-time auction** over TCP.

---

##  Features

-  TCP socket communication
-  Supports up to 10 concurrent clients
-  Multithreaded server using `std::thread`
-  Thread safety using `std::mutex`
-  Real-time bidding system
-  Windows compatible (uses Winsock2)

---

##  Requirements

Make sure the following are installed:

| Requirement | Version |
|-------------|----------|
| g++ compiler | **10 or higher** |
| Git | Latest |
| Windows OS |  Supported |
| Visual Studio Code (optional) | Recommended |

>  Ensure that your **g++ version is 10 or higher**, as this project uses modern **C++17 features and multithreading** (`<thread>`, `<mutex>`).

---
Server Side
server/main.cpp
Entry point for the server application.
Sets up signal handling to gracefully stop the server on interruption (Ctrl+C).
Creates an AuctionServer object and starts the server.
server/AuctionServer.h
Defines AuctionServer class:
Holds auction item state and client socket list.
Uses mutexes for thread safety (clients_mutex and auction_mutex).
Functions: initialize/cleanup Winsock, setup listening socket, manage clients, broadcast messages, start/stop the server.
Defines AuctionItem class:
Represents the auctioned item (name, current bid, current highest bidder).
server/AuctionServer.cpp
Implements all server logic:
Initialization: Sets up Winsock, listens for incoming connections.
Client Management: Spawns a new thread for each client that connects (manageclient).
Auction Logic: Receives bids, updates highest bid, and broadcasts changes to all clients.
Shutdown/Cleanup: Closes sockets and cleans up resources.
Client Side
client/main.cpp
Entry point for the client application.
Creates an AuctionClient object and starts the client.
client/AuctionClient.h
Defines AuctionClient class:
Handles socket connection to the server, sending bids, receiving messages.
Functions: initialize/cleanup Winsock, connect to server, receive messages, send bid, start/stop the client.
client/AuctionClient.cpp
Implements all client logic:
Initialization: Sets up Winsock, connects to the server.
Bid Loop: Prompts the user to enter bids and sends them to the server.
Receiving Updates: Runs a thread to constantly listen for messages from the server.
Shutdown/Cleanup: Closes the connection and cleans up resources.
Key Technical Features
Multithreading: The server uses std::thread to handle multiple concurrent clients.
Thread Safety: Uses std::mutex to protect shared resources (like the list of clients and auction item state).
TCP Sockets: Uses Winsock2 for networking.
Real-Time Updates: Server broadcasts bid updates to all connected clients.
Graceful Shutdown: Both server and client clean up sockets and threads on exit.
How It Works (High-Level Flow)
Start the Server: Listens for connections on port 8000.
Start Clients: Each client connects to the server on localhost:8000.
Bidding: Clients enter bid amounts, which are sent to the server.
Auction Logic: Server receives bids, updates the highest bid, and broadcasts the new state to all clients.
Concurrency: Up to 10 clients can participate simultaneously.
Shutdown: Server and clients can be stopped gracefully.
---

##  Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/Pavankumar-Batchu1185/Multithreaded-TCP-Based-Live-Auction-Server-
cd Multithreaded-TCP-Based-Live-Auction-Server-
```

### 2. Build the Server
```bash
cd server
g++ -std=c++17 AuctionServer.cpp main.cpp -o AuctionServer.exe -lws2_32
```

### 3.Build the Client
```bash
cd ../client
g++ -std=c++17 AuctionClient.cpp main.cpp -o AuctionClient.exe -lws2_32
```

## Run the application
### 1.Start the server
```bash
cd server
./AuctionServer.exe
```

### 2.Start a Client

Open a new terminal:
```bash
cd client
./AuctionClient.exe
```
You can open up to 10 client windows to simulate multiple bidders.

## Project Structure
```css
.
├── server
│   ├── AuctionServer.cpp
│   ├── main.cpp
├── client
│   ├── AuctionClient.cpp
│   ├── main.cpp
└── README.md
```

