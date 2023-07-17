---
title: Payment System
description: 支付系統
published: true
date: 2023-07-10T00:47:23.735Z
tags: system design
editor: markdown
dateCreated: 2023-07-10T00:47:07.907Z
---

# Payment System

## principles

![10 principles for building resilient payment systems (by Shopify).png](http://192.168.25.60:8000/files/file_storage/b1eee04d.png)

### Lower the timeouts, and let the service fail early
The default timeout is 60 seconds. Based on Shopify’s experiences, read timeout of 5 seconds and write timeout of 1 second are decent setups.

### Install circuit breaks
Shopify developed Semian to protect Net::HTTP, MySQL, Redis, and gRPC services with a circuit breaker in Ruby.

### Capacity management
If we have 50 requests arrive in our queue and it takes an average of 100 milliseconds to process a request, our throughput is 500 requests per second.

### Add monitoring and alerting
Google’s site reliability engineering (SRE) book lists four golden signals a user-facing system should be monitored for: latency, traffic, errors, and saturation.

### Implement structured logging
We store logs in a centralized place and make them easily searchable.

### Use idempotency keys
Use the Universally Unique Lexicographically Sortable Identifier (ULID) for these idempotency keys instead of a random version 4 UUID.

### Be consistent with reconciliation
Store the reconciliation breaks with Shopify’s financial partners in the database.

### Incorporate load testing
Shopify regularly simulates the large volume flash sales to get the benchmark results.

### Get on top of incident management
Each incident channel has 3 roles: Incident Manager on Call (IMOC), Support Response Manager (SRM), and service owners.

### Organize incident retrospectives
For each incident, 3 questions are asked at Shopify: What exactly happened? What incorrect assumptions did we hold about our systems? What we can do to prevent this from happening?