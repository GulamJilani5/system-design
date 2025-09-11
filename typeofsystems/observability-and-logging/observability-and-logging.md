🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Observability and Monitoring

### ➡️Three Pillar

##### 🟦 Metrics

- Numeric time-series (throughput, latency, error rates, JVM memory, CPU).
- Good for dashboards and alerting.

##### 🟦 Logs

- structured event records (request errors, stack traces). Useful for forensic/debugging.

##### 🟦 Traces

- Distributed request traces across services (end-to-end latency, where time is spent).

### ➡️ Splunk

- **Splunk** = One platform that replaces ELK + Prometheus + Grafana + Jaeger.

### ➡️ Dynatrace

- **Dynatrace** = One managed platform that replaces Prometheus + Grafana + Jaeger + ELK + Alertmanager.

### ➡️ Observability vs. Monitoring

| Feature  | Monitoring                         | Observability                                 |
| -------- | ---------------------------------- | --------------------------------------------- |
| Purpose  | Identify and troubleshoot problems | Understand the internal state of a system     |
| Data     | Metrics, traces, and logs          | Metrics, traces, logs, and other data sources |
| Goal     | Identify problems                  | Understand how a system works                 |
| Approach | Reactive                           | Proactive                                     |

---

**In other words, monitoring is about collecting data and observability is about understanding data.**

**Monitoring is reacting to problems while observability is fixing them in real time.**
