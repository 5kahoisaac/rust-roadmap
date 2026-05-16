# 🦀 Rust Async TCP Key-Value Micro-Engine

A high-performance, asynchronous, distributed in-memory key-value store built from scratch in Rust. This infrastructure project serves as a practical milestone to master system memory ownership boundaries, deterministic thread safety, and zero-overhead non-blocking I/O without relying on a runtime Garbage Collector (GC).

---

## ⚡ Architectural Paradigm Shift: Go vs. Rust

Coming from a Go backend architecture (`go func()` and channel-based routines), this project intentionally shifts paradigms to achieve compile-time guaranteed correctness and predictable tail latencies:

* **Go Architecture:** Relies on an automated runtime multiplexer (the M:N scheduler) and an implicit background Garbage Collector. While fast to write, periodic GC tracing cycles introduce micro-pauses and memory spikes under intense network throughput.
* **Rust Architecture:** Eliminates the garbage collector completely. Memory allocations are deterministic. Thread access boundaries are validated entirely at compile-time using explicit types like `Arc` (Atomically Reference Counted pointer) for thread-safe shared ownership and `Mutex` (Mutual Exclusion) for memory mutation.



---

## 🛠️ Core Technology Stack

* **Language:** Rust (Stable Toolchain)
* **Runtime Engine:** `tokio` (Multi-threaded, event-driven asynchronous I/O framework)
* **Data Serialization:** `serde` (Framework for generic data serialization/deserialization)
* **Protocol Encoding:** `bincode` (Compact, high-efficiency binary serialization protocol)
* **Network Protocol:** Raw TCP Sockets (`tokio::net::TcpListener` / `TcpStream`)

---

## 🚀 Key Functional Requirements

1.  **Zero-Copy Binary Protocol:** Commands and payloads are packed into compact binary frames rather than heavy JSON string text arrays, cutting down networking overhead.
2.  **Thread-Safe Cache Matrix:** In-memory state is coordinated across parallel asynchronous worker tasks safely using standard `Arc<Mutex<HashMap<String, Vec<u8>>>>` abstractions.
3.  **Non-Blocking Event Loops:** The network core relies on discrete async tasks utilizing multi-producer, single-consumer channels (`tokio::sync::mpsc`) to isolate state mutation from concurrent I/O readers.

---

## 💻 Local Workspace Installation

### 1. Prerequisites
Ensure you have the official Rust toolchain manager installed on your workstation:
```bash
curl --proto '=https' --tlsv1.2 -sSf [https://sh.rustup.rs](https://sh.rustup.rs) | sh
