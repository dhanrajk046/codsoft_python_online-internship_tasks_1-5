#!/usr/bin/env python3
"""
Rock-Paper-Scissors Game - VSCode Compatible Version
Simple but professional implementation for internship demonstration
"""

import random
import json
import os

class RockPaperScissorsGame:
    def __init__(self):
        self.player_wins = 0
        self.computer_wins = 0
        self.ties = 0
        self.total_games = 0
        
        # Game choices
        self.choices = {
            'rock': {'emoji': '🪨', 'beats': 'scissors'},
            'paper': {'emoji': '📄', 'beats': 'rock'},
            'scissors': {'emoji': '✂️', 'beats': 'paper'}
        }
        
        self.stats_file = "rps_stats.json"
        self.load_stats()
    
    def load_stats(self):
        """Load game statistics from file"""
        try:
            if os.path.exists(self.stats_file):
                with open(self.stats_file, 'r') as f:
                    data = json.load(f)
                    self.player_wins = data.get('player_wins', 0)
                    self.computer_wins = data.get('computer_wins', 0)
                    self.ties = data.get('ties', 0)
                    self.total_games = data.get('total_games', 0)
        except (json.JSONDecodeError, FileNotFoundError):
            print("Starting with fresh statistics...")
    
    def save_stats(self):
        """Save game statistics to file"""
        stats = {
            'player_wins': self.player_wins,
            'computer_wins': self.computer_wins,
            'ties': self.ties,
            'total_games': self.total_games,
            'win_rate': self.get_win_rate()
        }
        try:
            with open(self.stats_file, 'w') as f:
                json.dump(stats, f, indent=2)
        except IOError as e:
            print(f"Warning: Could not save stats - {e}")
    
    def get_computer_choice(self):
        """Generate random computer choice"""
        return random.choice(list(self.choices.keys()))
    
    def determine_winner(self, player_choice, computer_choice):
        """Determine the winner of the round"""
        if player_choice == computer_choice:
            return 'tie'
        elif self.choices[player_choice]['beats'] == computer_choice:
            return 'win'
        else:
            return 'lose'
    
    def play_round(self, player_choice):
        """Play a single round"""
        if player_choice not in self.choices:
            return None, None, 'invalid'
        
        computer_choice = self.get_computer_choice()
        result = self.determine_winner(player_choice, computer_choice)
        
        # Update statistics
        self.total_games += 1
        if result == 'win':
            self.player_wins += 1
        elif result == 'lose':
            self.computer_wins += 1
        else:
            self.ties += 1
        
        return computer_choice, result
    
    def get_win_rate(self):
        """Calculate win rate percentage"""
        if self.total_games == 0:
            return 0.0
        return (self.player_wins / self.total_games) * 100
    
    def display_stats(self):
        """Display current statistics"""
        print(f"\n📊 GAME STATISTICS")
        print("=" * 30)
        print(f"Total Games: {self.total_games}")
        print(f"Your Wins: {self.player_wins}")
        print(f"Computer Wins: {self.computer_wins}")
        print(f"Ties: {self.ties}")
        print(f"Win Rate: {self.get_win_rate():.1f}%")
        print("=" * 30)
    
    def reset_stats(self):
        """Reset all statistics"""
        self.player_wins = 0
        self.computer_wins = 0
        self.ties = 0
        self.total_games = 0
        if os.path.exists(self.stats_file):
            os.remove(self.stats_file)
        print("✅ Statistics reset!")


def display_welcome():
    """Display welcome message"""
    print("🎮" + "=" * 50 + "🎮")
    print("     WELCOME TO ROCK-PAPER-SCISSORS!")
    print("🎮" + "=" * 50 + "🎮")
    print("\n📋 RULES:")
    print("   🪨 Rock beats ✂️  Scissors")
    print("   📄 Paper beats 🪨 Rock")
    print("   ✂️  Scissors beat 📄 Paper")
    print("\n💡 COMMANDS:")
    print("   Type: rock, paper, scissors (or r, p, s)")
    print("   'stats' - View statistics")
    print("   'reset' - Reset statistics")
    print("   'quit' - Exit game")
    print("-" * 52)


