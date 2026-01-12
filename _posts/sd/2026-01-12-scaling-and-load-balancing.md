---
title: "Scaling and Load Balancing: System Design Guide"
date: 2026-01-12 12:00:00 +0530
categories: [System Design, Backend]
tags: [scaling, load-balancer, architecture, system-design]
toc: true
---

If you’re building an application that needs to support **100 users today but 1 million tomorrow**, you need to understand how systems grow.  
This post is your definitive guide to **Scaling and Load Balancing**.

---

## 1. Vertical vs. Horizontal Scaling: How Do We Handle More Users?

When your server starts to sweat under high traffic, you have two choices.
![Vertical vs Horizontal Scaling](/assets/img/posts/blog/veritcalvshorizontalscaling.jpeg)
*Vertical vs Horizontal Scaling*

---

### Vertical Scaling (Scale Up)

**Concept:**  
Adding more power (CPU, RAM, SSD) to an existing single machine.

**Analogy:**  
Buying a bigger, faster truck to carry more boxes.

**Pros:**
- Simple to implement
- No complex distributed logic

**Cons:**
- Hard ceiling (hardware limits)
- Single point of failure (if the truck breaks, everything stops)

---

### Horizontal Scaling (Scale Out)

**Concept:**  
Adding more machines to your pool of resources.

**Analogy:**  
Hiring a fleet of 50 small vans instead of one giant truck.

**Pros:**
- Theoretically infinite scaling
- High availability (if one van breaks, others still work)

**Cons:**
- Requires a load balancer
- Applications must be **stateless** so any server can handle any request

---

## 2. Enter the Load Balancer (LB): The Traffic Cop

A **Load Balancer** sits between users and your server farm, distributing incoming traffic to ensure **performance, reliability, and availability**.
![Load Balancing](/assets/img/posts/blog/loadbalancing.jpeg)
*Load Balancing*

---

### Why Do You Need a Load Balancer?

1. **High Availability**  
   Detects failed servers and reroutes traffic instantly (failover)
2. **SSL Offloading**  
   Handles HTTPS encryption, freeing app servers for business logic
3. **Zero-Downtime Maintenance**  
   Allows servers to be updated without service interruption
4. **Global Server Load Balancing (GSLB)**  
   Routes users to the nearest data center to reduce latency

---

## 3. Layer 4 vs. Layer 7: Choose Your Strategy

Load balancers operate at different layers of the **OSI model**.

---

### Layer 4 Load Balancing (Fast & Blind)

![Layer 4](/assets/img/posts/blog/layer4.jpeg)
*Layer 4*

**Operates at:** Transport Layer (TCP/UDP)

**How it works:**  
Routes traffic based on IP address and port.

**Pros:**
- Extremely fast
- Low CPU overhead

**Key Technologies:**
- **NAT (Network Address Translation)**
- **DSR (Direct Server Return)** — server responds directly to the client

---

### Layer 7 Load Balancing (Smart & Detailed)

![Layer 7](/assets/img/posts/blog/layer7.jpeg)
*Layer 7*

**Operates at:** Application Layer (HTTP/HTTPS)

**How it works:**  
Inspects request content before routing.

**Capabilities:**
- URL-based routing (`/api` → Server A, `/images` → Server B)
- Cookie-based routing (returning users)
- Header or JSON-based routing

---

## 4. How the Load Balancer Remembers You: Persistence (Sticky Sessions)

In a horizontally scaled system, one request may hit Server A and the next may hit Server B.  
Without persistence, users could get logged out unexpectedly.

---

### Persistence Methods

- **Active Cookie Persistence**  
  Load balancer inserts its own cookie to remember the server

- **Source IP Persistence**  
  Same client IP always maps to the same server

- **Hash-Based Routing**  
  Uses consistent hashing on cookies or headers

---

## Reverse Proxy vs. Load Balancer

These two are often confused, but they serve different purposes.

![Reverse Proxy vs Forward Proxy](/assets/img/posts/blog/reverse.png)
*Reverse Proxy vs Forward Proxy*

---

### Reverse Proxy

**Focus:** Security & Anonymity  
- Hides backend servers
- Provides caching, logging, and filtering

---

### Load Balancer

**Focus:** Availability & Traffic Distribution  
- Prevents server overload
- Ensures reliable request handling

---

## What’s Next?

In the next part of this **System Design 101** series, we’ll explore the **different load balancing algorithms** and when to use each one.

Stay tuned 🚀