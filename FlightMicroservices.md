# Microservices activity

## What we're building

Two services that talk to each other over HTTP.

**Group A — Flights Service** (runs on port 5001)
Owns all flight and seat data. Other services ask it questions.

**Group B — Booking Service** (runs on port 5002)
Takes a booking from a customer. Calls the Flights Service to check and reserve seats.

---

## The rule

The two services **cannot share a database or any code**.
They can only communicate by making HTTP requests to each other.

---

## HTTP methods — quick reference

| Method | What it means | When to use it |
|--------|--------------|----------------|
| `GET` | "Give me some information" | Reading data — no changes on the server |
| `POST` | "Here is some data, do something with it" | Creating something, or triggering an action |

**Status codes your service should return:**

- `200` — worked fine
- `404` — not found
- `409` — conflict (e.g. not enough seats)
- `500` — something went wrong on the server

---

## Seed data (Group A)

Start with these flights in memory — no database needed, just a variable.

```python
flights = {
    "BA123": { "seats": 50 },
    "LH456": { "seats": 30 },
    "EK789": { "seats": 12 },
}
```


## Minimal Express starter (Node)

```javascript
const express = require('express')
const app = express()
app.use(express.json())

// your routes go here

app.listen(5001, () => console.log('running on 5001'))
// Group B: change to 5002
```

Install: `npm install express`

To call another service from Group B:
```javascript
const response = await fetch("http://[GROUP_A_IP]:5001/flights/BA123/availability")
const data = await response.json()
```

