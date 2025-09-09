# Bulkhead

- **In ships**, a bulkhead is a wall that divides the ship into separate compartments. If one compartment floods, the other compartments are still safe. This prevents the entire ship from sinking.
- **In the context of Resilience4j**, a bulkhead is a resilience pattern used to limit the number of concurrent calls to a specific component or service to prevent resource exhaustion and maintain system stability.

- **Bulkhead** = isolate resources so one failure doesn’t affect all.

### Use Cases

- Limiting API calls to an external service to avoid exceeding rate limits.
- Protecting a database from too many concurrent queries.
- Isolating a slow or unreliable microservice in a distributed system.
