# ✏️ Ranked Sudoku (Full-Stack)

> A competitive, hand-drawn styled Sudoku game featuring real-time multiplayer leaderboards, authentication, and a cumulative scoring engine.

*(Replace this link with an actual screenshot of your game)*

## 📖 About The Project

Ranked Sudoku is not just a logic puzzle; it's a **full-stack web application** designed to demonstrate advanced frontend logic and backend integration. 

Unlike standard Sudoku apps, this project features a **"Paper & Ink" aesthetic** using CSS Grid and SVG filters to mimic a hand-drawn look. Under the hood, it is powered by a **backtracking algorithm** for puzzle generation and **Supabase** for handling user authentication and real-time database updates.

The core loop revolves around a **cumulative scoring system**, where players build their "Rank" over multiple sessions, competing for a spot on the global leaderboard.

## ✨ Key Features

### 🎮 Gameplay & Logic
* **Dynamic Difficulty:** Three grid sizes available: **3x3** (Tutorial), **6x6** (Warmup), and **9x9** (Standard).
* **Procedural Generation:** Custom **Backtracking Algorithm** generates unique, valid puzzles every time—no static arrays.
* **Real-Time Validation:** Instant visual feedback. Correct moves lock in (`+5 points`), while mistakes shake the cell (`-3 points`).
* **Cumulative Scoring:** Scores are **UPSERTED** (updated) to the database, meaning your total score grows the more you play.

### 🎨 UI/UX Design
* **Hand-Drawn Aesthetic:** Uses `feTurbulence` and `feDisplacementMap` SVG filters to create "wobbly" lines.
* **Responsive Keypad:** Adaptive controls that change based on grid size.
* **Visual Feedback:** Green flashes for successful saves, red shakes for errors.

### 🔐 Backend & Security (Supabase)
* **User Authentication:** Secure Email/Password login and signup flows.
* **Live Leaderboard:** Real-time fetching of top 10 global players.
* **Row Level Security (RLS):** Database policies ensure users can only update their own scores, preventing cheating.

## 🛠️ Tech Stack

* **Frontend:** HTML5, CSS3 (Grid/Flexbox), Vanilla JavaScript (ES6+)
* **Backend:** [Supabase](https://supabase.com/) (PostgreSQL)
* **Auth:** Supabase Auth (JWT)
* **Fonts:** Indie Flower (Google Fonts)

## 🚀 Getting Started

Follow these steps to run the project locally.

### Prerequisites
* A modern web browser.
* A [Supabase](https://supabase.com/) account (Free Tier).

### Installation

1.  **Clone the repo**
    ```sh
    git clone [https://github.com/yourusername/ranked-sudoku.git](https://github.com/yourusername/ranked-sudoku.git)
    ```
2.  **Open the project**
    Navigate to the project folder.
    ```sh
    cd ranked-sudoku
    ```
3.  **Configure Supabase**
    Open `index.html` and look for the configuration section. You need to replace the placeholder keys with your own:
    ```javascript
    const SUPABASE_URL = 'YOUR_SUPABASE_PROJECT_URL';
    const SUPABASE_KEY = 'YOUR_SUPABASE_ANON_KEY';
    ```
4.  **Run the App**
    Because of modern browser security policies (CORS), you cannot just double-click `index.html`. You must run it on a local server.
    * **VS Code:** Install "Live Server" extension -> Right Click `index.html` -> "Open with Live Server".
    * **Python:** `python -m http.server`

## 🗄️ Database Setup (SQL)

To make the leaderboard work, run the following SQL query in your Supabase **SQL Editor** to set up the table and security policies.

```sql
-- 1. Create the Scores Table
create table game_scores (
  user_id uuid references auth.users not null primary key,
  username text,
  score int default 0,
  time_taken int default 0,
  updated_at timestamp with time zone default now()
);

-- 2. Enable Row Level Security (RLS)
alter table game_scores enable row level security;

-- 3. Policy: Allow anyone to READ scores (for Leaderboard)
create policy "Public Read" 
on game_scores for select 
using ( true );

-- 4. Policy: Allow users to INSERT/UPDATE their own scores
create policy "User Actions" 
on game_scores for all 
using ( auth.uid() = user_id ) 
with check ( auth.uid() = user_id );

```
## 🖊️ AUTHOR 
* DEVANGSHU PANDEY (pandeydevangshu12)
