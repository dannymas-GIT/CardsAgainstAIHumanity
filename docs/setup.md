# Setup Guide

## Local Development Setup

### Backend Setup

1. Create virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your settings
```

4. Initialize database:
```bash
cd backend
alembic upgrade head
python -m scripts.seed_db  # Load initial data
```

### Frontend Setup

1. Install dependencies:
```bash
cd frontend
npm install
```

2. Set up environment variables:
```bash
cp .env.example .env.local
# Edit .env.local with your settings
```

## Docker Setup

1. Build and start containers:
```bash
docker-compose up --build
```

2. Initialize database:
```bash
docker-compose exec backend alembic upgrade head
docker-compose exec backend python -m scripts.seed_db
```

## Environment Variables

### Backend (.env)
```
DATABASE_URL=sqlite:///./cards_against_ai.db
REDIS_URL=redis://localhost:6379
ANTHROPIC_API_KEY=your_key_here
```

### Frontend (.env.local)
```
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000/ws
```

## Initial Data

The seed script creates:
- Default AI personalities
- Initial card deck
- Test user account

## Troubleshooting

Common issues and solutions:

1. Database connection errors:
   - Check DATABASE_URL format
   - Ensure SQLite file is writable

2. Redis connection errors:
   - Verify Redis is running
   - Check REDIS_URL format

3. Frontend API connection:
   - Verify API_URL in .env.local
   - Check CORS settings in backend