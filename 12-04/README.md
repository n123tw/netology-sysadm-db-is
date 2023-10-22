# Домашнее задание к занятию «Очереди RabbitMQ» Лазарев Даниил
## Задание 1

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/1.jpg)

## Задание 2

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/2.jpg)

Изменил producer.py, прикладываю ниже. Предложенный producer.py не выполнялся с ошибкой:

```
Traceback (most recent call last):
  File "/var/tmp/consumer.py", line 14, in <module>
    channel.basic_consume(callback, queue='hello', no_ack=True)
TypeError: basic_consume() got multiple values for argument 'queue'
```

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/3.jpg)

### modified producer.py:

```
#!/usr/bin/env python
# coding=utf-8
import pika

def publish_message(message, queue_name):
    try:
        connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
        channel = connection.channel()

        # Declare the queue if it doesn't exist
        channel.queue_declare(queue=queue_name)

        # Publish the message to the specified queue
        channel.basic_publish(exchange='', routing_key=queue_name, body=message)

        print(f" [x] Sent: {message} to {queue_name}")

    except Exception as e:
        print(f"An error occurred: {e}")
    finally:
        if connection.is_open:
            connection.close()

if __name__ == '__main__':
    message = 'Hello Netology!'
    queue_name = 'hello'

    publish_message(message, queue_name)
```

## Задание 3

![Скриншот-4](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/4.jpg)

![Скриншот-5](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/5.jpg)

<details>
  <summary>rabbitmqctl cluster_status@rabbit1</summary>

  ```
      Cluster status of node rabbit@rabbit2 ...
    Basics

    Cluster name: rabbit@rabbit2
    Total CPU cores available cluster-wide: 8

    Disk Nodes

    rabbit@rabbit1
    rabbit@rabbit2

    Running Nodes

    rabbit@rabbit1
    rabbit@rabbit2

    Versions

    rabbit@rabbit2: RabbitMQ 3.12.7 on Erlang 26.1.2
    rabbit@rabbit1: RabbitMQ 3.12.7 on Erlang 26.1.2

    CPU Cores

    Node: rabbit@rabbit2, available CPU cores: 4
    Node: rabbit@rabbit1, available CPU cores: 4

    Maintenance status

    Node: rabbit@rabbit2, status: not under maintenance
    Node: rabbit@rabbit1, status: not under maintenance

    Alarms

    (none)

    Network Partitions

    (none)

    Listeners

    Node: rabbit@rabbit2, interface: [::], port: 15672, protocol: http, purpose: HTTP API
    Node: rabbit@rabbit2, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
    Node: rabbit@rabbit2, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
    Node: rabbit@rabbit1, interface: [::], port: 15672, protocol: http, purpose: HTTP API
    Node: rabbit@rabbit1, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
    Node: rabbit@rabbit1, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

    Feature flags

    Flag: classic_mirrored_queue_version, state: enabled
    Flag: classic_queue_type_delivery_support, state: enabled
    Flag: direct_exchange_routing_v2, state: enabled
    Flag: drop_unroutable_metric, state: enabled
    Flag: empty_basic_get_metric, state: enabled
    Flag: feature_flags_v2, state: enabled
    Flag: implicit_default_bindings, state: enabled
    Flag: listener_records_in_ets, state: enabled
    Flag: maintenance_mode_status, state: enabled
    Flag: quorum_queue, state: enabled
    Flag: restart_streams, state: enabled
    Flag: stream_queue, state: enabled
    Flag: stream_sac_coordinator_unblock_group, state: enabled
    Flag: stream_single_active_consumer, state: enabled
    Flag: tracking_records_in_ets, state: enabled
    Flag: user_limits, state: enabled
    Flag: virtual_host_metadata, state: enabled

  ```
</details>