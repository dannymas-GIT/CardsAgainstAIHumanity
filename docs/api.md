# API Documentation

## Authentication

The API uses token-based authentication. Include the token in the Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Game Management

#### Create Game Session

```http
POST /api/game-sessions
```

Request:
```json
{
  "username": "string",
  "aiPersonalityId": "string"
}
```

Response:
```json
{
  "id": "string",
  "status": "active",
  "currentRound": 1,
  "aiPersonality": {
    "id": "string",
    "name": "string"
  }
}
```

#### Submit Cards

```http
POST /api/game-rounds/{round_id}/submit
```

Request:
```json
{
  "cardIds": ["string"],
  "availableCards": ["string"]
}
```

Response:
```json
{
  "winner": "user|ai",
  "explanation": "string",
  "aiCards": ["string"],
  "userScore": 0,
  "aiScore": 0,
  "roundNumber": 1
}
```

### AI Personalities

#### List AI Personalities

```http
GET /api/ai-personalities
```

Response:
```json
{
  "personalities": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "isCustom": false
    }
  ]
}
```

#### Create Custom AI

```http
POST /api/ai-personalities
```

Request:
```json
{
  "name": "string",
  "description": "string"
}
```

## WebSocket Events

Connect to: `ws://host/ws/game/{game_id}`

### Events From Server

```typescript
interface GameStateUpdate {
  type: 'gameState';
  data: {
    currentRound: number;
    cards: Card[];
    scores: Score;
  };
}

interface RoundComplete {
  type: 'roundComplete';
  data: {
    winner: 'user' | 'ai';
    explanation: string;
  };
}
```

### Events From Client

```typescript
interface SubmitCards {
  type: 'submitCards';
  data: {
    cardIds: string[];
  };
}
```

## Error Handling

The API uses standard HTTP status codes and returns error responses in the format:

```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": {}
  }
}
```

Common status codes:
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found
- 409: Conflict
- 500: Internal Server Error

## Rate Limiting

- 100 requests per minute per IP
- 1000 requests per hour per user
- WebSocket connections limited to 1 per game per user

## Data Models

```typescript
interface Card {
  id: string;
  text: string;
  type: 'black' | 'white';
}

interface Game {
  id: string;
  status: 'active' | 'completed';
  currentRound: number;
  scores: {
    user: number;
    ai: number;
  };
}

interface Round {
  id: string;
  gameId: string;
  number: number;
  status: 'in_progress' | 'completed';
  winner?: 'user' | 'ai';
}
```