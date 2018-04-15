# REST Architectural Style

Author: [Wen Xin](https://github.com/wenmogu)

## Definition:
Representational State Transfer (REST) is the architecture style of the World Wide Web we are using today.

## Why learn REST: 
It is the set of guiding principles of the development of the Web (i.e. how the Web should be developed such that it can be efficient and widely applicable). It also serves as the model for Web protocols e.g. HTTP and the guiding principles for RESTful API. Hence, understanding REST will give us an idea of the rationale behind the design of the Web and let us have a better understanding of the key properties of RESTful API, which would come in handy when we need to use other people’s RESTful APIs and implement our own RESTful APIs. 

## <a name="meaningOfRest"></a>Meaning of REST:
REST stands for “Representational State Transfer”. In this section, I will explain the meaning of this name.
Before we understand the term “representational state”, we have to understand the term “resource”. In the context of REST, the term “resource” is used as the way to organize and represent data/information. Roy Fielding -- the originator of REST, states in his paper that “any information that can be named can be a resource”, and the identification of resources is done by using unique identifiers of the resources i.e. unique name of the resources. The information represented by one resource can change at any time, but the name of the resource i.e. the identifier of the resource has to remain the same for identification purposes. For example, the student list of the course CS3281 can be named as “CS3281StudentList”, and hence can be a resource. The student list might change over time, e.g. from those who enrolled in Year 2017 to those who enrolled in Year 2018, but the identifier “CS3281StudentList” always refers to the student list regardless of its state/value. 

As the combination of various types (video, audio, hyperlinks, text…) of information into one unit are becoming more common (e.g. in an online article teaching the reader how to use IntelliJ, there would be text describing the process, videos giving demonstrations and hyperlinks directing the reader to other articles), using an encompassing term to represent a unit of information makes sense as that’s how the user organizes information. Moreover, this abstraction saves us the trouble of distinguishing the types or implementation of information inside this unit when thinking of the process of accessing and transmitting this unit of information.

Resource and the term “representational state” are closely related to the client-server model outlined as one of the rules of REST. Resource is the information stored at the server, whereas what the clients see when they access the resource are actually the “representations” of the resource at a specific point of time, i.e. the “representational state” of the resource. Giving the users the representation of the resource instead of the resources itself allows [dynamic binding](https://en.wikipedia.org/wiki/Late_binding) of the reference to a representation of the resource, and this enables the users to have access to and operate on this resource in the formats they want, such as JSON, HTML, XML etc. When the user wishes to make some changes to the resource, he/she can change the representation of this resource and send the representation back to server to update the resource. As such, the server is freed from managing different [application states](https://ruben.verborgh.org/phd/hypermedia/#the-statelessness-constraint-p-8) across requests. 

The quote below from Roy Fielding outlines what the name entails:
> The name “Representational State Transfer” is intended to evoke an image of how a well-designed Web application behaves: a network of web pages (a virtual state-machine), where the user progresses through the application by selecting links (state transitions), resulting in the next page (representing the next state of the application) being transferred to the user and rendered for their use.

## Why REST exists
REST specifies the key designs of the Web. The design of the Web affects greatly how efficient, applicable and modifiable the Web can be. Hence, before understanding the REST principles, we need to understand the requirements of the Web i.e. what the Web is expected to be. 

The Web is expected to be a place for convenient storing and sharing of information by its users. Hence, to make the ideal Web, we need to consider the properties of its users and their information. The target audience of the Web are people in different organizations all over the world connected via the Internet. Their organizations would have different requirements for their information (e.g. the authentication for access of information), and the machines they use to access the Web might be very different, with different operating systems and requesting different file formats. These people’s information also has a large variety in terms of the content type and the format. Hence, the Web needs to have the following properties:
 * the Web should have a simple, uniform interface to present various types of information (e.g. video, audio, graphics, text etc.) 
 * the Web should be efficient in transmitting information 
 * the Web should be able to evolve
 * the Web should not have a central entity to control the whole system, which means there is no control over the amount and the content of information outside the boundary of the organizations using the Web. Hence, 1) the Web elements should be able to cope with unexpected load 2) the Web should be able to communicate authentication data and authorization controls to sieve out the malicious information
 * the Web should allow its element to undergo incremental changes for evolution 

REST is the abstraction of the design that satisfies the above requirements.

## Details of REST

REST is an architectural style. Fielding defined architectural style as “a coordinated set of architectural constraints that restricts the roles/features of architectural elements and the allowed relationships among those elements within any architecture that conforms to that style”. 

After surveying some common architectures for network-based applications on how the architectural constraints induce their corresponding architectural properties, Fielding came up with REST. REST has 6 constraints which aim to induce the properties the Web should have. 

### <a name="clientServer"></a>Client-Server
A system in REST style should separate the user interface concern from the data storage concern. As such, the server is freed from managing the user interface and the user interface is detached from the server. This separation of concern allows the server to evolve without impacting the user interface and makes upgrading the server easier. It also enables the system to have a uniform interface regardless of the different structures of data storage at the servers.

