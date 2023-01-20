# Overview

This is a transitional build of activemq-client jar for use with Jakarta JMS 3.0 namespaces for use with projects until the Apache ActiveMQ project completes a Jakarta release.

# Usage notes

## Maven dependency
```
<dependency>
  <groupId>io.hyte.activemq</groupId>
  <artifactId>activemq-client-jakarta</artifactId>
  <version>5.18.0.hyte-1</version>
</dependency>
```

## Sample code
```
package io.hyte.activemq;

import org.apache.activemq.ActiveMQConnectionFactory;

import jakarta.jms.Connection;
import jakarta.jms.Message;
import jakarta.jms.MessageConsumer;
import jakarta.jms.MessageProducer;
import jakarta.jms.Session;
import jakarta.jms.TextMessage;

public class JakartaClient {

    public static void main(String[] args) throws Exception {
        ActiveMQConnectionFactory activemqConnectionFactory = new ActiveMQConnectionFactory("admin", "admin", "tcp://localhost:61616");

        Connection connection = activemqConnectionFactory.createConnection();
        connection.start();
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        MessageProducer messageProducer = session.createProducer(session.createQueue("JAKARTA.1"));

        messageProducer.send(session.createTextMessage("hello jakarta!"));
        MessageConsumer messageConsumer = session.createConsumer(session.createQueue("JAKARTA.1"));
        Message tmpMessage = messageConsumer.receive(2000);

        if(TextMessage.class.isAssignableFrom(tmpMessage.getClass())) {
            System.out.println("body: " + TextMessage.class.cast(tmpMessage).getText());
        }
    }
}
```
