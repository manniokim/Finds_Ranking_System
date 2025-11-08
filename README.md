# Finds - Dynamic Percentage-Based Ranking System

This repository showcases the dynamic scoring and ranking system for **Finds**, a daily word-finding puzzle game where players create words from a set of seven letters.

The ranking system is designed to provide players with a clear ladder of progression, motivating them to find more and longer words to advance from "Newbie" to the ultimate "Keeper" status.

## System Design & Philosophy

A static point system can feel arbitrary and unbalanced from one puzzle to the next. This system was designed to be **dynamic and scalable**, built on a core philosophy of consistent player experience.

1.  **Percentage-Based Thresholds:** Ranks are tied to a percentage of the *total possible score* for each day's unique puzzle. This means the system automatically scales. Whether a puzzle has a maximum of 150 points or 400 points, achieving "Great" rank will always represent the same level of accomplishment (e.g., 40% of the total).

2.  **Player Motivation:** Instead of just a raw score, the ranks provide tangible goals. The UI constantly shows the player their current rank and exactly how many points are needed to reach the next one, creating a compelling "just one more word" gameplay loop.

3.  **The "Keeper" Goal:** While "Mastery" is the highest rank on the percentage ladder, a special rank, "Keeper," is awarded only upon finding 100% of the words. This provides an ultimate goal for completionists beyond just hitting a high score.

## How It Works

The system operates through two key functions working together:

1.  `calculateTotalScore()`: Before the game starts, this function iterates through every valid word for the day's puzzle and calculates the maximum possible score.
2.  `calculateRankThresholds()`: Using the maximum score as a baseline, this function then calculates the specific point values needed to achieve each rank based on the pre-defined percentages.

### Code Highlight: The Configuration and Logic

The system's elegance comes from the combination of a simple configuration array and a function that brings it to life. First, we define the ranks and their percentage thresholds:

```javascript
const rankConfig = [
    { name: "Newbie", threshold: 0 }, 
    { name: "Good", threshold: 0.05 }, 
    { name: "Very Good", threshold: 0.15 }, 
    { name: "Solid", threshold: 0.25 }, 
    { name: "Great", threshold: 0.40 }, 
    { name: "Fantastic", threshold: 0.55 }, 
    { name: "Superb", threshold: 0.70 }, 
    { name: "Brilliant", threshold: 0.85 }, 
    { name: "Mastery", threshold: 0.95 }, 
    { name: "Keeper", threshold: 1 }
];

Next, the calculateRankThresholds function converts these percentages into concrete point goals for the current puzzle. This is the core of the dynamic system.

/**
 * Converts percentage-based thresholds into specific point values for the current puzzle.
 * This function is called once at the start of the game after the total possible score is known.
 */
function calculateRankThresholds() {
    // The totalPossibleScore is a global variable calculated beforehand.
    rankConfig.forEach(rank => {
        // The "Keeper" rank is special; it's always equal to the maximum possible score.
        // The "Newbie" rank is the starting point at 0.
        if (rank.name === "Keeper") {
            rank.points = totalPossibleScore;
        } else if (rank.name === "Newbie") {
            rank.points = 0;
        } else {
            // For all other ranks, calculate the required points based on the percentage.
            // Math.ceil() ensures there are no fractional points.
            rank.points = Math.ceil(totalPossibleScore * rank.threshold);
        }
    });
}

This approach ensures that the player's journey through the ranks feels consistent and fair every single day, no matter how the puzzle changes.

Technologies Used
JavaScript (ES6): For all game logic, including score calculation and UI state management.
HTML/CSS: For structuring and styling the dynamic progress bar and rankings page.