### <a name="stateless"></a>Stateless
A system in REST style should have stateless communications. This means that all the messages must be self-sufficient, containing all the necessary information to understand the message without referring to information outside the message. By making all the messages self-sufficient, the workload of the server is reduced as the server does not have to keep track of the state of application at the clients. The coupling between the server and the client is further reduced by the statelessness of the system, and thus the server can be scaled up and down according to the amount of workload (e.g. the number of requests from the client at certain point of time). Moreover, by eliminating cross referencing to other messages when interpreting one, this reduces the chances of error and increases reliability of the system. The statelessness of the system also allows the system to make use of intermediaries as the intermediaries in between the client and the server have all the necessary information to complete their tasks (see [Layered System](#layeredSystem)).  

### <a name="cache"></a>Cache
Data within the messages for communications between the server and the client should be indicated if it is cacheable. If cacheable, the caches at the elements along the line of communication (e.g. the client cache, the server cache, the proxy cache etc.) will store the data and reuse if for identical requests later. The cacheability of information reduces the amount of interactions needed between the client and the server to access information, and thus improves the efficiency of the system. The average latency of interactions is also reduced, which leads to faster response to the client and an improved user-perceived performance.

### <a name="layeredSystem"></a>Layered System
There should be multiple layers between client and server. The layers are made up by various [intermediaries](https://www.techopedia.com/definition/24378/web-intermediary-wbi) between the client and the server facilitating the processing tasks. Some examples of intermediaries are load-balancer, cache etc. The system elements have no knowledge of the things outside their own layers. For example, a client would not know if it is connected directly to the server or to an intermediary, and an intermediary would not know if it is connected to another intermediary. By limiting the scope of the system element, the complexity of the system is greatly reduced. The system becomes more modifiable as there are less dependencies between the system elements. By facilitating the processing tasks, intermediaries can enhance the server performance by reducing the redundant server processing. Moreover, as the intermediaries can carry out a wide range of tasks including [encryption](https://en.wikipedia.org/wiki/Encryption) and request modification and etc.(see "MEGs" in [this article](http://www.almaden.ibm.com/cs/wbi/doc/Architecture.html)), the organizations can use intermediaries to enhance their security and their control over the information. 

### <a name="uniformInterface"></a>Uniform Interface
There should be a way for the server, the client and the intermediaries in the layers in between to communicate with each other. Hence, there should be a uniform interface in the system. The existence of the uniform interface is the foundation for the other 4 architectural constraints. Each component is encapsulated by the interface and hence become more independent of each other, allowing each to evolve independent of the rest. By having a uniform interface in the system, interactions between the layers can be monitored as the set of interactions are predefined. By allowing the interactions to be inspected by mediators (e.g., network firewalls), the security of the system is enhanced. However, the existence of the uniform interface might compromise the efficiency of the system as the information is transmitted in a standard format rather than catering to each component’s needs.

There are four sub-constraints which further specify the Uniform Interface constraint.
 * Identification of resources: as mentioned before (see [Meaning Of REST](#meaningOfRest)), “resource” is an organization of information and the identifiers of the resources need to remain constant. An example to illustrate this constraint is [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). 
 * Manipulation of resources through representations: as mentioned before (see [Meaning Of REST](#meaningOfRest)), the “representation” of the resource at one point i.e. the representation of the state of the resource, is what the users see and modify, instead of the resource itself. 
 * Self-descriptive messages: A message in a communication between the web components should contain all the information needed for the web components to understand its content. 
 * Hypermedia as the engine of application state (HATEOAS): There should be hyperlinks embedded inside the representations given to the client, such that all the future actions that the client might take are within these representations. Hence, the client can interact with and navigate through the application without any prior knowledge of how to do so. Hence, the client and the server are more independent of each other. 

### <a name="codeOnDemand"></a>Code On Demand (Optional)
The server can send a code snippet to the client to let the client execute. One example of this is the Javascript code sent along with the webpage in HTML. This constraint extends the client functionality and reduces the workload of the server by reducing the number of features to be implemented at the server. However, it reduces the visibility of the interactions between the client and the server and makes it more difficult to monitor the interactions. Hence, this constraint might be disabled in some implementations in REST style.

## Useful Resources
* [Roy Fielding's paper which gave birth to REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf)
* [History of hypermedia and REST explained](https://ruben.verborgh.org/phd/hypermedia/)
* Web intermediaries
	* [Web intermediaries explained](http://www.almaden.ibm.com/cs/wbi/doc/Architecture.html)
	* an important type of intermediaries: [proxy](https://en.wikipedia.org/wiki/Proxy_server#Filtering_of_encrypted_data)
* Hypermedia as the engine of application state (HATEOAS)
	* [A general wikipedia explanation](https://en.wikipedia.org/wiki/HATEOAS)
	* [A more detailed explanation](https://ruben.verborgh.org/phd/hypermedia/#hypermedia-as-the-engine-of-application-state)
* Code On Demand
	* [a general wikipedia explanation](https://en.wikipedia.org/wiki/Code_on_demand)
	* [a stackoverflow explanation](https://stackoverflow.com/questions/32094952/code-demand-constraint-for-restful-apis?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
* [Video explaning REST (easy to get the big picture but not very precise)](https://www.youtube.com/watch?v=YCcAE2SCQ6k)