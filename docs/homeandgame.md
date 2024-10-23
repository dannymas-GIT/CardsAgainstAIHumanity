mport React, { useState } from 'react';
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Users, User, ArrowRight, Copy } from 'lucide-react';

const GameApp = () => {
  const [currentView, setCurrentView] = useState('home');

  const HomePage = () => (
    <div className="p-8 max-w-6xl mx-auto space-y-8">
      <header className="text-center mb-12">
        <h1 className="text-4xl font-bold mb-4">Cards Against AI</h1>
        <p className="text-xl text-gray-600">The AI-powered party game for horrible people</p>
      </header>

      <div className="grid md:grid-cols-2 gap-8">
        <Card className="hover:shadow-lg transition-shadow">
          <CardHeader>
            <CardTitle>Create New Game</CardTitle>
            <CardDescription>Start a new game and invite friends</CardDescription>
          </CardHeader>
          <CardContent>
            <div className="space-y-4">
              <Button className="w-full" onClick={() => setCurrentView('game')}>
                <ArrowRight className="mr-2 h-4 w-4" /> Start Game
              </Button>
            </div>
          </CardContent>
        </Card>

        <Card className="hover:shadow-lg transition-shadow">
          <CardHeader>
            <CardTitle>Join Game</CardTitle>
            <CardDescription>Enter a game code to join</CardDescription>
          </CardHeader>
          <CardContent>
            <div className="space-y-4">
              <Button variant="outline" className="w-full">
                <Users className="mr-2 h-4 w-4" /> Join Existing Game
              </Button>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );

  const GamePage = () => (
    <div className="min-h-screen bg-gray-50">
      <div className="p-4">
        <div className="mb-4 flex justify-between items-center">
          <div className="flex items-center space-x-2">
            <h2 className="text-xl font-bold">Game #ABC123</h2>
            <Button variant="outline" size="sm">
              <Copy className="h-4 w-4 mr-1" /> Share
            </Button>
          </div>
          <div className="flex items-center space-x-2">
            <span className="text-sm text-gray-600">Round 1/10</span>
            <Button variant="outline" size="sm">Leave Game</Button>
          </div>
        </div>

        <div className="grid grid-cols-12 gap-4">
          {/* Left sidebar - Players */}
          <div className="col-span-2">
            <Card>
              <CardHeader>
                <CardTitle className="text-sm">Players</CardTitle>
              </CardHeader>
              <CardContent>
                <div className="space-y-2">
                  {['Player 1', 'AI Player', 'Player 3'].map((player) => (
                    <div key={player} className="flex items-center space-x-2">
                      <User className="h-4 w-4" />
                      <span className="text-sm">{player}</span>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>
          </div>

          {/* Main game area */}
          <div className="col-span-10">
            <Card className="mb-4">
              <CardContent className="p-6">
                <p className="text-xl font-bold text-center">
                  The black card prompt goes here _________.
                </p>
              </CardContent>
            </Card>

            {/* Player's hand */}
            <div className="grid grid-cols-5 gap-4">
              {[1, 2, 3, 4, 5].map((card) => (
                <Card key={card} className="hover:shadow-lg cursor-pointer transition-shadow">
                  <CardContent className="p-4">
                    <p>White card {card}</p>
                  </CardContent>
                </Card>
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  return currentView === 'home' ? <HomePage /> : <GamePage />;
};

export default GameApp;