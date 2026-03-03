### Multiple Client Chat - Client, Server, and Protocol

**Overview**

Multiple Clients Chat Application:

* Built a chat application supporting multiple clients connecting to a central server.
* Designed and implemented `chat_client_skeleton.py` and `chat_server_skeleton.py` for client-server communication.
* Utilized a custom protocol defined in `protocol.py` to handle commands such as setting usernames, sending messages, and retrieving user lists, effectively encapsulating all data transmission logic.
* Ensured asynchronous handling of client sessions for efficient real-time communication.
* **Codebase strictly adheres to PEP8 standards**, ensuring high readability and maintainability.

#### Chat Protocol (`chat_protocol_skeleton.py`)

**Overview**
`chat_protocol_skeleton.py` acts as the core networking engine for the application. It defines essential constants, commands, and the primary data transmission functions integral to the client-server communication, enforcing a strict separation of concerns.

**Features and Functionality:**

* **Server Configuration:** Specifies `SERVER_IP`, `SERVER_PORT`, and `MAX_MSG_LENGTH` to standardize limits across the network.
* **Client-Server Commands:** Represents distinct commands (`COMMAND_NAME`, `COMMAND_GET_NAMES`, `COMMAND_MSG`, `COMMAND_EXIT`) facilitating user interactions.
* **Data Transmission Abstraction:** * **`send_message(socket, message)`:** Encodes and reliably transmits UTF-8 messages over the network.
* **`receive_message(socket)`:** Manages incoming streams, ensuring complete message reception and graceful handling of closed connections. Moving these functions to the protocol module centralizes the network logic.



#### Chat Client (`chat_client_skeleton.py`)

**Overview**
The `chat_client_skeleton.py` script provides the client-side interface, connecting to the server using TCP sockets. It facilitates bidirectional communication, prioritizing a clean user experience and robust error handling.

**Features and Functionality:**

* **Socket Initialization:** Initializes a TCP socket (`socket.AF_INET`, `socket.SOCK_STREAM`) utilizing the constants provided by the protocol module.
* **Command-Line Interface (CLI):**
* **`NAME <name>`:** Sets the username, validated for uniqueness.
* **`GET_NAMES>`:** Requests a list of all connected users.
* **`MSG <name> <message>`:** Sends a private message to a specific user.
* **`EXIT`:** Gracefully disconnects from the server.


* **Enhanced User Experience (UI/UX):**
* **Clean Input Prompts:** Resolved terminal display issues to ensure the input prompt does not duplicate before and after server responses, maintaining a clean console UI.
* **Input Validation:** Provides clear, descriptive feedback and error indications to the user when an invalid command or malformed message is entered, preventing silent failures.


* **Main Asynchronous Loop:**
* Uses `select.select()` to monitor socket input asynchronously.
* Detects real-time user keypresses using `msvcrt.kbhit()` (on Windows).
* **Safe Termination:** The `EXIT` command is securely handled to close the socket and terminate the loop without crashing the client application.



#### Chat Server (`chat_server_skeleton.py`)

**Overview**
`chat_server_skeleton.py` is a robust server implementation managing concurrent multi-client connections. It handles chat routing, username tracking, and connection stability.

**Features and Functionality:**

* **Asynchronous Client Handling:** Utilizes `select.select()` for efficient, non-blocking I/O management across multiple connected clients.
* **Command Routing:**
* **`name_handle`:** Manages username assignments and prevents duplicates.
* **`get_name_handle`:** Broadcasts the active user directory.
* **`msg_handle`:** Validates sender/recipient and routes private messages.
* **`exit_handle`:** Safely executes the disconnection protocol.


* **Resilient Client Management:**
* Tracks connections via `client_sockets` and usernames via a `clients_names` dictionary.
* **Abrupt Disconnection Handling:** The server is hardened against violent or unexpected client closures (e.g., forced terminal exits). It detects broken pipes, safely cleans up the disconnected socket from the active tracking lists, and prevents server crashes.



### Conclusion

Collectively, these modules constitute a robust framework for a multi-client chat architecture using Python sockets. Recent refactoring efforts have significantly improved the system by abstracting the send/receive logic into the protocol module, enforcing **PEP8 compliance**, and implementing resilient error handling for both malformed user inputs and abrupt network disconnections. The result is a stable, clean, and highly maintainable real-time communication platform.
