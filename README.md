# Order Processing System

This repository contains a simple order placement and processing system that utilizes RabbitMQ as a messaging broker for communication between components.

## Modules

### Order Placement

The Order Placement module is responsible for receiving and placing orders. It exposes an HTTP endpoint `/place-order` to accept order requests. When an order is received, it establishes a connection to RabbitMQ, publishes the order message to an exchange, and sends a response indicating the successful placement of the order.

### Order Processing Service

The Order Processing Service module processes the orders received from the Order Placement module. It establishes a connection to RabbitMQ, consumes order messages from a queue, and asynchronously processes each order. After processing an order, it acknowledges the message, indicating successful processing.

### Inventory Management Service

The Inventory Management Service module updates the inventory based on the received orders. It establishes a connection to RabbitMQ, consumes order messages from a queue, and asynchronously updates the inventory. After updating the inventory, it acknowledges the message.

### Email Notifications Service

The Email Notifications Service module sends email notifications to customers after their orders have been placed. It establishes a connection to RabbitMQ, consumes order messages from a queue, and asynchronously sends email notifications to customers. After sending the email notification, it acknowledges the message.

## Prerequisites

- Node.js and npm should be installed on your system.
- RabbitMQ server should be running and accessible.
- SMTP email server or service with valid credentials (if using the Email Notifications Service).

## Configuration

The system's configuration is stored in the `config.js` module. It exports the RabbitMQ URL and email configuration details used by the other modules. The configuration values are retrieved from environment variables defined in the `.env` file.

### Specifying Environment Variables

To configure the Order Processing System, you need to set the following environment variables in the `.env` file:

- `RABBITMQ_URL`: The URL of the RabbitMQ server. For example, `amqp://localhost`.

- `EMAIL_HOST`: The SMTP email server address for sending email notifications. For example, `smtp.example.com`.

- `EMAIL_PORT`: The port number of the SMTP email server. For example, `2525`.

- `EMAIL_USER`: The username for authenticating with the SMTP email server.

- `EMAIL_PASSWORD`: The password for authenticating with the SMTP email server.

Make sure to provide valid credentials if you intend to use the Email Notifications Service. If any of the SMTP-related variables are not set, the email notifications functionality will be disabled.

## Getting Started

To run the Order Processing System, follow these steps:

1. Clone the repository and navigate to the project directory.
2. Create a `.env` file and set the required environment variables, including the RabbitMQ URL and email configuration details.
3. Install the dependencies by running `npm install`.
4. Start the Order Placement module by running `node order-placement.js`.
5. Start the Order Processing Service module by running `node order-processing-service.js`.
6. Start the Inventory Management Service module by running `node inventory-management-service.js`.
7. Start the Email Notifications Service module by running `node email-notifications-service.js`.

Ensure that RabbitMQ is running and accessible with the provided URL.

Once the system is up and running, you can place orders by sending HTTP POST requests to the `/place-order` endpoint with the necessary order details in the request body.

## Example

Here's an example of how to place an order using cURL:

```shell
curl -X POST -H "Content-Type: application/json" -d '{
  "id": "123",
    "customerId": "67890", 
    "customerName": "test",
    "customerEmail": "customerEmail@test.com",
    "items": [
    {
      "product_id": "P001",
      "quantity": 2
    },
    {
      "product_id": "P002",
      "quantity": 1
    }
  ]
}' http://localhost:3000/place-order
```
This example places an order with the order ID "123", customer name "test", customer email "customerEmail@test.com", and two items with product IDs "P001" and "P002" along with their respective quantities.