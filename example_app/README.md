
# An example app with a client-server architecture

This project provides a real-time WebSocket-based game designed to simulate network latency and bandwidth conditions, inspired by applications such as Augmented Reality (AR). The application uses padding to artificially increase message sizes for testing purposes, simulating the behavior of heavier payloads in real-world scenarios.

## Features

- **WebSocket Communication**: Real-time communication between the client and server for gameplay and latency simulation.
- **Latency Measurement**: Measures round-trip latency using WebSocket `ping-pong` messages.
- **Payload Simulation**: Dynamically adjustable payload sizes to simulate varying levels of network load (e.g., Light, Medium, Heavy, or No Payload). Payload can also be configured via a URL parameter (`load=NoPadding/Light/Medium/Heavy`) or dynamically toggled using the UI.
- **Frame Updates**: The client updates the game state only when new data is received from the server.
- **Configurable Frame Rate**: The server frame rate and padding frequency are configurable in the code, with the current setup transmitting padded messages every 500ms to simulate state synchronization.
- **Responsive Gameplay**: A canvas-based game playable in a web browser, including paddle movement and a bouncing ball.
- **Auto Mode**: Toggleable feature to enable automated paddle movement based on the ball's position. Auto mode can also be enabled via a URL parameter (`auto=true`) or dynamically toggled using the UI.
- **Keyboard Controls**: Arrow keys allow manual paddle control alongside automated features.

## Simulation Details

1. **Dynamic Payload Sizes**:
    - **No Payload**: No additional payload is added to messages.
    - **Light**: ~50 KB added to client messages, ~500 KB added to server messages.
    - **Medium**: ~100 KB added to client messages, ~1 MB added to server messages.
    - **Heavy**: ~250 KB added to client messages, ~2.5 MB added to server messages.
2. **Padding Usage**:
    - Padding simulates the heavier data loads of AR applications.
    - The additional padding is dropped as soon as it is received and is not processed further.
3. **Frame Updates**:
    - The game frame updates only when the client receives new game state data from the server.
    - This approach ensures synchronized gameplay but may result in visible delays when large payloads are used.

## Setup and Usage

### Prerequisites

1. **Node.js**: Required for local development.
2. **Docker**: (Optional) For containerized deployment.

### Clone the Repository

```bash
git clone https://github.com/dimitrisgiannopoulos/latency-bandwidth-simulator.git
cd example_app
```

### Running Locally

#### Using Node.js

1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the server:
   ```bash
   node server.js
   ```
3. Open your browser at `http://localhost:80`.

#### Using Docker

1. Build the Docker image:
   ```bash
   docker build -t dimitrisgian/example-app .
   ```
2. Run the Docker container:
   ```bash
   docker run -it --rm -p 80:80 dimitrisgian/example-app
   ```
3. Open your browser at `http://localhost:80`.

### Configurable Options

1. **Payload Slider Settings**:
    - Adjust the payload slider in the client to simulate different payload sizes.
    - This dynamically updates the payload size during gameplay.
2. **Server Configuration**:
    - **Frame Rate**: Configurable in the server code (`updateInterval`, currently set to 16ms or ~60 FPS).
    - **State Sync**: The server sends padded messages every 500ms to simulate state synchronization.
3. **Client Padding**:
    - Padded messages are sent to the server each time a move action is triggered (e.g., paddle movement).
4. **Auto Mode**:
    - Enable auto mode using the toggle in the UI or by adding `?auto=true` to the URL.
    - Auto mode dynamically adjusts paddle movement to align with the ball's position.

## Measuring Latency

1. The client sends a `ping` message with a timestamp.
2. The server responds with a `pong` message.
3. The client calculates latency as the time difference between sending and receiving the messages.

## Known Issues

- High payload sizes (e.g., Heavy) may cause visible delays due to network congestion or browser rendering limitations.

## License

This project is open-source and available under the [Apache 2.0 License](LICENSE).