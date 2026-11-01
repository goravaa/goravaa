# Gorav

**Backend & AI Infrastructure Engineer**  
*Data Pipelines · Object Storage · GPU Tooling · Cloud Provisioning · Backend*

I started writing C and Python inside Termux on an Android phone at 13, and rented a laptop at 16 to take on my first freelance work. Today, I work on the infrastructure behind data and AI products: moving and indexing large datasets, testing object storage systems, building GPU data pipelines, backend services and automating cloud setups with Terraform, Golang, and Python.

---

### Open-Source Projects

#### [Sab](https://github.com/goravaa/Sab) - Self-hosted RAG Index over S3, served via MCP (`v0.1.0`)
*Python · DuckDB · Lance · S3 · PostgreSQL · vLLM/Ollama · Docker* · **[Demo Video](https://youtu.be/g98wnnadK9I)**
A two-service system (`dataset-engine` + `query-service`) that reads PostgreSQL tables and S3 files (CSV, XLSX, PDF, DOCX, Markdown), creates embeddings, and writes a versioned Lance dataset back to your own S3 bucket. Exposes 9 read-only MCP tools over HTTP.
- **Commit isolation:** `query-service` only reads committed manifests and job receipts, so in-flight indexing jobs don't leak into query answers.
- **Bounded memory:** Runs read-only `SELECT` queries via DuckDB over streamed Lance batches, indexing the manifest in SQLite on temp disk with a 512 MB default memory cap and spill-to-disk.
- **Engine-side aggregations:** Computes exact `count`/`sum`/`avg`/`min`/`max` with `group-by` and dataset integrity audits (`audit_integrity`) inside the engine rather than relying on the LLM.

#### [Chotu](https://github.com/goravaa/Chotu) - Disposable Private GPU Coding Box on AWS (`v0.1.0`)
*Go · Terraform · AWS EC2 (g5/g6 Spot) · vLLM (Qwen3-Coder-30B) · S3/rclone · systemd* · **[Demo Video](https://youtu.be/XPaYZT3ZX5Q)**
One `terraform apply` spins up a spot GPU instance running `vLLM` (`Qwen3-Coder-30B`) wired to OpenCode, reachable only over SSH with no public ports. `terraform destroy` tears down compute while keeping the user's S3 workspace bucket safe (`prevent_destroy`).
- **Go setup & status daemons:** Wrote `chotu-setup` (handles NVIDIA driver reboots, Docker, model pull, and `rclone` workspace sync) and `status-service` (streams setup progress and health over SSE).

#### [apache-polaris-cli](https://github.com/goravaa/apache-polaris-cli) - Single-Binary Go CLI for Apache Polaris
*Go · Apache Iceberg · Apache Polaris · OAuth2*
A ~7 MB static Go binary for managing Apache Polaris (Iceberg) catalogs, namespaces, tables, and RBAC principals without needing a JVM or Python runtime.
- Includes `polaris wait` (readiness poller for CI/CD pipelines) and `polaris seed` (declarative YAML/JSON bootstrapper with dry-run and credential capture), tested against live Polaris 1.2.0 containers.

#### [gemma-embedder-rust](https://github.com/goravaa/gemma-embedder-rust) - Embedding Microservice
*Rust · ONNX Runtime · Axum (HTTP) · Tonic (gRPC) · CUDA · Docker*
Rust service serving `EmbeddingGemma-300M` over HTTP and gRPC with dynamic request batching (configurable batch size and wait window) and separate CPU/GPU Docker images.

---

### Contract & Lab Work (Closed-Source)

A summary of infrastructure problems I have worked on in freelance and lab roles:

- **Proving GPU storage bottlenecks & Python↔Go FFI MVP:** Built a reproducible Python training harness for `nanoGPT` (Dolphin dataset) on AWS GPU instances to collect evidence that storage I/O was starving training (logs showed the GPU sitting idle ~90% of the time waiting on data). To address this, I designed and built a Python↔Go FFI MVP that streams tokenized Parquet data from a Go storage SDK into Python workers via a C data interface without extra memory copies, passing 134 `fsspec` network tests.
- **Storage cluster provisioning backend:** Built a cluster-provisioning backend in Go and PostgreSQL (with a Next.js UI) using a stateful job queue and lifecycle tracking, focused on handling partial provisioning failures, timeouts, and worst-case rollback states cleanly.
- **10 TB object storage benchmarking:** Benchmarked a custom S3-compatible object store against AWS S3 Standard and S3 Express using a 10 TB TPC-DS dataset with Spark and ClickHouse across 20+ automated infra configs. Tracked down and fixed a server-side pagination bug in Go within 4 hours that was blocking the run.
- **200 GB+ (147M docs) MongoDB migration:** Built a Python pipeline on GCP VMs (over VPN) that extracted 147 million MongoDB documents into Parquet for loading into BigQuery and ClickHouse.
- **Applied ML & video pipelines:** Built gRPC microservices in Python using ONNX (ArcFace/Blaze) and YOLO11 for real-time video stream detection, and trained XGBoost, LightGBM, and Transformers on 134M-row tabular datasets on Vast.ai GPUs.

---

### How I Work

- **AI-assisted, failure-tested:** I use coding agents heavily for implementation speed, and spend my own time on the architecture, business logic, and trying to break the system testing partial failures, restart recovery, memory limits, and worst-case states before shipping.
- **Systems & Operational Automation:** I build systems that make complex, repetitive workflows reliable and repeatable, with reproducible deployments, automated black-box verification, versioned releases, and documented limitations.
- **High visibility:** I turn rough problems or quick calls into concrete written plans, reproducible scripts, and documented PRs so progress is easy to track.

---

### Core Stack

- **Languages:** Python, Go, Rust, SQL, Bash, Dart/Flutter, TypeScript
- **Data & Storage:** S3 / S3 Express / MinIO, PostgreSQL / Aurora, ClickHouse, BigQuery, DuckDB, Lance, Apache Iceberg (Polaris), Parquet, MongoDB
- **AI & Inference:** vLLM, ONNX Runtime, RAG / MCP, Ollama, PyTorch, XGBoost/LightGBM
- **Infra:** AWS (EC2 GPU, S3, IAM), GCP, Vast.ai, Terraform, Docker / Compose, systemd, GitHub Actions

**Contact:** [gorav.works@gmail.com](mailto:gorav.works@gmail.com)
