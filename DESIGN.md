# DESIGN.md

## API Efficiency Benchmark Challenge

### Overview

This repository hosts a **collaborative engineering challenge** focused on designing efficient API services.

Participants compete by implementing a **data population pipeline and an API server** that expose the same contract but differ in **architecture, performance, and efficiency**.

The challenge is intentionally structured so that solutions are **merged into the main repository**, allowing a shared benchmark and a continuously updated leaderboard.

The goal is to create a **public GitHub benchmark of API implementations**, where each solution becomes a **technical portfolio artifact**.

---

# Core Idea

Each participant implements their solution inside a dedicated folder under:

```
/solutions/<solution-name>/
```

Each solution must include:

1. **A database population script**
2. **An API server reading from that database**
3. **A Dockerized environment**

The repository includes:

* a **common dataset**
* a **fixed API specification**
* a **benchmark runner**
* a **GitHub Actions workflow** that builds and evaluates every solution

When a pull request is merged, the CI pipeline automatically:

1. builds the Docker image
2. populates the database
3. starts the API server
4. executes benchmark queries
5. measures performance metrics
6. updates the ranking

---

# Challenge Flow

Each solution must support the following execution flow:

```
1. populate database
2. start API server
3. benchmark API calls
4. collect metrics
```

The CI system will execute:

```
populate → start-server → benchmark
```

---

# Repository Structure

```
api-efficiency-benchmark/

data/
    users.json
    transactions.json
    products.json

api-spec/
    openapi.yaml

benchmark/
    requests.json
    runner.py

solutions/
    example-solution/
        Dockerfile
        populate.sh
        server.sh
        src/

.github/workflows/
    ranking.yaml

README.md
DESIGN.md
```

---

# Dataset

The repository contains structured data used to populate the database.

Example:

```
data/
    users.json
    products.json
    transactions.json
```

Participants are free to:

* choose any database
* design indexes
* transform data
* precompute aggregates

The **populate script** is responsible for preparing the database.

---

# Required Components

Each solution must implement two components.

---

## 1. Database Population Script

Responsible for:

* reading data from `/data`
* transforming and loading data
* creating schema
* building indexes

Example execution:

```
./populate.sh
```

This script must leave the database ready for queries.

Participants may use:

* PostgreSQL
* SQLite
* Redis
* DuckDB
* custom engines
* embedded databases

The choice of database is part of the optimization challenge.

---

## 2. API Server

After the population phase, a server must start.

Example execution:

```
./server.sh
```

The server must expose the API defined in:

```
/api-spec/openapi.yaml
```

Example endpoint:

```
GET /report/top-users
```

Expected output example:

```
[
  {
    "user_id": 12,
    "total_spent": 1432.50,
    "favorite_category": "electronics"
  }
]
```

The API must run on:

```
localhost:8080
```

---

# Docker Requirement

Each solution must be fully containerized.

Required file:

```
Dockerfile
```

The container must support:

```
docker build
docker run
```

The CI system will execute the solution inside Docker.

---

# Benchmark Execution

The benchmark runner will:

1. start the container
2. populate the database
3. start the API server
4. send predefined API requests

Example request set:

```
benchmark/requests.json
```

Requests may include:

```
GET /users/{id}
GET /report/top-users
GET /products/top
GET /transactions/recent
```

---

# Performance Metrics

The CI pipeline evaluates each solution based on:

### 1. Execution Time

Total time required to execute benchmark requests.

### 2. Memory Usage

Peak RAM usage during execution.

### 3. Docker Image Size

Size of the final Docker image.

### 4. API Latency

Average latency across requests.

---

# Ranking

Results are aggregated into a leaderboard.

Example:

```
🏆 Leaderboard

1. rust-stream       120MB RAM   2.9s   18MB image
2. go-pipeline       140MB RAM   3.4s   22MB image
3. python-async      210MB RAM   4.1s   95MB image
```

The leaderboard will be automatically updated in:

```
README.md
```

---

# GitHub Workflow

A GitHub Action named:

```
ranking.yaml
```

will run a matrix job across all solutions:

```
solutions/*
```

Workflow steps:

```
build docker image
run populate script
start API server
execute benchmark
measure metrics
update ranking
```

---

# Contribution Model

Participants do **not create independent forks** for the competition.

Instead:

1. fork the repository
2. implement a solution under `/solutions/<name>`
3. open a pull request
4. once merged, the CI runs the benchmark

Only merged solutions appear in the leaderboard.

This ensures the repository becomes a **shared benchmark archive**.

---

# Design Goals

This challenge emphasizes **engineering quality**, not just code generation.

Key evaluation aspects:

* efficient data modeling
* indexing strategy
* memory management
* streaming vs in-memory processing
* caching
* container optimization
* API design

---

# Expected Outcomes

The project aims to produce:

* a public **API engineering benchmark**
* a collection of high-quality reference implementations
* a repository that serves as a **technical portfolio for participants**

Each solution becomes a **demonstration of real-world backend engineering skills**.

---

# Summary

Participants must implement:

```
1. populate database script
2. API server
3. Docker container
```

The CI system will automatically evaluate:

```
performance
memory usage
image size
API latency
```

and generate a **public ranking of solutions**.

This creates a collaborative benchmark showcasing different approaches to building efficient API services.
