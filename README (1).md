#Cricket Live Commentary 

This is my Python project for the subject CSE- Introduction to Problem Solving and Programming. 

A lightweight Python application that simulates, fetches, and displays real-time or simulated over-by-over cricket commentary, focusing intensely on the critical final phase of a T20 or ODI match—The Last 5 Overs.

# Features (what the program can do)
1.Ball-by-Ball Updates: Real-time generation or fetching of match events (dots, singles, boundaries, wickets, and extras).

2.Over Summary: Concise breakdowns at the end of each over, including total runs scored, wickets fallen, and updated Run Rate (RR) / Required Run Rate (RRR).

3.Customizable Teams & Players: Easily configure batting line-ups, bowlers, and target scores.

# Topics Used
This project applies a comprehensive mix of basic, intermediate, and applied Python features:

1. Core Syntax & Data Structures
Variables & Data Types: Storing match metrics like scores, target runs, total balls, wickets, and run rates using integers (int), floats (float), and strings (str).

Lists & Dictionaries:

Lists: Tracking sequential data like batting lineups, over score histories, and event logs.

Dictionaries: Mapping team rosters, player statistics (runs scored, balls faced, strike rates), and commentary templates.

2. Control Flow & Logic
Conditional Statements (if, elif, else): Handling game states such as checking if a boundary was hit, if a wicket fell, if an innings is complete, or if the target has been successfully chased.

Loops (for and while):

Iterating through the match progression (from over 16 to 20).

Inner loops simulating individual balls (1 to 6 legal deliveries per over, plus handling extras like wides/no-balls).

3. Functions & Modularity
Modular Programming: Splitting code into logical files (main.py, commentary.py, config.py) and utilizing Python's import system to maintain clean architecture.

Function Parameters & Return Values: Writing reusable functions to calculate current run rates (CRR) and required run rates (RRR).

## Example 
import random
import time

class MatchSimulation:
    def __init__(self, batting_team, bowling_team, target):
        self.batting_team = batting_team
        self.bowling_team = bowling_team
        self.target = target
        self.score = 150  # Starting score at the 16th over
        self.wickets = 4   # Wickets lost so far
        self.current_over = 15
        
        # Commentary templates based on match events
        self.outcomes = {
            "0": [
                "Defended solidly back to the bowler, no run.", 
                "Beaten outside the off-stump! Excellent seam movement."
            ],
            "1": [
                "Worked away gently into the leg side for a quick single.", 
                "Pushed into the gap for a comfortable run."
            ],
            "2": [
                "Driven through the covers, good running allows them to come back for a second.",
                "Worked into deep mid-wicket, they sprint hard and complete two runs."
            ],
            "4": [
                "FOUR! Flayed away through point with breathtaking timing!", 
                "Cracked through the cover region! Races away to the boundary rope!"
            ],
            "6": [
                "SIX! That's massive! Picked up early and deposited deep into the stands!", 
                "CLEARED THE ROPE! Full toss dispatched over long-on for a monstrous maximum!"
            ],
            "W": [
                "OUT! Caught in the deep! Sliced it high and the fielder takes a clean catch under pressure!",
                "BOWLED HIM! Cleaned up! Perfect yorker smashes straight into the base of middle stump!"
            ]
        }

    def simulate_over(self, over_num):
        runs_this_over = 0
        wickets_this_over = 0
        ball_outcomes = []
        
        print(f"\n--------------------------------------------------")
        print(f"STARTING OVER {over_num + 1}")
        print(f"--------------------------------------------------")
        
        for ball_num in range(1, 7):
            # Weighted random selection favoring runs in the death overs
            choices = ["0", "1", "1", "2", "4", "4", "6", "W"]
            event = random.choice(choices)
            
            # Prevent wicket count from exceeding 10
            if event == "W":
                if self.wickets < 10:
                    self.wickets += 1
                    wickets_this_over += 1
                else:
                    event = "1"  # Fallback if all out
                    runs_this_over += 1
                    self.score += 1
            elif event.isdigit():
                runs = int(event)
                runs_this_over += runs
                self.score += runs

            # Pick a random descriptive sentence for the commentary
            commentary_text = random.choice(self.outcomes[event])
            
            # Print live ball-by-ball update
            print(f"[{over_num}.{ball_num}] {event.upper()}! - {commentary_text}")
            
            # Realistic pause between deliveries
            time.sleep(0.5)
            
            # Check if target is already chased down
            if self.score >= self.target:
                break
                
        return runs_this_over, wickets_this_over

    def start_game(self):
        print("==================================================")
        print(f"   CRICKET LIVE COMMENTARY: THE FINAL 5 OVERS")
        print("==================================================")
        print(f"Match: {self.batting_team} vs {self.bowling_team}")
        print(f"Target: {self.target} runs | Current Score: {self.score}/{self.wickets} (15.0 Overs)")
        
        balls_remaining = 30
        runs_needed = self.target - self.score
        print(f"Equation: {runs_needed} runs needed from {balls_remaining} balls.\n")
        
        # Simulating overs 16 to 20
        for i in range(15, 20):
            if self.score >= self.target or self.wickets >= 10:
                break
                
            runs_over, wickets_over = self.simulate_over(i)
            balls_remaining -= 6
            runs_needed = max(0, self.target - self.score)
            
            current_rr = self.score / (i + 1)
            required_rr = (runs_needed / (balls_remaining / 6)) if balls_remaining > 0 else 0
            
            # End of over summary block
            print(f"\n--- End of Over {i + 1} ---")
            print(f"Score: {self.score}/{self.wickets} ({i + 1}.0 Overs) | This Over: {runs_over} runs, {wickets_over} wkts")
            print(f"Current RR: {current_rr:.2f} | Required RR: {required_rr:.2f} ({runs_needed} runs needed from {balls_remaining} balls)\n")
            time.sleep(1)

        # Match Result Conclusion
        print("==================================================")
        print("               MATCH CONCLUDED                    ")
        print("==================================================")
        if self.score >= self.target:
            print(f"🎉 {self.batting_team} have successfully chased down the target and won the match!")
        elif self.wickets >= 10:
            print(f"🚨 All Out! {self.bowling_team} defend their total and win the match!")
        else:
            print(f"🏆 {self.bowling_team} win by defending the target!")

# Run the simulation
if __name__ == "__main__":
    game = MatchSimulation(batting_team="Royal Strikers", bowling_team="Super Kings", target=185)
    game.start_game() 

# Made by 
Saurav kapasiya 
B.Tech CSE, First Year    
