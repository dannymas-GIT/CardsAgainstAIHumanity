# Cards Against AI

An AI-powered adaptation of Cards Against Humanity, built with FastAPI and React.

## Quick Start

```bash
# Clone the repository
git clone [repository-url]
cd cards-against-ai

# Start all services using Docker
make docker-up

# Access the application
Frontend: http://localhost:3000
API: http://localhost:8000/docs
```

## Prerequisites

- Python 3.8+
- Node.js 16+
- Docker and Docker Compose
- Make

## Development Setup

```bash
# Create virtual environment and install dependencies
make setup

# Start backend server
make run-backend

# Start frontend development server
make run-frontend

# Run tests
make test-backend
make test-frontend
```

## Project Structuref