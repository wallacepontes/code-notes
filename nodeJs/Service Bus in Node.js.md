# Service Bus in Node.js

## Table of Contents
- [Service Bus in Node.js](#service-bus-in-nodejs)
  - [Table of Contents](#table-of-contents)
  - [🚀 Tutorial: Getting Started with Azure Service Bus in Node.js](#-tutorial-getting-started-with-azure-service-bus-in-nodejs)
    - [🧭 Overview](#-overview)
  - [1️⃣ Prerequisites](#1️⃣-prerequisites)
  - [2️⃣ Install Required Packages](#2️⃣-install-required-packages)
  - [3️⃣ Create a Service Bus Namespace and Queue in Azure](#3️⃣-create-a-service-bus-namespace-and-queue-in-azure)
  - [4️⃣ Create a `.env` File](#4️⃣-create-a-env-file)
  - [5️⃣ Sending Messages (Producer)](#5️⃣-sending-messages-producer)
  - [6️⃣ Receiving Messages (Consumer)](#6️⃣-receiving-messages-consumer)
  - [7️⃣ Topics and Subscriptions (Pub/Sub)](#7️⃣-topics-and-subscriptions-pubsub)
  - [8️⃣ Error Handling and Retry Policies](#8️⃣-error-handling-and-retry-policies)
  - [9️⃣ Best Practices](#9️⃣-best-practices)
  - [🔍 Example Folder Structure](#-example-folder-structure)
  - [🧩 Alternatives](#-alternatives)
  - [References](#references)

Here’s a **step-by-step tutorial** to help you **get started with implementing a Service Bus in Node.js**, using **Azure Service Bus** as a practical example (the same principles apply to AWS SQS/SNS or RabbitMQ).

---

## 🚀 Tutorial: Getting Started with Azure Service Bus in Node.js

### 🧭 Overview

**Azure Service Bus** is a **message broker** that allows different parts of your system (microservices, APIs, etc.) to communicate asynchronously and reliably.

It’s great for:

* Decoupling services
* Handling spikes in traffic
* Implementing event-driven architectures

---

## 1️⃣ Prerequisites

* Node.js ≥ 18
* An Azure account ([free tier available](https://azure.microsoft.com/free/))
* Basic understanding of queues and topics

---

## 2️⃣ Install Required Packages

Create a new Node.js project:

```bash
mkdir node-service-bus
cd node-service-bus
npm init -y
```

Install the Azure Service Bus SDK:

```bash
npm install @azure/service-bus
```

---

## 3️⃣ Create a Service Bus Namespace and Queue in Azure

1. Go to the **Azure Portal**
2. Search for **Service Bus** → Click **Create**
3. Create a **Namespace** (e.g., `my-namespace`)
4. Inside it, create a **Queue** (e.g., `orders-queue`)
5. Go to **Shared access policies** → Copy the **Primary Connection String**

---

## 4️⃣ Create a `.env` File

```bash
npm install dotenv
```

Then create `.env`:

```bash
SERVICE_BUS_CONNECTION_STRING=Endpoint=sb://my-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXX
QUEUE_NAME=orders-queue
```

---

## 5️⃣ Sending Messages (Producer)

Create a file `sendMessage.js`:

```js
import { ServiceBusClient } from "@azure/service-bus";
import dotenv from "dotenv";
dotenv.config();

const connectionString = process.env.SERVICE_BUS_CONNECTION_STRING;
const queueName = process.env.QUEUE_NAME;

async function main() {
  const sbClient = new ServiceBusClient(connectionString);
  const sender = sbClient.createSender(queueName);

  try {
    const message = {
      body: { orderId: 101, item: "Laptop", quantity: 2 },
      contentType: "application/json",
      label: "OrderCreated",
    };

    console.log("📦 Sending message:", message);
    await sender.sendMessages(message);
    console.log("✅ Message sent successfully");
  } finally {
    await sender.close();
    await sbClient.close();
  }
}

main().catch(console.error);
```

Run it:

```bash
node sendMessage.js
```

---

## 6️⃣ Receiving Messages (Consumer)

Create `receiveMessage.js`:

```js
import { ServiceBusClient } from "@azure/service-bus";
import dotenv from "dotenv";
dotenv.config();

const connectionString = process.env.SERVICE_BUS_CONNECTION_STRING;
const queueName = process.env.QUEUE_NAME;

async function main() {
  const sbClient = new ServiceBusClient(connectionString);
  const receiver = sbClient.createReceiver(queueName);

  const messageHandler = async (message) => {
    console.log("📨 Received message:", message.body);
    // process message...
    await receiver.completeMessage(message);
  };

  const errorHandler = (error) => {
    console.error("❌ Error receiving message:", error);
  };

  receiver.subscribe({
    processMessage: messageHandler,
    processError: errorHandler,
  });

  console.log("👂 Listening for messages...");
}

main().catch(console.error);
```

Run it:

```bash
node receiveMessage.js
```

Now you’ll see the consumer print out the message sent by the producer.

---

## 7️⃣ Topics and Subscriptions (Pub/Sub)

For publish-subscribe (multiple subscribers):

* Create a **Topic** instead of a queue in Azure.
* Create one or more **Subscriptions**.
* Replace `createSender(queueName)` with `createSender(topicName)`.
* Use `createReceiver(topicName, subscriptionName)` to consume.

---

## 8️⃣ Error Handling and Retry Policies

You can control retry behavior via Service Bus client options:

```js
const sbClient = new ServiceBusClient(connectionString, {
  retryOptions: {
    maxRetries: 3,
    retryDelayInMs: 2000,
  },
});
```

Messages that fail repeatedly are moved to a **Dead Letter Queue (DLQ)** for further inspection.

---

## 9️⃣ Best Practices

✅ Use **message sessions** for ordered message processing
✅ Use **dead-letter queues** to handle poison messages
✅ Log message IDs and timestamps for observability
✅ Avoid long-running operations in the consumer handler
✅ Consider **Service Bus Topics** for event-driven architectures

---

## 🔍 Example Folder Structure

```
node-service-bus/
├── .env
├── package.json
├── sendMessage.js
├── receiveMessage.js
└── README.md
```

---

## 🧩 Alternatives

If you’re not using Azure, similar SDKs exist:

| Platform           | Library                                       | Notes                     |
| ------------------ | --------------------------------------------- | ------------------------- |
| **AWS SQS/SNS**    | `@aws-sdk/client-sqs` / `@aws-sdk/client-sns` | For queues and pub/sub    |
| **RabbitMQ**       | `amqplib`                                     | Self-managed or CloudAMQP |
| **Google Pub/Sub** | `@google-cloud/pubsub`                        | GCP-managed pub/sub       |

---

Would you like me to show you a **comparison** between Azure Service Bus and alternatives like **RabbitMQ** or **AWS SQS** — including when to choose each one?


## References
1. https://github.com/boringnode/bus
2. https://www.npmjs.com/search?q=Message%20Bus
3. https://medium.com/@edhalliwell/using-rabbitmq-and-kafka-to-send-messages-between-applications-84c962420149
4. 