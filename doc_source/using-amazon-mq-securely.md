# Using Amazon MQ Securely<a name="using-amazon-mq-securely"></a>

The following design patterns can improve the security of your Amazon MQ broker\.

**Topics**
+ [Prefer Brokers without Public Accessibility](#prefer-brokers-without-public-accessibility)
+ [Always Use Client\-Side Encryption as a Complement to TLS](#always-use-client-side-encryption-complement-tls)
+ [Always Configure an Authorization Map](#always-configure-authorization-map)
+ [Always Configure a System Group](#always-configure-system-group)

## Prefer Brokers without Public Accessibility<a name="prefer-brokers-without-public-accessibility"></a>

Brokers created without public accessibility can't be accessed from outside of your [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)\. This greatly reduces your broker's susceptibility to Distributed Denial of Service \(DDoS\) attacks from the public internet\. For more information, see [Accessing the ActiveMQ Web Console of a Broker without Public Accessibility](accessing-web-console-of-broker-without-private-accessibility.md) in this guide and [How to Help Prepare for DDoS Attacks by Reducing Your Attack Surface](http://aws.amazon.com/blogs/security/how-to-help-prepare-for-ddos-attacks-by-reducing-your-attack-surface/) on the AWS Security Blog\.

## Always Use Client\-Side Encryption as a Complement to TLS<a name="always-use-client-side-encryption-complement-tls"></a>

You can access your brokers using the following protocols with TLS enabled:
+ [AMQP](http://activemq.apache.org/amqp.html)
+ [MQTT](http://activemq.apache.org/mqtt.html)
+ MQTT over [WebSocket](http://activemq.apache.org/websockets.html)
+ [OpenWire](http://activemq.apache.org/openwire.html)
+ [STOMP](http://activemq.apache.org/stomp.html)
+ STOMP over WebSocket

Amazon MQ encrypts messages at rest and in transit using encryption keys that it manages and stores securely\. For additional security, we highly recommend designing your application to use client\-side encryption\. For more information, see the *[AWS Encryption SDK Developer Guide](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/)*\.

## Always Configure an Authorization Map<a name="always-configure-authorization-map"></a>

Because ActiveMQ has no authorization map configured by default, any authenticated user can perform any action on the broker\. Thus, it is a best practice to restrict permissions *by group*\. For more information, see `[authorizationEntry](child-element-details.md#authorizationEntry)`\.

## Always Configure a System Group<a name="always-configure-system-group"></a>

Amazon MQ uses a *system group* \(called `activemq-webconsole`\) to allow the [ActiveMQ Web Console](http://activemq.apache.org/web-console.html) to communicate with the ActiveMQ broker\.

The settings for the `activemq-webconsole` group in the authorization map restrict which operations can be performed on queues or topics from the web console\. For more information and an example configuration, see `[authorizationEntry](child-element-details.md#authorizationEntry)`\.

**Important**  
If you specify an authorization map which doesn't include the `activemq-webconsole` group, you can't use the ActiveMQ Web Console because the group isn't authorized to send messages to, or receive messages from, the Amazon MQ broker\.