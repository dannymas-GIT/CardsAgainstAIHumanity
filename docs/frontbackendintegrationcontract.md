## Game page

The game page will have the following interactions with the backend:

* When the user has selected the required number of white cards and hits submit cards, the endpoint `POST /game-rounds/{round_id}/submit` will be invoked with the user selected card ids and the 10 white cards available for the round. This call will return a game round results with information of:
  - winner
  - explanation
  - cards picked by the AI
  - user score
  - ai score 
  - current round number
  Look at the openapi_final.yaml for the schema of the response.

* In the Judge Decision Modal, when the user hits "Next Round", the next game round will be created by invoking the endpoint `POST /game-rounds` with the game session id. This call will return all necessary information to refresh all game page data with the new game round after clearing the modal.

* When the user hits the `End Game` button and confirms to `End Game` in the modal, the endpoint `POST /game-sessions/{session_id}/end` will be invoked to end the game and get final score information. Use the returned data to populate the end result modal.

Additional suggested endpoints that might be needed:

1. Game State Recovery:
```
GET /game-sessions/{session_id}/state
- Retrieves current game state if page is refreshed
- Returns current round, scores, and cards
```

2. Round History:
```
GET /game-sessions/{session_id}/rounds
- Gets history of played rounds
- Useful for reviewing previous plays
```

3. Player Status:
```
POST /game-sessions/{session_id}/heartbeat
- Updates player's active status
- Handles disconnection/reconnection
```

Data Structures for Game Flow:
```typescript
// Submit Cards Request
interface SubmitCardsRequest {
  roundId: string;
  selectedCardIds: string[];
  availableWhiteCards: Card[];
}

// Submit Cards Response
interface RoundResultResponse {
  roundId: string;
  winner: 'user' | 'ai';
  explanation: string;
  aiCards: Card[];
  userScore: number;
  aiScore: number;
  roundNumber: number;
  isGameComplete: boolean;
}

// Next Round Response
interface NextRoundResponse {
  roundId: string;
  blackCard: Card;
  whiteCards: Card[];
  roundNumber: number;
  userScore: number;
  aiScore: number;
}

// End Game Response
interface GameEndResponse {
  winner: 'user' | 'ai';
  finalScore: {
    user: number;
    ai: number;
  };
  totalRounds: number;
  winningExplanation: string;
  roundHistory: RoundResult[];
}
```

Error Handling:
- 400: Invalid card selection
- 403: Not player's turn
- 404: Round/Session not found
- 409: Round already submitted
- 500: Server error

The current endpoints cover the core game flow. The suggested additional endpoints would provide a more robust experience with state recovery and history features. Would you like me to:

1. Detail the error responses for each endpoint?
2. Add WebSocket events for real-time updates?
3. Include rate limiting specifications?
4. Add authentication flow details?

Let me know if you need any clarification or see any missing endpoints!