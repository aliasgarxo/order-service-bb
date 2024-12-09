
# Order Service for Best Buy Application

The Order Service provides a robust API for submitting customer orders within the Best Buy application. It interfaces with a message queue (RabbitMQ or Azure Service Bus) to ensure reliable processing.

## Prerequisites

- [Node.js](https://nodejs.org/en/download/)
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Message Queue Options

The service can connect to RabbitMQ or Azure Service Bus via AMQP 1.0.

### Option 1: RabbitMQ

1. Run RabbitMQ using the provided `docker-compose.yml`:

```bash
docker compose up
```

2. Set the environment variables:

```bash
cat << EOF > .env
ORDER_QUEUE_HOSTNAME=localhost
ORDER_QUEUE_PORT=5672
ORDER_QUEUE_USERNAME=username
ORDER_QUEUE_PASSWORD=password
ORDER_QUEUE_NAME=orders
EOF

source .env
```

### Option 2: Azure Service Bus

1. Create a Service Bus namespace and queue using Azure CLI:

```bash
az group create --name <resource-group> --location <location>
az servicebus namespace create --name <namespace-name> --resource-group <resource-group>
az servicebus queue create --name orders --namespace-name <namespace-name> --resource-group <resource-group>
```

2. Configure environment variables for Azure Service Bus:

```bash
cat << EOF > .env
USE_WORKLOAD_IDENTITY_AUTH=true
AZURE_SERVICEBUS_FULLYQUALIFIEDNAMESPACE=<your-namespace>
ORDER_QUEUE_NAME=orders
EOF

source .env
```

## Running the Service Locally

Install dependencies and start the service:

```bash
npm install
npm run dev
```

Access the service at `http://127.0.0.1:3000`.

### Testing the API

Use the `test-order-service.http` file in the repository to test the API endpoints.
    