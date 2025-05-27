## 10 System Design Trade-offs You Must Not Ignore

This image presents a circular diagram highlighting 10 key trade-offs in system design. Each section of the circle represents a different trade-off, often contrasting two opposing approaches.

Here's a breakdown of the 10 trade-offs shown:

1.  **Relational vs. Key-Value:**
    * **Relational:** Structured relational data stores, flexible, scalable non-relational data stores. (Illustrated with tables for Products, Customers, Orders).
    * **Key-Value:** (Implied as the alternative non-relational approach).

2.  **Graph vs. Document:**
    * **Graph:** (Illustrated with nodes and edges representing relationships).
    * **Document:** (Implied as another non-relational approach, likely storing data as JSON or XML documents).

3.  **Normalization vs. Denormalization:**
    * **Normalization:** Optimize for data integrity vs. faster read performance. (Illustrated with separate tables for Customers and Customer Orders).
    * **Denormalization:** (Implied as combining data into fewer structures for faster reads).

4.  **Consistency vs. Availability:**
    * **Consistency vs. Eventual Consistency:** Immediate data accuracy vs. performance and scalability with eventual consistency. (Illustrated with Customer Service and Email Service having strongly consistent and eventually consistent replication respectively).

5.  **Strong Consistency vs. Eventual Consistency:** (Repeated from point 4, emphasizing the spectrum).

6.  **REST vs. GraphQL:**
    * **REST:** Fixed data structures vs. flexible querying with a single endpoint. (Illustrated with separate endpoints for `/posts` and `/comments`).
    * **GraphQL:** (Illustrated with a single endpoint `/graphql` allowing specific data requests).

7.  **Stateful vs. Stateless:**
    * **Stateful:** Maintain client session data on the server or a dedicated state store. (Illustrated with a Client, Application, and State component).
    * **Stateless:** Each request from the client contains all necessary information. (Illustrated with a Client and Application where each Request leads to a Response).

8.  **Sync vs. Async Processing:**
    * **Sync:** Wait for response or error before continuing, potentially blocking the system. (Illustrated with a synchronous Request and Response flow).
    * **Async:** Initiate a request and continue processing without waiting for an immediate response. (Illustrated with an asynchronous Request and a separate Response).

9.  **Read-Through vs. Write-Through Cache:**
    * **Read-Through Cache:** Cache miss loads data vs. write cache and database. (Illustrated with a flow: Request -> Cache (Miss) -> Read db -> Write Cache -> Response).
    * **Write-Through Cache:** Write data to both cache and database simultaneously. (Illustrated with a flow: Write -> Cache -> Write db).

10. **Vertical vs. Horizontal Scaling:**
    * **Vertical Scaling:** Scale up a single machine (add more resources). (Illustrated with a single server increasing in size).
    * **Horizontal Scaling:** Distribute load across multiple servers. (Illustrated with multiple servers working in parallel).

**Additional elements in the image:**

* **Batch vs. Stream:** Process data in chunks vs. handle data in near real-time flow. (Illustrated with Batch data processing having Input -> Batch -> Output, and Stream data processing having Input -> Stream -> Instant Output).
* **CAP Theorem (Consistency, Availability, Partition Tolerance):** Choose between always accurate data and always available services when network partitions occur. (Illustrated with a Venn diagram showing CA, CP, and AP).

The diagram serves as a visual reminder of the fundamental trade-offs engineers face when designing and scaling complex systems. Understanding these trade-offs is crucial for making informed decisions that align with the specific requirements and constraints of a project.
