---

A REST (Representational State Transfer) API is an architectural style for building web services. It relies on a stateless, client-server communication protocol, typically HTTP. Here are its key features:

**1. Resource-Based:**
* REST APIs treat everything as a **resource**. A resource is an object or representation of information with an associated URI (Uniform Resource Identifier).
* Examples of resources: users, products, orders, articles.
* URIs identify specific resources (e.g., `/users`, `/products/123`).

**2. Uniform Interface:**
* This is a core principle ensuring consistency and predictability. It involves several aspects:
    * **Identification of Resources:** Requests identify resources using URIs.
    * **Manipulation of Resources Through Representations:** Clients interact with resources by exchanging representations (e.g., JSON, XML). These representations contain enough information to modify or delete the resource.
    * **Self-Descriptive Messages:** Responses include metadata about how to process the representation.
    * **Hypermedia as the Engine of Application State (HATEOAS):** Responses may contain links to related resources, allowing clients to discover available actions dynamically.

**3. Stateless:**
* Each request from a client to the server contains all the information needed to understand and process the request.
* The server does not store any client state between requests. Session state is typically managed on the client (e.g., using tokens).
* This makes REST APIs scalable and reliable as the server doesn't need to maintain session information.

**4. Client-Server Separation:**
* The client and server operate independently. The client application is concerned with the user interface, while the server application manages the data and logic.
* This separation allows each to evolve independently without affecting the other, as long as the interface remains consistent.

**5. Layered System:**
* The architecture can be composed of multiple layers (e.g., client, load balancer, server, database).
* Clients don't necessarily know if they are communicating directly with the end server or an intermediary.
* This enhances scalability, security, and flexibility.

**6. Cacheability:**
* Responses can be cached by the client or intermediary servers to improve performance and reduce server load.
* Servers should indicate whether a response is cacheable and for how long.

**7. Standard HTTP Methods:**
* REST APIs leverage standard HTTP methods to perform actions on resources:
    * **GET:** Retrieve a resource.
    * **POST:** Create a new resource.
    * **PUT:** Update an existing resource or create it if it doesn't exist.
    * **PATCH:** Partially update an existing resource.
    * **DELETE:** Remove a resource.
    * **HEAD:** Retrieve metadata about a resource (like GET, but without the response body).
    * **OPTIONS:** Describe the communication options for the target resource.

**8. Standard Data Formats:**
* REST APIs commonly use lightweight data formats for representing resources, with JSON being the most popular choice due to its simplicity and ease of parsing. XML is another common format.

**9. Use of MIME Types:**
* HTTP headers include MIME types to indicate the format of the data being transferred (e.g., `application/json`, `application/xml`). This helps clients understand how to process the response.

**10. Proper HTTP Status Codes:**
* REST APIs use standard HTTP status codes to indicate the outcome of a client's request (e.g., 200 OK, 201 Created, 400 Bad Request, 404 Not Found, 500 Internal Server Error). This provides clear feedback to the client.

**Optional Features:**

* **Code on Demand (Optional):** The server can send executable code to the client to extend its functionality. This is less common due to security concerns.

**Benefits of Using REST APIs:**

* **Simplicity:** Easy to understand and implement.
* **Scalability:** Statelessness allows for easy scaling.
* **Flexibility:** Supports various data formats.
* **Platform Independence:** Clients and servers can be developed in different languages and on different platforms.
* **Wide Adoption:** A large ecosystem of tools and libraries supports REST.
* **Cacheability:** Improves performance and reduces server load.

**In summary, REST APIs provide a standardized and flexible way for applications to communicate over a network, making them a fundamental building block for modern web services and microservices architectures.**
