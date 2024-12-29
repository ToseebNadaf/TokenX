# TokenX

**TokenX** is a cutting-edge, real-time stock trading platform built to streamline the buying and selling of stocks. It supports both market and limit orders, delivering an efficient and responsive trading experience. TokenX leverages WebSockets for dynamic live updates, Redis for high-speed data processing, and TimescaleDB for managing time-series data. The platform is designed for scalability, robustness, and seamless user interaction.

## Key Features

- **Instant Order Placement**: Users can place market and limit orders for various stocks.
- **Order Management**: View, delete, or check the status of orders.
- **Live Market Data**: Real-time WebSocket streams for market depth, order books, and ticker data.
- **Market Quotes & Liquidity**: Ability to request market quotes and ensure liquidity through the Market Maker component.

## System Architecture

TokenX is organized into several core modules:

### API Module (Node.js)
- Manages frontend requests for placing orders, retrieving quotes, and other trading actions.
- Utilizes a Redis queue to streamline data processing and subscribes to updates for delivering real-time responses.

### Trading Engine
- Processes data from Redis queues to calculate and update market metrics like order book, kline data, and market depth.
- Publishes processed data to the WebSocket module for real-time updates to the frontend.
- Forwards structured data to a Redis queue for database storage.

### WebSocket Module
- Streams real-time data such as price movements, recent trades, and graphical trends to the frontend.
- Relies on Redis pub/sub mechanisms for rapid data dissemination.

### Database Module (TimescaleDB)
- Handles time-series data storage, updating records at various intervals (seconds, minutes, hours) for comprehensive data analysis.

### Market Maker (MM)
- Ensures liquidity by managing dynamic buy and sell orders to stabilize the market.

## Repository Overview

### Modules

- **API**: Incorporates WebSocket integration for delivering real-time updates to users.
- **Database (DB)**: Uses TimescaleDB for efficient, scalable time-series data storage.
- **Docker**: Provides a `docker-compose.yml` configuration for initializing TimescaleDB and Redis containers.
- **Trading Engine**: Processes core trading logic, including order book management and frontend data preparation.
- **Frontend**: Built with Next.js to provide an intuitive and responsive user interface.
- **Market Maker (MM)**: Implements algorithms to maintain liquidity and market balance.
- **WebSocket (WS)**: Manages live data streams for the frontend.

### Recent Updates

- **API**: Added WebSocket integration for real-time updates.
- **Database (DB)**: Integrated TimescaleDB for better data handling.
- **Docker**: Configured containers for Redis and TimescaleDB setup.
- **Trading Engine & Frontend**: Enhanced order book functionality with unit tests.
- **Market Maker (MM)**: Improved Market Maker for better liquidity.

## API Endpoints

### Orders

- **Place an Order**  
  - **Endpoint**: `POST /api/v1/order`
  - **Request Parameters**:
    - `type`: `limit` or `market`
    - `kind`: `buy` or `sell`
    - `price`: `number` (optional for limit orders) Numeric value
    - `quantity`: Numeric value
    - `market`: Example: `TATA-INR`
  - **Response**:
    - `orderId`: Unique identifier for the order
    - `fillStatus`: Order fulfillment status

- **Get Order Status**  
  - **Endpoint**: `GET /api/v1/order/:orderId`
  - **Description**: Fetches the current status of an existing order.

- **Delete an Order**  
  - **Endpoint**: `DELETE /api/v1/order/:orderId`
  - **Description**: Cancels an unfilled order.

- **Get a Quote**  
  - **Endpoint**: `POST /api/v1/order/quote`
  - **Request Parameters**:
    - `kind`: `buy` or `sell`
    - `quantity`: Numeric value
    - `market`: Example: `TATA-INR`
  - **Description**: Provides a market quote based on specified parameters.

## Real-Time WebSocket Streams

TokenX supports live updates through WebSocket streams, including:

- **Current Price**: Real-time price tracking.
- **Order Book**: Dynamic updates on buy/sell orders.
- **Recent Trades**: Displays trade history.
- **Graph Data**: Updates charts for price and order trends.

## Getting Started
### Setup Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/ToseebNadaf/TokenX.git
   cd TokenX
2. **Start Docker Containers** (for TimescaleDB and Redis)
   ```bash
   docker-compose up -d
3. **Run Individual Modules** Navigate to each module directory (e.g., api, engine, frontend) and run:
   ```bash
   npm run dev