---
layout: post
title: The One with Queue Theory
date: 2021-06-06 15:07:00 +0300
description: A little bit about queue theory
img: we-in-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---

Have you ever think about the math behind a Queue ?

_[I'll assume that you said no, that's why I'm posting this]_

So, let's talk a little bit about the Queue Theory

_[Drumrolls begin]_

## What its a Queue ?

Its basically a line, or a sequence of anything, awaiting their turn to be attended ot preceed.

### Examples of Queues

- Shoppers in a checkout line
- Patients in a waiting room
- Lines to use the toilets
- Lines to use ATM's

## So, whats the deal of Queue Theory ?

Queue Theory is the study of the movement of things through a line, which can be used to design services.

The goal is to reduce its adverse impact to "Tolerable" levels

## Characteristics

- Arrival distribution
- Departure distribution
- Service channels
- Service discipline
- Maximum number of customers allowed in the system
- Calling source

### Examples

A checkin in an airport, where the employee of the Airline is the server.

**Service Time:** Amount of time that the employee would take to serve you.  
**Arrival distribution:** Poisson distribution (Markov)  
**Queue size:** Can be finite or infinite  
**Queue discipline:**

- FCFS (First Come, First Served)
- LCFS (Last Come, First Served)
- Service in Random Order (SRO)
- General Service Disciplie (GD)
- Priority (P)

### Attitude

- **Jockeying:** When the customer enter one line and then switches to a different one in an effort to reduce the waiting line
- **Balking:** The customer decides to not enter the waiting line
- **Reneging:** The customer enter the line but decides to leave before being served

### Kendall Notation

`(a/b/c) : (d/e/f)`

**a:** Arrival distribution

- _M = Poisson arrival_
- _D = Deterministic arrival_
- _Ek = Erlangian/Gamma arrival_
- _GI = General Independent distribution_
- _G = General distribution_  
  **b:** Departure Distribution
- _Number of servers_  
  **d:** Services Disciplines
- _FCFS - LCFS - SRO - GD_  
  **e:** Maximum number of customers allowed in the system
- _finite or infinite_  
  **f:** Calling source
- _finite or infinte_

### Symbols

- **ƛ**: _Arrival rate_
- **𝜇**: _Service rate per busy server_  
  ⍴: ƛ / 𝜇  
  _Utilization factor_
- **n**: _Number of units in the system_
- **Pn(t)**: _Probability of exactly n customers in the system at time t_
- **Pn**: _Probability of exactly n customers in the system_
- **c**: _Number of parallel servers_
- **Ws**: _Expected waiting time per customer in the system_
- **Wq**: _Expected waiting time per customer in the queue_
- **Ls**: _Expected number of customers in the system_
- **Lq**: _Expected number of customers in the queue_

### Exercise

A Checkin flow of an airline company has one employee which can serve 6 customers per hour. Customers arrive at the airline company at a rate of 10 per hour which is exponentially distributed.

**(a/b/c) : (d/e/f)**  
**(M/M/1) : (FCFS / infinite / infinite)**  
M: distributed (Markov)  
M: distributed (Markov)  
1: we have only one employee, so 1  
FCFS: First Come, First Served  
infinite: entire population

**ƛ = Arrival rate**

```
10 customers / 60 mins
= 1 customer / 6 min
(1 customer arrive every 6 min)
```

**𝜇 = Service rate per busy server**

```
6 customers / 60 min
= 1 customers / 10 min
(1 customer would be served for 10 min)
```

**Utilization factor**

```
⍴ = ƛ / 𝜇
0.60
(60%)
```

**Ws: Expected waiting time per customer in the system**

```
(1 / 𝜇 - ƛ)
= (1 / 10 - 6)
= 0.25 hrs (15 min)
```

**Wq: Expected waiting time per customer in the queue**

```
(ƛ / 𝜇(𝜇 - ƛ))
= (6 / 10(10 - 6))
= 0.15 hrs
```

**Ls: Expected number of customers in the system**

```
(ƛ / (𝜇 - ƛ))
= (6 / (10 - 6))
= 2 customers
```

**Lq: Expected number of customers in the queue**

```
(ƛ*ƛ / 𝜇(𝜇 - ƛ))
= (36 / 10(10 - 6))
= 1 customer
```

_Any questions, opinions or insults !? ;)_
