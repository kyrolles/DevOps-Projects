# Project Prometheus 1 — Disk Usage Monitoring & Alerting

A monitoring stack using **Prometheus**, **Alertmanager**, and **Node Exporter** on AWS EC2, with alerts sent via **Telegram** and **Email**.

---

## Stack

- Prometheus + Alertmanager (Docker Compose)
- Node Exporter (on EC2 instances)
- Alert: disk usage > 80%
- Notifications: Telegram bot + Gmail

---

## Setup & Running

### 1. Node Exporter on EC2

Install and run Node Exporter on each EC2 instance to expose hardware/OS metrics on port `9100`.

<img width="1920" height="941" alt="Image" src="https://github.com/user-attachments/assets/cfb4cb49-1a3c-45ca-9295-b0072afd0bba" />
---

### 2. Simulate High Disk Usage

Use `fallocate` to create a large file and push disk usage above 80% to trigger the alert.

<img width="1920" height="941" alt="Image" src="https://github.com/user-attachments/assets/646451f7-0566-4278-a1af-acea59ac8433" />

---

### 3. Start the Stack

```bash
docker compose up -d
```

<img width="1910" height="1072" alt="Image" src="https://github.com/user-attachments/assets/dc359ba9-c7e6-43dd-ac19-cb05999c7e86" />
---

## Alerts

### Prometheus — PENDING

Once disk usage exceeds 80%, the alert enters **PENDING** state (waiting for the `for: 1m` duration).

<img width="1910" height="1072" alt="Image" src="https://github.com/user-attachments/assets/df4fd9e9-adfa-4831-b2ab-c532b970003b" />

---

### Prometheus — FIRING

After 1 minute, the alert transitions to **FIRING**.

<img width="1920" height="925" alt="Image" src="https://github.com/user-attachments/assets/117903ec-76b5-459b-bec1-e97d78c10c92" />

---

### Alertmanager

Alertmanager receives the firing alert and routes it to both receivers.

<img width="1920" height="925" alt="Image" src="https://github.com/user-attachments/assets/0d25c7a9-1077-4468-8bab-01f8685785b3" />

---

## Notifications

### Telegram

<img width="1444" height="874" alt="Image" src="https://github.com/user-attachments/assets/769d306d-3b62-4f42-9f4b-e7b6b536ebfa" />

---

### Email (Gmail)

<img width="1920" height="957" alt="Image" src="https://github.com/user-attachments/assets/44f75593-f51b-4cfb-a0f3-7d8fc9aab42f" />

---

## Project Files

| File | Description |
|------|-------------|
| `prometheus.yml` | Prometheus config — scrape targets & alertmanager endpoint |
| `alert.yml` | Alert rule — disk usage > 80% |
| `alertmanager.yml` | Alertmanager routing — Telegram + Email receivers |
| `docker-compose.yml` | Runs Prometheus + Alertmanager containers |
