# MQTT.dart Client Example

## Overview
This is an MQTT client example project that showcases how you can use HiveMQ Cloud with the Dart Client. The example project covers the basic MQTT functionality: Connecting MQTT clients to your HiveMQ Cloud cluster, subscribing to topics and publishing data (sending and receiving messages using the MQTT protocol).

The Dart client used here supports MQTT v3.1 and v3.1.1.
You can find documentation for this client library here: https://www.hivemq.com/blog/mqtt-client-library-mqtt-dart/.

This example repository is easily and clearly structured, so you can start quickly:
This readme file is your starting point. Here we will describe what you have to do step-by-step to get started with this example.
[``main.dart``](bin/main.dart) in the ``bin`` directory is a simple implementation that demonstrates the core functionality of an MQTT Client.

Follow the instructions in the following paragraphs to get started yourself.

## HiveMQ Cloud
[HiveMQ](https://www.hivemq.com/) is the industry leader for enterprise-ready, beautifully scalable, large-scale IoT deployments with MQTT. We help companies connect things to the Internet. Our MQTT-based messaging platform ensures fast, reliable, and secure movement of data to and from connected IoT devices for companies around the world. HiveMQ Cloud is our fully-managed, cloud-native IoT messaging platform that makes trustworthy and scalable IoT device connectivity simple. You can learn more about HiveMQ Cloud on our [website](https://www.hivemq.com/mqtt-cloud-broker/), [documentation](https://www.hivemq.com/docs/hivemq-cloud/introduction.html)  and our [blog posts](https://www.hivemq.com/tags/cloud/).

## Getting started
[By signing up](https://console.hivemq.cloud) for HiveMQ Cloud you will automatically get access to a HiveMQ Cloud Basic cluster. HiveMQ Cloud Basic is our smallest package that allows you to connect up to 100 MQTT clients for free and test the full MQTT functionality. 

The [HiveMQ Cloud Quick Start Guide](https://www.hivemq.com/docs/hivemq-cloud/introduction.html#guide) gives you step-by-step instructions on how to set up your HiveMQ Cloud account, create clusters, and connect MQTT clients.


### Prerequisites 
After signing up, you have a running HiveMQ Cloud cluster, that you can use in this example.
Now clone this repository into your local IDE.

For using the code examples, you need to install the necessary client library.
The correct dependencies are listed in the ``pubspec.yaml``.
Execute this command in the terminal of your IDE to get the right dependencies.
```sh
dart pub get
```

### Broker credentials
To define the HiveMQ Cloud cluster which should be targeted, you need to fill the placeholders in the code with your host name, username and password.
The <b>host name</b> can be found in the <b>Details</b> section of the <b>Overview</b> tab of your cluster.
![cluster overview](/img/hivemq-cloud-cluster-overview.png)

After the cluster is created, add a set of credentials that you can use in this example or future implementations.
Use any secure username and password you desire.
The <b>username</b> and <b>password</b> are the values used as <b>Client Credentials</b> that you just created.
![credentials](/img/hivemq-cloud-credentials.png)

### Code Examples
This example project covers the core functionality of an MQTT client interacting with HiveMQ Cloud.
To securely connect the MQTT client with HiveMQ Cloud you need to enable TLS.
Use your username and password, to authenticate your MQTT client at HiveMQ Cloud.
To connect the client, use the port 8883 that is standard for secure MQTT communication. 
``'<your_name>'`` is the name you give your client, you can choose any name for this.

```dart
await client.connect('<your_username>', '<your_password>');
```
```dart
client = MqttServerClient.withPort('<your_host>', '<your_name>', <your_port>);
```

The code located inside [``main.dart``](bin/main.dart) connects to the configured HiveMQ Cloud Broker in a simple way. 
This is a ready-set example that can simply be run after inputting your credentials.
Navigate to the ``bin`` directory and run [``main.dart``](bin/main.dart) in your terminal.

```sh
cd bin
dart main.dart
```

The different processes are all defined in their respective functions.
These get all called in the ``prepareMqttClient()`` function at the start, which in turn gets called by the ``main`` function with ``newclient.prepareMqttClient()``.
After connecting the client, the code first subscribes to the topic filter ``"Dart/Mqtt_client/testtopic"``.  
That means the MQTT client receives all messages that are published to this [topic filter](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/).

```dart
void _subscribeToTopic(String topicName) {
  print('Subscribing to the $topicName topic');
  client.subscribe(topicName, MqttQos.atMostOnce);

  // print the message when it is received
  client.updates.listen((List<MqttReceivedMessage<MqttMessage>> c) {
    final MqttPublishMessage recMess = c[0].payload;
    var message = MqttPublishPayload.bytesToStringAsString(recMess.payload.message);

    print('YOU GOT A NEW MESSAGE:');
    print(message);
   });
}
```

The ``_onSubscribed`` callback acts as a reassurance that the subscription worked.
Then the code publishes a message to the same topic with ``_publishMessage``.
The message gets printed to the terminal by the ``_subscribeToTopic`` method, that listens for incoming messages.

```dart
void _publishMessage(String message) {
  final MqttClientPayloadBuilder builder = MqttClientPayloadBuilder();
  builder.addString(message);

  print('Publishing message "$message" to topic ${'Dart/Mqtt_client/testtopic'}');
  client.publishMessage('Dart/Mqtt_client/testtopic', MqttQos.exactlyOnce, builder.payload);
}
```

## Learn more
If you want to learn more about MQTT, visit the [MQTT Essentials](https://www.hivemq.com/mqtt-essentials/) guide, that explains the core of MQTT concepts, its features and other essential information. Learn about Dart, a client-optimized programming language for fast apps on any platform, on their [website](https://dart.dev/). Also have a look at the [client library](https://pub.dev/packages/mqtt_client).

## Contributing

Please see our [contributing guidelines](./CONTRIBUTING.adoc) and [code of conduct](./code-of-conduct.md).

## License

[Apache 2.0](./LICENSE).

