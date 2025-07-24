Here is a complete `README.md` file for a PHP-based RabbitMQ messaging system using `php-amqplib`. You can paste this directly into your project’s `README.md` file.

---

# PHP RabbitMQ Messaging System

A simple PHP project demonstrating **how to send and receive messages using RabbitMQ**. This project uses the [php-amqplib](https://github.com/php-amqplib/php-amqplib) library and is great for learning the basics of message queues in PHP.

---

## 📦 Requirements

- PHP 7.4 or later
- [Composer](https://getcomposer.org/)
- [RabbitMQ](https://www.rabbitmq.com/) (running locally or remotely)
- [Erlang](https://www.erlang.org/downloads) (required by RabbitMQ)

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/shrikant-28/php-rabbitmq-messaging.git
cd php-rabbitmq-messaging
````

### 2. Install PHP Dependencies

```bash
composer install
```

### 3. Start RabbitMQ

Ensure RabbitMQ is running on your machine. You can install RabbitMQ from:

* [RabbitMQ Downloads](https://www.rabbitmq.com/download.html)
* Use Docker (optional):

  ```bash
  docker run -d --hostname my-rabbit --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
  ```

### 4. Access RabbitMQ Web UI

Visit:

```
http://localhost:15672
```

* **Username:** `guest`
* **Password:** `guest`

---

## ✉️ Sending Messages (Producer)

To send a message to the queue:

```bash
php send.php
```

This will publish a "Hello World!" message to the `hello` queue.

---

## 📥 Receiving Messages (Consumer)

To consume messages from the queue:

```bash
php receive.php
```

This script listens to the `hello` queue and prints any messages it receives.

---

## 📂 Project Structure

```
php-rabbitmq-messaging/
├── vendor/             # Composer dependencies
├── send.php            # Script to send messages
├── receive.php         # Script to receive messages
├── composer.json       # PHP dependencies
└── README.md           # This file
```

---

## 📝 Example Code

### send.php

```php
<?php
require_once __DIR__ . '/vendor/autoload.php';

use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

$msg = new AMQPMessage('Hello World!');
$channel->basic_publish($msg, '', 'hello');

echo " [x] Sent 'Hello World!'\n";

$channel->close();
$connection->close();
```

### receive.php

```php
<?php
require_once __DIR__ . '/vendor/autoload.php';

use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

echo " [*] Waiting for messages. To exit press CTRL+C\n";

$callback = function($msg) {
    echo ' [x] Received ', $msg->body, "\n";
};

$channel->basic_consume('hello', '', false, true, false, false, $callback);

while ($channel->is_consuming()) {
    $channel->wait();
}
```

---

## 🔧 Customization

* You can change the queue name from `'hello'` to anything you like.
* Extend the message content to JSON, notifications, task payloads, etc.
* Integrate with Laravel Queue system for advanced use cases.

---

## 📚 Resources

* [RabbitMQ Official Site](https://www.rabbitmq.com/)
* [php-amqplib GitHub](https://github.com/php-amqplib/php-amqplib)
* [Laravel Queues Documentation](https://laravel.com/docs/queues) (for Laravel integration)

---

## 📌 License

This project is open-sourced under the MIT License.
