# rabbitmq-pub-sub

Publish/Subscribe using a RabbitMQ Fanout Exchange.

## Run with Docker.  

 `docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management`

## Usage

If you want to save logs to a file, just open a console and type:
```bash
go run receiver/receive_logs.go &> logs_from_rabbit.log
```
If you wish to see the logs on your screen, spawn a new terminal and run:
```bash
go run receiver/receive_logs.go
```

To emit logs type:
```bash
go run emit_log.go
```

Using `rabbitmqctl list_bindings` you can verify that the code actually creates bindings and queues as we want

###  Forgotten acknowledgment
It's a common mistake to miss the ack. It's an easy error, but the consequences are serious. Messages will be redelivered when your client quits (which may look like random redelivery), but RabbitMQ will eat more and more memory as it won't be able to release any unacked messages.

In order to debug this kind of mistake you can use rabbitmqctl to print the messages_unacknowledged field:

```bash
docker exec -it rabbitmq /bin/sh
rabbitmqctl list_queues name messages_ready messages_unacknowledged
```