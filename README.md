# ESD Kitchen Management

An event-driven **microservices** system that runs a restaurant kitchen end to end: menu availability, ordering and payment, kitchen station assignment, inventory restocking and customer feedback.

Built for the Enterprise Solution Development (IS213) module at Singapore Management University.

---

## Overview

The system is split into **16 independent services**, each owning one business capability. They communicate synchronously over **REST APIs** and asynchronously through **RabbitMQ**, and every service runs in its own **Docker** container.

Key design points:
- **Loose coupling:** each service has a single responsibility and its own data.
- **Event-driven workflows:** order and restocking events are published to RabbitMQ, so services react without calling each other directly.
- **Self-healing inventory:** when the Menu Service finds that ingredients are insufficient, the Error Resolution Service automatically triggers the Inventory Service to restock from suppliers.

---

## Features

| Feature | What it does |
|---|---|
| **View Menu** | Shows only the menu items that can be prepared with the ingredients currently in stock. |
| **Order Creation & Payment** | Customers check out and pay via **Stripe**, creating an order job for the kitchen. |
| **Kitchen Station Management** | Assigns orders to stations; chefs see the items to prepare and the exact ingredients and quantities needed. |
| **Order Tracking** | Customers can follow the status of their order after checkout. |
| **Inventory Management** | Tracks all ingredients and supplies. |
| **Automated Restocking** | Low stock triggers restock requests to supplier services. |
| **Feedback & Notifications** | Collects customer feedback and sends email notifications via **Mailjet**. |

---

## Architecture

| Service | Responsibility |
|---|---|
| `menu_service` | Menu items and availability checks |
| `order_service` | Order creation |
| `order_fulfilment_service` | Orchestrates the order flow across services |
| `kitchen_station_service` | Assigns orders to kitchen stations |
| `inventory_service` | Ingredient stock levels |
| `manage_inventory_service` | Inventory management operations |
| `restocking_service` | Handles restock requests |
| `error_resolution_service` | Resolves ingredient shortages by triggering restocking |
| `feedback_service` | Customer feedback collection |
| `RFID_scanning_service` | RFID-based ingredient scanning |
| `supplier_*_service` (×6) | Simulated suppliers: CheeseHaven, DairyDelight, FreshFarms, LettuceLand, OrganicGoods, TomatoExpress |

<!-- Optional: add an architecture diagram here, e.g. ![Architecture](images/architecture.png) -->

---

## Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | Python (Flask), OutSystems |
| Database | MySQL |
| Messaging | RabbitMQ |
| DevOps | Docker, Docker Compose |
| External APIs | [Stripe Payment Intents](https://stripe.com/docs/api/payment_intents), [Mailjet](https://dev.mailjet.com/email/guides/) |

---

## Getting Started

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [WAMP](https://www.wampserver.com/) (MySQL database and hosting for the feedback frontend)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/qinnyen/esd-kitchen-management.git
   cd esd-kitchen-management
   ```
2. Import every `.sql` file in the `sql/` folder into MySQL.
3. Copy `feedback_service/frontend` into WAMP's `www` folder.

### Running the scenarios

**Scenario 1 – Customer feedback**
```bash
cd feedback_service
docker-compose -f compose.yaml up -d --build
```

**Scenario 2 – [describe, e.g. ordering and kitchen fulfilment]**
```bash
docker-compose -f compose_scenario2.yaml up -d --build
```

**Scenario 3 – [describe, e.g. inventory shortage and auto-restocking]**
```bash
docker-compose -f compose.yaml up -d --build
```

---

## Beyond the Labs

- Integrated the **Stripe Payment API** for real payment processing.
- Integrated **Mailjet** for email notifications.

---

## My Contributions

- Developed the `feedback_service` for collecting and managing customer feedback
- Co-developed the inventory management services with a teammate
- Integrated **Mailjet** email notifications into the order and feedback workflows
- Applied RabbitMQ-based asynchronous messaging for order and restocking workflows across services

---

## Team

Built by a team of [N] students at SMU School of Computing and Information Systems.

<p align="right">(<a href="#top">back to top</a>)</p>
