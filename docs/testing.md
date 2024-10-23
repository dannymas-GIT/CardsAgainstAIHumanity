# Testing Guide

## Testing Strategy

### Backend Tests

1. Unit Tests
   - Service logic
   - Model methods
   - Utility functions

2. Integration Tests
   - API endpoints
   - Database operations
   - External services

3. End-to-End Tests
   - Complete game flows
   - User scenarios

### Frontend Tests

1. Component Tests
   - Rendering
   - User interactions
   - State management

2. Integration Tests
   - API integration
   - Navigation flows
   - State updates

## Running Tests

### Backend Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test category
pytest tests/unit/
pytest tests/integration/

# Run tests in parallel
pytest -n auto
```

### Frontend Tests

```bash
# Run all tests
npm test

# Run with coverage
npm test -- --coverage

# Run specific test file
npm test src/components/GameBoard.test.tsx

# Watch mode
npm test -- --watch
```

## Writing Tests

### Backend Test Example

```python
# tests/test_services/test_game_service.py
import pytest
from app.services.game_service import GameService

async def test_submit_cards():
    # Arrange
    service = GameService()
    game_id = "test-game"
    cards = ["card1", "card2"]
    
    # Act
    result = await service.submit_cards(game_id, cards)
    
    # Assert
    assert result.status == "submitted"
    assert len(result.cards) == 2
```

### Frontend Test Example

```typescript
// src/components/GameBoard.test.tsx
import { render, fireEvent } from '@testing-library/react';
import GameBoard from './GameBoard';

describe('GameBoard', () => {
  it('allows card selection', () => {
    // Arrange
    const { getByTestId } = render(<GameBoard />);
    
    // Act
    fireEvent.click(getByTestId('card-1'));
    
    // Assert
    expect(getByTestId('card-1')).toHaveClass('selected');
  });
});
```

## Test Data

### Fixtures

```python
# tests/conftest.py
import pytest
from app.models import Game

@pytest.fixture
async def test_game():
    game = Game(
        id="test-game",
        status="active",
        current_round=1
    )
    yield game
```

### Mocks

```python
# tests/mocks.py
class MockAIService:
    async def get_response(self, prompt):
        return {
            "text": "Mock AI response",
            "confidence": 0.9
        }
```

## Coverage Requirements

- Backend: Minimum 80% coverage
- Frontend: Minimum 70% coverage
- Critical paths: 100% coverage

## CI/CD Integration

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run backend tests
        run: |
          pip install -r requirements.txt
          pytest --cov=app
      - name: Run frontend tests
        run: |
          cd frontend
          npm install
          npm test -- --coverage
```

## Best Practices

1. Test Organization
   - Group related tests
   - Use descriptive names
   - Follow AAA pattern (Arrange-Act-Assert)

2. Test Data
   - Use fixtures
   - Reset state between tests
   - Mock external services

3. Assertions
   - Be specific
   - Test edge cases
   - Verify error handling

4. Maintenance
   - Keep tests focused
   - Update with code changes
   - Remove obsolete tests