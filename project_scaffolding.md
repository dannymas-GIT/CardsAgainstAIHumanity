#!/usr/bin/env python
"""Script to create and verify project scaffolding"""
import os
import shutil
from pathlib import Path

def create_directory(path):
    Path(path).mkdir(parents=True, exist_ok=True)
    Path(path) / '.gitkeep'

def create_file(path, content=''):
    with open(path, 'w') as f:
        f.write(content)

def setup_project():
    # Project root structure
    directories = [
        'backend/app/api/v1/endpoints',
        'backend/app/core',
        'backend/app/models',
        'backend/app/schemas',
        'backend/app/services',
        'backend/tests/test_api',
        'backend/tests/test_services',
        'frontend/src/api',
        'frontend/src/components/ui',
        'frontend/src/components/game',
        'frontend/src/components/shared',
        'frontend/src/pages',
        'frontend/src/hooks',
        'frontend/src/types',
        'frontend/src/utils',
        'docs/api',
        'docker'
    ]

    # Create directories
    for directory in directories:
        create_directory(directory)

    # Create essential files
    files = {
        'Makefile': '''
.PHONY: setup test run

setup:
	python -m venv venv
	. venv/bin/activate && pip install -r requirements.txt
	cd frontend && npm install

run-backend:
	cd backend && uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

run-frontend:
	cd frontend && npm run dev

test-backend:
	cd backend && pytest

test-frontend:
	cd frontend && npm test

docker-up:
	docker-compose up -d

docker-down:
	docker-compose down
''',
        'docker-compose.yml': '''
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    volumes:
      - ./backend:/app
    environment:
      - DATABASE_URL=sqlite:///./cards_against_ai.db
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
    depends_on:
      - backend

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
''',
        'backend/requirements.txt': '''
fastapi>=0.68.0,<0.69.0
uvicorn>=0.15.0,<0.16.0
sqlmodel>=0.0.8
python-jose[cryptography]>=3.3.0,<4.0.0
passlib[bcrypt]>=1.7.4,<2.0.0
python-multipart>=0.0.5,<0.1.0
redis>=4.0.0,<5.0.0
httpx>=0.23.0
alembic>=1.7.0
pytest>=6.2.5
pytest-asyncio>=0.18.0
aiohttp>=3.8.0
''',
        'backend/app/main.py': '''
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.api.v1.router import api_router

app = FastAPI(
    title="Cards Against AI",
    description="API for Cards Against AI game",
    version="1.0.0"
)

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

app.include_router(api_router, prefix="/api/v1")

@app.get("/health")
async def health_check():
    return {"status": "healthy"}
''',
        'frontend/package.json': '''
{
  "name": "cards-against-ai",
  "private": true,
  "version": "0.1.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "test": "vitest",
    "lint": "eslint src --ext ts,tsx",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.8.0",
    "@radix-ui/react-dialog": "^1.0.4",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.0.0",
    "tailwind-merge": "^1.14.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.27",
    "@types/react-dom": "^18.0.10",
    "@typescript-eslint/eslint-plugin": "^6.5.0",
    "@typescript-eslint/parser": "^6.5.0",
    "@vitejs/plugin-react": "^3.1.0",
    "autoprefixer": "^10.4.15",
    "eslint": "^8.48.0",
    "postcss": "^8.4.29",
    "tailwindcss": "^3.3.3",
    "typescript": "^4.9.3",
    "vite": "^4.1.0",
    "vitest": "^0.34.3"
  }
}
''',
        'frontend/vite.config.ts': '''
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://localhost:8000',
        changeOrigin: true,
      },
    },
  },
})
''',
    }

    for file_path, content in files.items():
        create_file(file_path, content.strip())

    # Copy documentation files
    docs_path = Path('docs')
    docs_path.mkdir(exist_ok=True)
    
    print("Project scaffolding complete!")

if __name__ == "__main__":
    setup_project()