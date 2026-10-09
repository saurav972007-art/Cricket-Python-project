# Python-project
mport random
import time

COMMENTARY = {
    "dot": [
        "{bowler} to {batsman}, no run. Good, tight delivery.",
        "{bowler} to {batsman}, defended solidly, no run.",
    ],
    "1": ["{bowler} to {batsman}, pushed for a quick single."],
    "2": ["{bowler} to {batsman}, driven well, they come back for two."],
    "4": ["{bowler} to {batsman}, FOUR! Cracked away to the boundary!"],
    "6": ["{bowler} to {batsman}, SIX! That's gone miles into the stands!"],
    "wicket": [
        "{bowler} to {batsman}, OUT! Bowled him, that's plumb!",
        "{bowler} to {batsman}, OUT! Caught in the deep!",
    ],
}

OUTCOMES = ["dot", "1", "2", "4", "6", "wicket"]
WEIGHTS = [40, 25, 10, 12, 8, 5]  # roughly realistic frequencies

def main():
    print("=== Custom Live Match Simulation Setup ===\n")
    
    # 1. Setup Batters Squad
    print("--- Batting Squad Setup ---")
    batters = []
    for i in range(1, 6):
        name = input(f"Enter name for Batter {i}: ").strip()
        batters.append(name if name else f"Batter {i}")

    runs = 0
    wickets = 0
    striker_idx = 0
    non_striker_idx = 1

    print("\n=== Match Commences! ===\n")

    # 2. Loop through 5 Overs with User-Chosen Bowlers
    for over in range(5):
        if wickets >= len(batters) - 1:
            print("\nAll out!")
            break

        print(f"\n==============================")
        print(f"        STARTING OVER {over + 1}")
        print(f"==============================")
        
        # Prompt user to choose bowler for this specific over
        current_bowler = input(f"Enter bowler's name for Over {over + 1}: ").strip()
        if not current_bowler:
            current_bowler = f"Bowler {over + 1}"
            
        print(f"\n-> Bowler for this over: {current_bowler}\n")

        for ball in range(6):
            striker = batters[striker_idx]
            
            outcome = random.choices(OUTCOMES, weights=WEIGHTS, k=1)[0]
            line = random.choice(COMMENTARY[outcome]).format(bowler=current_bowler, batsman=striker)
            print(f"[{over}.{ball + 1}] {line}")

            if outcome == "wicket":
                wickets += 1
                print(f"* WICKET! {striker} is out. Total Wickets: {wickets} *")
                if wickets < len(batters) - 1:
                    striker_idx = max(striker_idx, non_striker_idx) + 1
                    print(f"--> New batsman walking in: {batters[striker_idx]}\n")
                else:
                    break
            else:
                run_value = 0 if outcome == "dot" else int(outcome)
                runs += run_value
                if run_value % 2 == 1:
                    # Rotate strike on odd runs
                    striker_idx, non_striker_idx = non_striker_idx, striker_idx

            time.sleep(0.4)

        print(f"\n--- End of Over {over + 1} | Score: {runs}/{wickets} ---\n")
        
        # Swap strike at the end of the over
        striker_idx, non_striker_idx = non_striker_idx, striker_idx

    print(f"\n==============================")
    print(f" FINAL SCORE: {runs}/{wickets}")
    print(f"==============================")

if _name_ == "_main_":
    main()r 
