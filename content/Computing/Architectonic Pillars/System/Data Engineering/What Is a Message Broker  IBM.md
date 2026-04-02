## What is a message broker?

A message broker is software that enables applications, systems and services to communicate with each other and exchange information.

The message broker does this by ==t**ranslating messages between formal messaging protocols.**== This allows interdependent services to “talk” with one another directly, even if they were written in different languages or implemented on different platforms.

Message brokers are software modules within messaging middleware or message-oriented middleware (MOM) solutions. This type of middleware provides developers with a standardized means of handling the flow of data between an application’s components so that they can focus on its core logic. It can serve as a distributed communications layer that allows applications spanning multiple platforms to communicate internally.

Message brokers can validate, store, route, and deliver messages to the appropriate destinations. They serve as intermediaries between other applications, allowing senders to issue messages without knowing where the receivers are, whether or not they are active, or how many of them there are. This ==facilitates decoupling of processes and services within systems.==

In order to provide reliable message storage and guaranteed delivery, message brokers often rely on a ==substructure or component called a [message queue](https://www.ibm.com/topics/message-queues) that stores and orders the messages until the consuming applications can process them.== In a message queue, messages are stored in the exact order in which they were transmitted and remain in the queue until receipt is confirmed.

Asynchronous messaging refers to the type of inter-application communication that message brokers make possible. It prevents the loss of valuable data and enables systems to continue ==functioning even in the face of the intermittent connectivity or latency issues common on public [networks](https://www.ibm.com/topics/networking). ==Asynchronous messaging guarantees that messages will be delivered once (and once only) in the correct order relative to other messages.

Message brokers may comprise queue managers to handle the interactions between multiple message queues, as well as services providing data routing, message translation, persistence, and client state management functionalities.


## Message broker models

Message brokers offer two basic message distribution patterns or messaging styles:

**Point-to-point messaging**: This is the distribution pattern utilized in message queues with a one-to-one relationship between the message’s sender and receiver. ==Each message in the queue is sent to only one recipient and is consumed only once.== Point-to-point messaging is called for when a message must be acted upon only one time. Examples of suitable use cases for this messaging style ==include payroll and financial transaction processing.== In these systems, ==both senders and receivers need a guarantee that each payment will be sent once and once only.== 

**Publish/subscribe messaging**: In this message distribution pattern, often referred to as “pub/sub,” the ==producer of each message publishes it to a topic, and multiple message consumers subscribe to topics from which they want to receive messages.== All messages published to a topic are distributed to all the applications subscribed to it. This is a ==broadcast-style distribution method,== in which there is a ==one-to-many relationship between the message’s publisher and its consumers.== If, for example, an airline were to disseminate updates about the landing times or delay status of its flights, multiple parties could make use of the information: ground crews performing aircraft maintenance and refueling, baggage handlers, flight attendants and pilots preparing for the plane’s next trip, and the operators of visual displays notifying the public. A pub/sub messaging style would be appropriate for use in this scenario.

## Message brokers in cloud architectures

[Cloud-native applications](https://www.ibm.com/topics/cloud-native) are built to take advantage of the inherent benefits of [cloud computing](https://www.ibm.com/topics/cloud-computing), including flexibility, scalability, and rapid deployment. These applications are made up of small, discrete, reusable components called [microservices](https://www.ibm.com/topics/microservices). Each microservice is deployed and can run independently of the others. This means that any one of them can be updated, scaled, or restarted without affecting other services in the system. Often packaged in [containers](https://www.ibm.com/topics/containers), microservices work together to comprise a whole application, though each has its own stack, including a database and data model that may be different from the others.

==Microservices must have a means of communicating with one another in order to operate in concert. Message brokers are one mechanism they use to create this shared communications backbone.==

Message brokers are often used to manage communications between on-premises systems and cloud components in [hybrid cloud](https://www.ibm.com/topics/hybrid-cloud) environments. Using a message broker gives increased control over interservice communications, ensuring that data is sent securely, reliably, and efficiently between the components of an application. Message brokers can play a similar role in integrating [multicloud](https://www.ibm.com/topics/multicloud) environments, enabling communication between workloads and runtimes residing on different platforms. They’re also well suited for use in [serverless computing](https://www.ibm.com/topics/serverless), in which individual cloud-hosted services run on demand on a per-request basis.

## Message brokers vs. APIs

[REST APIs](https://www.ibm.com/topics/rest-apis) are commonly used for communications between microservices. The term Representational State Transfer (REST) defines a set of principles and constraints that developers can follow when building web services. Any services that adhere to them will be able to communicate via a set of uniform shared stateless operators and requests. Application Programming Interface (API) denotes the underlying code that, if it conforms to REST rules, allows the services to talk to one another.

==REST APIs use Hypertext Transfer Protocol (HTTP) to communicate. Because HTTP is the standard transport protocol of the public Internet, REST APIs are widely known, frequently used, and broadly interoperable. HTTP is a request/response protocol, however, so it is best used in situations that call for a synchronous request/reply. ==This means that services making requests via REST APIs must be designed to expect an immediate response. If the client receiving the response is down, the sending service will be blocked while it awaits the reply. ==Failover and error handling logic should be built into both services.==

==Message brokers enable asynchronous communications between services so that the sending service need not wait for the receiving service’s reply. This improves fault tolerance and resiliency== in the systems in which they’re employed. In addition, the use of message brokers makes it easier to scale systems since a pub/sub messaging pattern can readily support changing numbers of services. Message brokers also keep track of consumers’ states.

## Message brokers vs. event streaming platforms

Whereas message brokers can support two or more messaging patterns, including message queues and pub/sub, event streaming platforms only offer pub/sub-style distribution patterns. Designed for use with high volumes of messages, event streaming platforms are readily scalable. They’re capable of ordering streams of records into categories called topics and storing them for a predetermined amount of time. Unlike message brokers, however, event streaming platforms cannot guarantee message delivery or track which consumers have received messages.

Event streaming platforms offer more scalability than message brokers but fewer features that ensure fault tolerance (like message resending), as well as more limited message routing and queueing capabilities.

Learn more about [event driven architecture](https://www.ibm.com/topics/event-driven-architecture).

## Message broker vs. ESB (enterprise service bus)

An [enterprise service bus (ESB)](https://www.ibm.com/topics/esb) is an architectural pattern sometimes utilized in [service-oriented architecture (SOAs)](https://www.ibm.com/topics/soa) implemented across enterprises. In an ESB, a centralized software platform combines communication protocols and data formats into a “common language” that all services and applications in the architecture can share. It might, for instance, translate the requests it receives from one protocol (such as XML) to another (such as JSON). ESBs transform their message payloads using an automated process. The centralized software platform also handles other orchestration logic, such as connectivity, routing, and request processing.

ESB infrastructures are complex, however, and can be challenging to integrate and expensive to maintain. It’s difficult to troubleshoot them when problems occur in production environments, they’re not easy to scale, and updating is tedious.

Message brokers are a “lightweight” alternative to ESBs that provide a similar functionality—a mechanism for interservice communications—more simply and at lower cost. They’re well-suited for use in the microservices architectures that have become more prevalent as ESBs have fallen out of favor.

## Message broker use cases

Implementing message brokers can address a wide variety of business needs across industries and within diverse enterprise computing environments. They’re useful whenever and wherever reliable inter-application communication and assured message delivery are required.

Message brokers are often employed in the following ways:

-   **Financial transactions and payment processing:** It’s critical to be certain that payments are sent once and once only. Using a message broker to handle these transactions’ data offers assurance that payment information will neither be lost nor accidentally duplicated, provides proof of receipt, and allows systems to communicate reliably even when intermediary networks are down.
-   **E-commerce order processing and fulfillment:** If you’re doing business online, the strength of your brand’s reputation depends on the reliability of your website and e-commerce platform. Message brokers’ ability to enhance fault tolerance and guarantee that messages are consumed once and once only makes them a natural choice to use when processing online orders.
-   **Protecting highly sensitive data at rest and in transit:** If your industry is highly regulated or your business confronts significant security risks, choose a messaging solution with end-to-end encryption capabilities.

## Related solutions

IBM MQ

IBM MQ offers enterprise-grade messaging capabilities that skillfully and safely move information between applications.

Explore IBM MQ

IBM Cloud Pak® for Integration

Connect applications, services and data with IBM Cloud Pak® for Integration, the most comprehensive integration platform on the market.

Explore IBM Cloud Pak® for Integration

## Resources

What is a message queue?

A message queue is a component of messaging middleware solutions that enables independent applications and services to exchange information.

What is middleware?

Middleware speeds development of distributed applications by simplifying connectivity between applications, application components and back-end data sources.

What is iPaaS (intergration-Platform-as-a-Service)?

iPaaS is a cloud-based solution that standardizes and simplifies integration across on-premises and cloud environments.