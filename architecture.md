# Subscription Engine Architecture

## Overview

This project aims to deepen my understanding of how a concurrent billing system operates and to provide hands-on experience in designing and implementing scalable microservices using Spring Boot. It is part of my broader effort to build production-grade backend systems while applying principles of clean architecture, concurrency, distributed systems, and cloud-native deployment.

## Components
**1. User Entry Point**
- AWS API Gateway: Receives `/pay` requests from external clients.
- AWS Application Load Balancer (ALB): Distributes traffic to ECS tasks

**2. Payment Service**
- Responsibilities:
    - One-time and recurring payment processing
    - Integration with Stripe, PayPal, etc.
    - Triggering mail notifications
    - Caching and rate-limiting responses

- Infrastructure:
    - ECS: Hosts microservices
    - ECR: Container registry 
    - CloudWatch: Metrics and logging
    - Redis: For rate limiting and caching
    - PostgreSQL(RDS): Metadata storage
    - Mailer Service: Sends email notifications

**3. Subscription Engine**
- Responsibilities:
    - Manages subscription lifecycles
    - Triggers payment operations
    - Handles scheduling and retries
    - Kafka-based communication for decoupling

- Infrastructure:
    - ECS/ECR: Containerized microservices
    - CloudWatch: Monitoring
    - Redis: Lightweight throttling and caching
    - Kafka: Pub/Sub for payment and notification events

**4. Database Layer**
- RDS(PostgreSQL):
    - Used in both Payment and subscription engines
    - Horizontal scalability using read replicas
    - Write operations go to primary, reads from replicas
    - Data separation as needed between services

**5. Security & Observability**
- IAM Roles * Policies: Secure service communication
- VPCs and Subnets: Enforce network isolation
- CloudWatch Alarms: Failure detection and alerting
- TLS Everywhere: Encrypted communication between components

## Scalability Strategy
- Stateless ECS services enable horizontal auto-scaling 
- Redis caching minimizes DB load
- Kafka decouples services and supports asnyc processing 
- Rate limiting defends against abuse and ensures fair usage

## High Availability
- Multi-AZ RDS deployment
- Load-balanced ECS Services
- Retry and circuit-breaker patterns for external services

## Architecture Diagram

