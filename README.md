# 2212237 — DevOps Final Project

> **Name:** Shahwaiz Rehman Khan
> **Student:** 2212237
> **Course:** DevOps Fundamentals
> **Live URL:** http://3.84.247.101:8000

---

## Architecture

```text
GitHub Push
    │
    ├── CI Pipeline (GitHub Actions)
    │       ├── flake8 lint
    │       └── pytest (with PostgreSQL service container)
    │
    └── CD Pipeline (GitHub Actions)
            └── SSH into EC2
                    └── git pull + docker compose up --build
```

**Services:**
- `web` — FastAPI application on port 8000
- `db`  — PostgreSQL 15 with persistent named volume

---

## Local Setup

**Prerequisites:** Docker, Docker Compose, Python 3.12

```bash
# 1. Clone the repository
git clone [https://github.com/shahwaizrehman/2212237-devops-project.git](https://github.com/shahwaizrehman/2212237-devops-project.git)
cd 2212237-devops-project

# 2. Create your local environment file
cp env.example .env
# Edit .env with your local database values

# 3. Start all services
docker compose up --build

# 4. Test the API
curl http://localhost:8000/health
curl http://localhost:8000/students
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /health | Health check + DB connection status |
| POST | /students | Create a new student record |
| GET | /students | List all students |
| GET | /students/{reg_no} | Get student by registration number |

---

## EC2 Deployment

```bash
# SSH into your EC2 instance
ssh -i your-key.pem ubuntu@3.84.247.101

# Install dependencies and configure Docker access
sudo apt update && sudo apt install -y docker.io docker-compose git
sudo usermod -aG docker ubuntu
newgrp docker

# Clone repository
git clone [https://github.com/shahwaizrehman/2212237-devops-project.git](https://github.com/shahwaizrehman/2212237-devops-project.git) ~/devops-project
cd ~/devops-project

# Create production environment variables
echo "DB_USER=devops_user" > .env.production
echo "DB_PASSWORD=your_strong_password_2212237" >> .env.production
echo "DB_NAME=devops_db" >> .env.production
chmod 600 .env.production

# Start production services
docker compose -f docker-compose.prod.yml up -d --build
```

**GitHub Secrets required:**
- `EC2_HOST` — your EC2 public IP address
- `EC2_SSH_KEY` — your private SSH key (contents of .pem file)

---

## Running Tests

```bash
pip install -r requirements.txt
pytest app/tests/ -v
```

---

*DevOps Fundamentals — Instructor: Afaq Ahmed*