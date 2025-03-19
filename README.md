### Detailed Guide on RabbitMQ and Its Exchanges

#### What is RabbitMQ?
RabbitMQ is a popular message broker system that uses the AMQP (Advanced Message Queuing Protocol). It allows applications to exchange messages asynchronously, ensuring reliability and scalability.

##### Key Components of RabbitMQ:
- **Producer** – Sends messages to the broker (RabbitMQ).
- **Exchange** – Distributes messages to queues.
- **Queue** – Stores messages awaiting processing.
- **Consumer** – Receives messages from the queue.

---

### **Types of Exchanges**
1. **Direct Exchange**
2. **Fanout Exchange**
3. **Topic Exchange**
4. **Headers Exchange**

---

### **Example Code in Python (pika)**

#### **Sending a Message to a Direct Exchange**
```python
import pika  

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.exchange_declare(exchange='direct_logs', exchange_type='direct')

routing_key = 'error'
message = "Critical error occurred!"

channel.basic_publish(exchange='direct_logs', routing_key=routing_key, body=message)

print(f" [x] Sent '{message}' with routing key '{routing_key}'")
connection.close()
```

#### **Receiving Messages from a Queue**
```python
def callback(ch, method, properties, body):
    print(f" [x] Received {body.decode()}")

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='error_queue')
channel.queue_bind(exchange='direct_logs', queue='error_queue', routing_key='error')

channel.basic_consume(queue='error_queue', on_message_callback=callback, auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

---

### **Conclusion**
- **Exchanges** – The mechanism for routing messages in RabbitMQ.
- **Direct Exchange** – Routes messages by exact key.
- **Fanout Exchange** – Broadcasts messages to all queues.
- **Topic Exchange** – Routes messages based on patterns.
- **Headers Exchange** – Routes messages based on headers.

---

### Подробное руководство по RabbitMQ и его Exchanges

#### Что такое RabbitMQ?

RabbitMQ — это популярная система брокера сообщений, использующая протокол AMQP (Advanced Message Queuing Protocol). Он позволяет приложениям обмениваться сообщениями в асинхронном режиме, обеспечивая надёжность и масштабируемость.

##### Основные компоненты RabbitMQ:

- **Producer (издатель)** – отправляет сообщения в брокер (RabbitMQ).
- **Exchange (обменник)** – распределяет сообщения по очередям.
- **Queue (очередь)** – хранит сообщения, ожидающие обработки.
- **Consumer (подписчик)** – получает сообщения из очереди.

---

### **Типы Exchange**

1. **Direct Exchange** (Прямой обменник)
2. **Fanout Exchange** (Широковещательный обменник)
3. **Topic Exchange** (Тематический обменник)
4. **Headers Exchange** (Заголовочный обменник)

---

### **Заключение**

- **Exchanges** – механизм маршрутизации сообщений в RabbitMQ.
- **Direct Exchange** – направляет сообщения по точному ключу.
- **Fanout Exchange** – широковещательная рассылка.
- **Topic Exchange** – маршрутизация по шаблону.
- **Headers Exchange** – маршрутизация на основе заголовков.

