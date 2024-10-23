# Development Guide

## Development Workflow

1. Create feature branch:
```bash
git checkout -b feature/your-feature
```

2. Start development servers:
```bash
# Terminal 1 - Backend
make run-backend

# Terminal 2 - Frontend
make run-frontend
```

3. Follow code style:
- Backend: PEP 8
- Frontend: ESLint + Prettier

## Code Organization

### Backend Structure

```plaintext
backend/
├── app/
│   ├── api/         # API endpoints
│   ├── core/        # Core functionality
│   ├── models/      # Database models
│   ├── schemas/     # Pydantic schemas
│   └── services/    # Business logic
```

### Frontend Structure

```plaintext
frontend/
├── src/
│   ├── api/         # API clients
│   ├── components/  # React components
│   ├── hooks/       # Custom hooks
│   ├── pages/       # Page components
│   └── utils/       # Utilities
```

## API Development

1. Define schema in `openapi_final.yaml`
2. Create endpoint in appropriate router
3. Implement service logic
4. Add tests
5. Update documentation

Example endpoint:
```python
@router.post("/game-rounds/{round_id}/submit")
async def submit_cards(
    round_id: str,
    cards: List[str],
    game_service: GameService = Depends(get_game_service)
) -> RoundResult:
    return await game_service.submit_cards(round_id, cards)
```

## Frontend Development

1. Add API client method
2. Create/update components
3. Implement state management
4. Add tests
5. Update documentation

Example component:
```typescript
const GameBoard: React.FC = () => {
  const { gameState, submitCards } = useGame();
  
  return (
    <div>
      {/* Component JSX */}
    </div>
  );
};
```

## Testing

### Backend Tests

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_api/test_game.py

# Run with coverage
pytest --cov=app
```

### Frontend Tests

```bash
# Run all tests
npm test

# Run specific test file
npm test src/components/GameBoard.test.tsx

# Run with coverage
npm test -- --coverage
```

## Common Tasks

### Database Migrations

```bash
# Create migration
alembic revision --autogenerate -m "description"

# Apply migrations
alembic upgrade head

# Rollback migration
alembic downgrade -1
```

### Adding Dependencies

Backend:
```bash
pip install package-name
pip freeze > requirements.txt
```

Frontend:
```bash
npm install package-name
```

## Best Practices

1. Code Style
   - Use type hints
   - Write docstrings
   - Follow naming conventions

2. Testing
   - Write tests first
   - Mock external services
   - Use fixtures

3. Git
   - Write clear commit messages
   - Keep PRs focused
   - Squash commits when merging

4. Documentation
   - Update docs with changes
   - Include examples
   - Document breaking changes