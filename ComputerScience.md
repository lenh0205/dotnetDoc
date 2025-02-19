

## 7. Stack - Heap
* **`Stack`** use static allocation at compile time -> for data have fixed-size
* **`Heap`** use for dynamic allocation at runtime -> for data that we don't know size at compile time, depend on data size at runtime

## 8. Graceful Shutdown
* -> is the process of stopping a service in a controlled manner, ensuring that **`ongoing tasks (active requests or jobs) are completed correctly`** and **`resources are released appropriately`** (close network connections properly)
* _`forceful shutdown` can interrupt running tasks and cause data loss, incomplete transactions_

* => Data Consistency: It ensures that no data is lost or left in an inconsistent state due to abrupt termination of the service.
* => User Experience: Users connected to the service at the time of shutdown do not experience unexpected errors.
* => Resource Management: Resources like file handles, memory, and network connections are properly released.
* => System Reliability: A well-handled shutdown improves the reliability and availability of the system

###  Important in Distributed Systems and microservices
* -> these system often manage critical and long-running processes, involve numerous dependencies, and maintain persistent connections to other services or clients
* -> and multiple services work together to complete a task, a forceful shutdown of one service can disrupt the entire system

### Target
* _in a microservices architecture, each service must be designed to **`handle graceful shutdowns independently while coordinating with other services`**_
* -> **`Stop Accepting New Requests`**: The service should stop accepting new incoming traffic but continue processing ongoing requests.
* -> **`Finish Processing Active Requests`**: Complete all current tasks, including transactions, database writes, or communications with other services.
* -> **`Notify Other Services`**: Inform other dependent services that the shutdown is happening so they can adjust accordingly, such as routing requests elsewhere.
* -> **`Release Resources`**: Properly release resources like file handles, database connections, and network connections

### Critical Steps for Implementing a Graceful Shutdown

#### Step 1: Catch Shutdown Signals:
Most operating systems send shutdown signals (e.g., SIGTERM or SIGINT) to running services. It’s important to catch these signals in your application and trigger the shutdown process.
#### Step 2: Stop Accepting New Requests:
Once the shutdown signal is received, the service should stop accepting new incoming requests, either by un-registering from the load balancer or by closing the network listener.
#### Step 3: Complete Ongoing Requests:
Allow all in-flight requests to complete gracefully. This involves ensuring that background tasks, such as database transactions or file I/O operations, are also completed.
#### Step 4: Graceful Connection Termination:
Shut down open connections gracefully, ensuring that no abrupt disconnections occur.
#### Step 5: Release Resources:
Close any open files, network sockets, or database connections to ensure there are no memory leaks or orphaned resources.
#### Step 6: Notify Other Systems:
In a microservices environment, it’s important to inform upstream and downstream services about the shutdown. This ensures that dependent services can handle the shutdown without disruption.

### Common techniques and pattern for Graceful Shutdown
* -> **`Draining Connections`**: When a service is about to shut down, it stops accepting new requests but continues processing existing requests until completion. This is known as connection draining.
* -> **`Timeout-Based Shutdown`**: Set a timeout period for the service to complete in-flight requests. After this timeout, the service forces the shutdown to prevent indefinite wait times.
* -> **`Backoff Mechanism`**: Use exponential backoff to gradually reduce the service’s availability to new requests, giving upstream services time to reroute traffic.
* -> **`Queue-Based Processing`**: For services that process messages from a queue, stop consuming messages from the queue while processing those already taken. This avoids leaving partially processed messages

### Graceful Shutdown in Common Frameworks
* -> **`Kubernetes`**: Kubernetes uses preStop hooks and termination grace periods to ensure that pods in a microservice architecture shut down gracefully. Kubernetes sends a SIGTERM signal, and the service is given a configurable grace period to complete existing requests.
* -> **`Node.js`**: In Node.js, a graceful shutdown can be implemented by capturing SIGTERM or SIGINT signals, stopping the HTTP server from accepting new connections, and finishing ongoing requests
* -> **`Spring Boot`**: Spring Boot provides built-in support for graceful shutdown through configuration properties. Once enabled, it ensures that the application waits for active requests to complete before terminating.

### Best Practices
* -> **`Set Reasonable Timeouts`**: Define reasonable timeouts for completing active requests and shutting down services. Infinite waits can cause other issues, like resource exhaustion.
* -> **`Unregister from Load Balancer`**: Ensure that the service is unregistered from the load balancer as soon as it stops accepting new requests. This prevents new traffic from reaching the shutting-down service.
* -> **`Monitor the Shutdown Process`**: Use logging and monitoring tools to track how gracefully the shutdown process is executed, identifying potential bottlenecks or failures.
* -> **`Test Regularly`**: Regularly test the graceful shutdown process in staging environments to catch any edge cases and validate that the system behaves as expected during real-world shutdowns.
* -> **`Manage Dependencies`**: Ensure services are aware of dependencies, both upstream and downstream, and shut down in the correct sequence to avoid cascading failures.

### Testing Graceful Shutdown
* _involves simulating real-world shutdowns, such as during deployments, scaling, or hardware failures:_
* -> Ongoing requests are completed successfully.
* -> Resource leaks or orphaned connections do not occur.
* -> Dependencies are properly managed during shutdown.
* -> Regular tests should be conducted in staging environments to refine the shutdown strategy and ensure that edge cases are addressed
