from pathlib import Path

readme = r'''# RideFlow 🚗☁️

### A Multi-Cloud, Cloud-Native Microservices Platform for Real-Time Ride-Hailing

RideFlow is a cloud-native ride-hailing platform designed around **microservices, Kubernetes, multi-cloud infrastructure, event-driven communication, GitOps, and observability**.

The system uses **AWS as the primary application cloud** and **Google Cloud Platform (GCP) for real-time analytics and stream processing**. The architecture is designed to provide scalability, fault isolation, secure networking, automated deployments, and a clear separation between transactional workloads and analytical workloads.

---

## 📐 System Architecture

The following diagram represents the current RideFlow system architecture and the relationship between the application, infrastructure, data, CI/CD, and analytics layers.

![RideFlow Architecture](docs/RideFlow_Multicloud_Architecture.png)

> **High Availability:** The AWS workload is designed to span three Availability Zones in `ap-south-1` (`ap-south-1a`, `ap-south-1b`, `ap-south-1c`).

---

## 🎯 Problem Statement

Traditional ride-hailing applications can become difficult to scale and maintain when user management, driver operations, trip processing, pricing, databases, analytics, and deployment workflows are tightly coupled.

RideFlow addresses this by decomposing the platform into independently deployable services and placing each workload on infrastructure appropriate to its requirements.

### Key goals

- Build a **microservices-based ride-hailing platform**
- Run application workloads on **managed Kubernetes**
- Support **horizontal scaling using Kubernetes HPA**
- Separate public and private network resources
- Use **event-driven communication** for asynchronous workloads
- Automate application delivery using **GitHub Actions + Argo CD**
- Use managed cloud databases for transactional workloads
- Process ride events and analytics using **Kafka + Apache Spark**
- Demonstrate a **multi-cloud architecture using AWS + GCP**
- Provide centralized **metrics and logs** for operational visibility

---

## 🏗️ Architecture Overview

RideFlow is divided into five major layers:

```text
                    ┌─────────────────────┐
                    │      Developers     │
                    └──────────┬──────────┘
                               │
                         Git Push / PR
                               │
                    ┌──────────▼──────────┐
                    │      GitHub         │
                    │  Source Repository  │
                    └──────────┬──────────┘
                               │
                     GitHub Actions (CI)
                               │
                    ┌──────────▼──────────┐
                    │    Amazon ECR       │
                    │   Docker Images     │
                    └──────────┬──────────┘
                               │
                           GitOps
                               │
                    ┌──────────▼──────────┐
                    │       Argo CD       │
                    │   Continuous CD     │
                    └──────────┬──────────┘
                               │
                 ┌─────────────▼─────────────┐
                 │        AWS EKS            │
                 │    Kubernetes Cluster     │
                 │                           │
                 │ Rider | Driver | Trip     │
                 │        | Pricing          │
                 └─────────────┬─────────────┘
                               │
              ┌────────────────┼─────────────────┐
              │                │                 │
        PostgreSQL         DynamoDB              S3
           (RDS)          (NoSQL Data)       (Objects/Data)
              │                │                 │
              └────────────────┼────────────────┘
                               │
                          Event Stream
                               │
                           Kafka
                               │
                     ┌─────────▼─────────┐
                     │       GCP         │
                     │     Dataproc      │
                     │   Apache Spark    │
                     └─────────┬─────────┘
                               │
                       Analytics Results
