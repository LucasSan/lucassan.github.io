---
layout: post
title: The One with Queue Theory
date: 2021-06-06 15:07:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: we-in-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---

Have you ever think about the math behind a Queue ?

*[I'll assume that you said no, that's why I'm posting this]*

So, let's talk a little bit about the Queue Theory

*[Drumrolls begin]*

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

```(a/b/c) : (d/e/f)```

a: Arrival distribution
  - *M = Poisson arrival*
  - *D = Deterministic arrival*
  - *Ek = Erlangian/Gamma arrival*
  - *GI = General Independent distribution*
  - *G = General distribution*
b: Departure Distribution
c: Services Channels
  - *Number of servers*
d: Services Disciplines
  - *FCFS - LCFS - SRO - GD*
e: Maximum number of customers allowed in the system
  - *finite or infinite*
f: Calling source
  - *finite or infinte*

### Symbols
- ƛ
  *Arrival rate*
- 𝜇
  *Service rate per busy server*
  ⍴: ƛ / 𝜇
  *Utilization factor*
- n
  *Number of units in the system*
- Pn(t)
  *Probability of exactly n customers in the system at time t*
- Pn
  *Probability of exactly n customers in the system*
- c
  *Number of parallel servers*
- Ws
  *Expected waiting time per customer in the system*
- Wq
  *Expected waiting time per customer in the queue*
- Ls
  *Expected number of customers in the system*
- Lq
  *Expected number of customers in the queue*

### Exercise
A Checkin flow of an airline company has one employee which can serve 6 customers per hour. Customers arrive at the airline company at a rate of 10 per hour which is exponentially distributed.

(a/b/c) : (d/e/f)
(M/M/1) : (FCFS / infinite / infinite)
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

*Any questions, opinions or insults !? ;)*