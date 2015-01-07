# eMQTT

eMQTT is a scalable, fault-tolerant and extensible mqtt broker written in Erlang/OTP.

eMQTT support MQTT V3.1 Protocol Specification.

eMQTT requires Erlang R17+.

## Startup in Five Minutes

```
$ git clone git://github.com/slimpp/emqtt.git

$ cd emqtt

$ make && make dist

$ cd rel/emqtt

$ ./bin/emqtt console
```

## Deploy and Start

### start

```
cp -R rel/emqtt $INSTALL_DIR

cd $INSTALL_DIR/emqtt

./bin/emqtt start

```

### stop

```
./bin/emqtt stop

```

## Configuration

### etc/app.config

```
{emqtt, [
   {auth, {anonymous, []}}, %internal, anonymous
   {listen, [
       {mqtt, 1883, [
           {max_conns, 1024},
           {acceptor_pool, 4}
       ]},
       {http, 8883, [
           {max_conns, 512},
           {acceptor_pool, 1}
       ]}
   ]}
]}

```

### etc/vm.args

```

-sname emqtt

-setcookie emqtt

```

When nodes clustered, vm.args should be configured as below:

```
-name emqtt@host1
```

......

## Cluster

Suppose we cluster two nodes on 'host1', 'host2', steps:

on 'host1':

```
./bin/emqtt start
```

on 'host2':

```
./bin/emqtt start

./bin/emqtt_ctl cluster emqtt@host1
```

Run './bin/emqtt_ctl cluster' on 'host1' or 'host2' to check cluster nodes.

## HTTP API

eMQTT support http to publish message.

Example:

```
curl -v --basic -u user:passwd -d "topic=/a/b/c&message=hello from http..." -k http://localhost:8883/mqtt/publish
```

### URL

```
HTTP POST http://host:8883/mqtt/publish
```

### Parameters

Name | Description
-----|-------------
topic | MQTT Topic
message | Text Message

## Design

[Design Wiki](https://github.com/slimpp/emqtt/wiki)

## License

The MIT License (MIT)

## Author

feng at slimchat.io

