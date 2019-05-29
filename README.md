"RabbitMQ Cluster POC"

rabbit1:
  image: rabbitmq-cluster
  hostname: rabbit1
  environment:
    - ERLANG_COOKIE=abcdefg
  ports:
    - "5672:5672"
    - "15672:15672"
rabbit2:
  image: rabbitmq-cluster
  hostname: rabbit2
  links:
    - rabbit1
  environment:
    - ERLANG_COOKIE=abcdefg
    - CLUSTER_WITH=rabbit1
    - ENABLE_RAM=true
    - RAM_NODE=true
  ports:
    - "5673:5672"
    - "15673:15672"
rabbit3:
  image: rabbitmq-cluster
  hostname: rabbit3
  links:
    - rabbit1
    - rabbit2
  environment:
    - ERLANG_COOKIE=abcdefg
    - CLUSTER_WITH=rabbit1
  ports:
    - "5674:5672"

If needed, additional nodes can be added to this file.

Once cluster is up:
* The management console can be accessed at `http://hostip:15672`
* The connection host should look like this: `hostip:5672,hostip:5673,hostip:5674