def get_player_input():
    """Get validated player input"""
    valid_inputs = {
        'rock': 'rock', 'r': 'rock',
        'paper': 'paper', 'p': 'paper', 
        'scissors': 'scissors', 's': 'scissors', 'scissor': 'scissors'
    }
    
    while True:
        try:
            choice = input("\n🎯 Your choice: ").strip().lower()
            
            if choice in ['quit', 'exit', 'q']:
                return 'quit'
            elif choice in ['stats', 'statistics']:
                return 'stats'
            elif choice in ['reset']:
                return 'reset'
            elif choice in ['help', 'h']:
                return 'help'
            elif choice in valid_inputs:
                return valid_inputs[choice]
            else:
                print("❌ Invalid choice! Try: rock, paper, scissors (or r, p, s)")
                
        except (EOFError, KeyboardInterrupt):
            return 'quit'


def display_round_result(game, player_choice, computer_choice, result):
    """Display the result of a round"""
    print(f"\n{'=' * 40}")
    print(f"You chose:     {game.choices[player_choice]['emoji']} {player_choice.title()}")
    print(f"Computer chose: {game.choices[computer_choice]['emoji']} {computer_choice.title()}")
    print("-" * 40)
    
    if result == 'win':
        print("🎉 YOU WIN! 🎉")
    elif result == 'lose':
        print("💻 COMPUTER WINS! 💻")
    else:
        print("🤝 IT'S A TIE! 🤝")
    
    print(f"{'=' * 40}")
    print(f"Score - You: {game.player_wins} | Computer: {game.computer_wins} | Ties: {game.ties}")


def main():
    """Main game function"""
    game = RockPaperScissorsGame()
    display_welcome()
    
    try:
        while True:
            player_input = get_player_input()
            
            if player_input == 'quit':
                break
            elif player_input == 'stats':
                game.display_stats()
            elif player_input == 'reset':
                confirm = input("Are you sure you want to reset stats? (y/N): ").strip().lower()
                if confirm in ['y', 'yes']:
                    game.reset_stats()
            elif player_input == 'help':
                display_welcome()
            elif player_input in game.choices:
                computer_choice, result = game.play_round(player_input)
                display_round_result(game, player_input, computer_choice, result)
                
                # Ask to continue every 5 games
                if game.total_games % 5 == 0:
                    continue_game = input("\n🎲 Continue playing? (Y/n): ").strip().lower()
                    if continue_game in ['n', 'no']:
                        break
    
    except KeyboardInterrupt:
        print("\n👋 Game interrupted!")
    
    finally:
        # Save stats and show final summary
        game.save_stats()
        if game.total_games > 0:
            print("\n🏁 FINAL RESULTS:")
            game.display_stats()
        print("Thanks for playing! 👋")


def demo_game():
    """Demo function to show game in action"""
    print("🧪 DEMO: Rock-Paper-Scissors Game in Action")
    print("=" * 50)
    
    game = RockPaperScissorsGame()
    choices = list(game.choices.keys())
    
    print("Simulating 5 random games...\n")
    
    for i in range(5):
        player_choice = random.choice(choices)
        computer_choice, result = game.play_round(player_choice)
        
        print(f"Game {i+1}:")
        print(f"  You: {game.choices[player_choice]['emoji']} {player_choice.title()}")
        print(f"  Computer: {game.choices[computer_choice]['emoji']} {computer_choice.title()}")
        
        if result == 'win':
            print("  🎉 YOU WIN!")
        elif result == 'lose':
            print("  💻 COMPUTER WINS!")
        else:
            print("  🤝 TIE!")
        print()
    
    game.display_stats()


if __name__ == "__main__":
    # Uncomment the line below to run a demo first
    # demo_game()
    
    # Run the main interactive game
    main()
