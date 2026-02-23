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

![Node Exporter Setup](images/node_exporter_setup.png)

---

### 2. Simulate High Disk Usage

Use `fallocate` to create a large file and push disk usage above 80% to trigger the alert.

![Fallocate Disk Fill](images/fallocate_disk_fill.png)

---

### 3. Start the Stack

```bash
docker compose up -d
```

![Docker Compose Up](images/docker_compose_up.png)

---

## Alerts

### Prometheus — PENDING

Once disk usage exceeds 80%, the alert enters **PENDING** state (waiting for the `for: 1m` duration).

![Prometheus Pending](images/prometheus_pending.png)

---

### Prometheus — FIRING

After 1 minute, the alert transitions to **FIRING**.

![Prometheus Firing](images/prometheus_firing.png)

---

### Alertmanager

Alertmanager receives the firing alert and routes it to both receivers.

![Alertmanager Alert](images/alertmanager_alert.png)

---

## Notifications

### Telegram

![Telegram Alert](images/telegram_alert.png)

---

### Email (Gmail)

![Email Alert](images/email_alert.png)

---

## Project Files

| File | Description |
|------|-------------|
| `prometheus.yml` | Prometheus config — scrape targets & alertmanager endpoint |
| `alert.yml` | Alert rule — disk usage > 80% |
| `alertmanager.yml` | Alertmanager routing — Telegram + Email receivers |
| `docker-compose.yml` | Runs Prometheus + Alertmanager containers |
