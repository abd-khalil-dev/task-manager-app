# 📋 Task Manager — Full-Stack DevOps Project

A containerized Task Manager web application deployed on AWS using a production-style, multi-tier architecture. Built as a hands-on practice project covering **Docker, Git/GitHub, and core AWS services** (VPC, EC2, RDS, ALB, CloudWatch).

---

## 🏗️ Architecture
                    ┌─────────────────────┐
                    │        Users         │
                    └──────────┬───────────┘
                               │
                               ▼
                 ┌─────────────────────────────┐
                 │  Application Load Balancer   │
                 │        (Port 80)             │
                 └──────────┬──────────┬────────┘
                            │          │
                ┌───────────▼──┐   ┌───▼───────────┐
                │  EC2 Instance │   │  EC2 Instance  │
                │   (AZ 1)      │   │   (AZ 2)       │
                │ Docker:       │   │  Docker:       │
                │ Flask App     │   │  Flask App     │
                │ (Port 5000)   │   │  (Port 5000)   │
                └───────┬───────┘   └────────┬───────┘
                        │                    │
                        └─────────┬──────────┘
                                  ▼
                      ┌───────────────────────┐
                      │   AWS RDS (PostgreSQL) │
                      │   Central Database     │
                      └───────────────────────┘

          All resources inside a custom VPC
          (2 public subnets across 2 Availability Zones)

          CloudWatch monitors EC2 CPU utilization
          and triggers an alarm above 70%

---

## 🚀 Tech Stack

| Layer            | Technology                          |
|-------------------|--------------------------------------|
| Application       | Python (Flask)                      |
| Frontend          | HTML + Jinja2 templates             |
| Database          | PostgreSQL (AWS RDS)                |
| Containerization  | Docker + Docker Compose             |
| Version Control   | Git + GitHub                        |
| Cloud Provider    | AWS                                 |
| Networking        | Custom VPC, public subnets, IGW     |
| Compute           | 2x EC2 instances (Amazon Linux)     |
| Load Balancing    | Application Load Balancer (ALB)     |
| Monitoring        | Amazon CloudWatch (CPU alarm)       |

---

## ✨ Features

- Add new tasks with a title and description
- View all tasks in a clean, styled UI
- Delete tasks
- Data persists in a central PostgreSQL database — both app servers read/write to the same source of truth
- Highly available: if one EC2 instance goes down, the Load Balancer routes traffic to the healthy instance

---

## ⚙️ How It Was Built

1. Built a Flask app with routes for viewing, adding, and deleting tasks
2. Connected the app to a PostgreSQL database using `psycopg2`
3. Wrote a `Dockerfile` and `docker-compose.yml` to containerize the app
4. Pushed the code to GitHub for version control
5. Created a custom AWS VPC with 2 public subnets across 2 Availability Zones
6. Launched an AWS RDS PostgreSQL instance inside the VPC (not publicly accessible)
7. Launched 2 EC2 instances, installed Docker on each, and deployed the containerized app — both connected to the same RDS database
8. Created a Target Group and an Application Load Balancer to distribute traffic across both EC2 instances
9. Set up a CloudWatch alarm to monitor CPU utilization and alert above a 70% threshold

---

## 🖥️ Running Locally

```bash
git clone https://github.com/YOUR_USERNAME/task-manager-app.git
cd task-manager-app

# Set your local/remote DB credentials as environment variables,
# then build and run:
docker-compose up -d --build
```

App will be available at `http://localhost:5000`

---

## 📌 Notes

- Database credentials are passed via environment variables (never hardcoded)
- RDS is configured with **no public access** — only reachable from the EC2 instances' security group
- This project was built as a learning exercise to practice Docker, Git/GitHub workflows, and core AWS infrastructure (VPC, EC2, RDS, ALB, CloudWatch)

---

## 🔮 Possible Next Steps

- Add a CI/CD pipeline (GitHub Actions) for automatic build & deploy on every push
- Add HTTPS via AWS Certificate Manager + a custom domain (Route 53)
- Move to auto-scaling EC2 groups instead of fixed instances

