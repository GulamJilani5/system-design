🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Logging

Logging refers to the recording of discrete events that occur in software applications over time, typically with a timestamp and contextual information like which process or user was involved. Logs are crucial for troubleshooting and debugging, helping developers reconstruct exactly what happened in an application at any given moment.

### ➡️ What Is Logging?

Logging involves systematically capturing event records (logs) during program execution.

##### 🟦 Each log usually includes:

- Timestamp marking when the event happened.
- Details about the event’s context (what action, which user/request, etc.).
- Severity level such as trace, debug, info, warn, or error.

##### 🟦 Logs help answer questions like:

- "What happened at this time?"
- "Which thread or user was involved?"

### ➡️ Logs are vital for:

- **Troubleshooting:** Identifying and diagnosing bugs and unexpected behavior.
- **Debugging:** Reconstructing the application’s state at a specific time.
- **Monitoring:** Observing system health and performance.
- **Audit trails:** Verifying user actions for compliance or security.

### ➡️ Logging in Monolithic Apps

- All code exists in a single codebase.
- Logs are stored together in one location.
- This simplifies problem-solving, as developers only need to look in one place for relevant logs.

### ➡️ Logging in Microservices

- Each service has its own individual logs.
- Troubleshooting is complex because logs are spread across multiple sources.
- Centralized logging tools aggregate logs from all services, consolidating them for easier access and problem resolution.

### ➡️ Key Takeaways

- Logs are essential for tracking and understanding the behavior of software over time.
- In monolithic apps, logs are easier to find because they’re in one spot.
- In microservices architectures, centralized logging tools are recommended to make troubleshooting manageable.
