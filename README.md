# VolatileWatch: Decentralized Price Deviation Notifier

A real-time, decentralized system for monitoring price volatility using on-chain oracles and custom WebSockets, delivering user-configurable alerts via serverless functions.

VolatileWatch is designed to provide a robust and reliable solution for tracking price deviations in decentralized finance (DeFi) and other cryptocurrency markets. Unlike centralized alerting services, VolatileWatch leverages on-chain oracles like Chainlink, Pyth, or custom deployed oracle solutions to fetch price data, ensuring transparency and resistance to manipulation. This system monitors price feeds and calculates deviations against user-defined thresholds. When a threshold is breached, the system triggers a serverless function to deliver a notification via email, SMS, or other integrated channels. The architecture prioritizes scalability and low latency, making it suitable for monitoring a large number of assets and delivering timely alerts.

The core innovation of VolatileWatch lies in its custom-built WebSocket infrastructure. Instead of relying on existing centralized WebSocket providers, VolatileWatch implements its own optimized WebSocket servers, enabling direct communication with the on-chain data sources and minimizing latency. This approach allows for fine-grained control over data delivery and improved reliability compared to using third-party services. The system is designed to be easily configurable, allowing users to define custom alert thresholds, notification channels, and the specific on-chain oracles to monitor.

By combining decentralized data sources with a custom WebSocket infrastructure and serverless functions, VolatileWatch offers a powerful and flexible solution for monitoring price volatility and reacting to market changes in real-time. This project empowers users with granular control over their alerts, enhanced reliability through decentralized data, and reduced latency through optimized communication channels. This provides a competitive advantage in fast-moving markets.

## Key Features

*   **Decentralized Data Sources:** Utilizes on-chain oracles (Chainlink, Pyth, custom deployed) to fetch price data, ensuring data integrity and immutability. Data is read directly from the blockchain using providers like ethers.js or web3.js.
*   **Custom WebSocket Infrastructure:** Implements optimized WebSocket servers for low-latency price feed delivery, bypassing centralized providers. This is accomplished through the 'ws' library in Node.js and scaled with message queuing.
*   **User-Defined Alert Thresholds:** Allows users to configure custom price deviation thresholds, triggering alerts based on percentage changes or absolute price differences. The thresholds are configured through a JSON file and applied to the data stream.
*   **Serverless Function Integration:** Triggers serverless functions (e.g., AWS Lambda, Google Cloud Functions) to deliver notifications via email, SMS, or other integrated channels. These functions are triggered via an event bus (e.g., AWS SNS, Google Cloud Pub/Sub).
*   **Configurable Notification Channels:** Supports multiple notification channels, including email (using libraries like Nodemailer), SMS (using services like Twilio), and custom webhooks.
*   **Scalable Architecture:** Designed for scalability and low latency, capable of monitoring a large number of assets and delivering timely alerts. WebSocket servers are horizontally scalable and the alert system is asynchronous.
*   **Typescript codebase:** The entire project is written in Typescript, providing strong typing and improved code maintainability.

## Technology Stack

*   **Typescript:** Programming language for the entire project, providing strong typing and improved code maintainability.
*   **Node.js:** Runtime environment for the WebSocket servers and serverless functions.
*   **ethers.js/web3.js:** Libraries for interacting with the Ethereum blockchain and on-chain oracles.
*   **ws:** Library for implementing WebSocket servers in Node.js.
*   **AWS Lambda/Google Cloud Functions:** Serverless platforms for executing alert notifications.
*   **AWS SNS/Google Cloud Pub/Sub:** Message queuing services for asynchronous alert triggering.
*   **Nodemailer/Twilio:** Libraries/services for sending email and SMS notifications.

## Installation

1.  **Clone the repository:**
    git clone https://github.com/uhsr/VolatileWatch.git
    cd VolatileWatch

2.  **Install dependencies:**
    npm install

3.  **Install Typescript globally:**
    npm install -g typescript

4.  **Compile the Typescript code:**
    tsc

5.  **Install serverless function dependencies (if applicable):**
    cd serverless-functions/notification-service
    npm install
    cd ..

## Configuration

The following environment variables need to be configured:

*   `NODE_ENV`: `development` or `production`.
*   `ORACLE_PROVIDER_URL`: URL of the Ethereum node provider (e.g., Infura, Alchemy).
*   `ORACLE_ADDRESS`: Address of the on-chain oracle contract.
*   `ALERT_THRESHOLD`: Percentage deviation threshold for triggering alerts (e.g., 0.05 for 5%).
*   `NOTIFICATION_CHANNEL`: `email`, `sms`, or `webhook`.
*   `EMAIL_ADDRESS`: Email address for sending notifications (if `NOTIFICATION_CHANNEL` is `email`).
*   `EMAIL_PASSWORD`: Password for the email account.
*   `TWILIO_ACCOUNT_SID`: Twilio account SID (if `NOTIFICATION_CHANNEL` is `sms`).
*   `TWILIO_AUTH_TOKEN`: Twilio authentication token.
*   `TWILIO_PHONE_NUMBER`: Twilio phone number.
*   `WEBHOOK_URL`: URL for sending webhook notifications (if `NOTIFICATION_CHANNEL` is `webhook`).

Configuration is primarily managed via a `.env` file at the root of the repository. Example:

ORACLE_PROVIDER_URL=https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID
ORACLE_ADDRESS=0xAbcDeFgHiJkLmNoPqRsTuVwXyZ1234567890
ALERT_THRESHOLD=0.02
NOTIFICATION_CHANNEL=email
EMAIL_ADDRESS=your_email@example.com
EMAIL_PASSWORD=your_email_password

## Usage

1.  **Start the WebSocket server:**
    node dist/websocket-server.js

2.  **Deploy the serverless function:**
    (Refer to the documentation for your chosen serverless platform for deployment instructions.) For AWS Lambda:
    zip -r notification-service.zip *
    aws lambda update-function-code --function-name notification-service --zip-file fileb://notification-service.zip

3.  **Configure the alert thresholds and notification channels:**
    Modify the `.env` file to set the desired alert thresholds and notification channels. Restart the WebSocket server after changes.

The WebSocket server publishes price data in JSON format:

{
    "asset": "ETH/USD",
    "price": 3000.50,
    "timestamp": 1678886400
}

The serverless function receives events from the message queue with the following format:

{
    "asset": "ETH/USD",
    "currentPrice": 3000.50,
    "threshold": 0.05,
    "timestamp": 1678886400
}

## Contributing

We welcome contributions to VolatileWatch! Please follow these guidelines:

*   Fork the repository and create a branch for your feature or bug fix.
*   Write clear and concise commit messages.
*   Submit a pull request with a detailed description of your changes.
*   Ensure that your code adheres to the project's coding standards.

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/uhsr/VolatileWatch/blob/main/LICENSE) file for details.

## Acknowledgements

This project was inspired by the need for a reliable and decentralized price deviation notifier in the DeFi space. We would like to thank the developers and maintainers of the open-source libraries and tools used in this project.