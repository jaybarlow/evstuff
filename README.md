Okay, I understand. You want the prompts generated without the ` ```text ... ``` ` Markdown code block formatting around each one.

My apologies, I included that formatting because the initial request mentioned "Each prompt should be tagged as text using code tags," which I interpreted as needing that specific Markdown structure.

Let me regenerate the prompts for you without that formatting, using headings and separators instead.

---

### Prompt 1: Initialize Supabase Project & Basic DB Schema

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Set up the initial Supabase project and define the core database schema according to the specification.
**Context:** This is the first step. No prior code exists.
**Task:**
1.  Provide the SQL statements to create the initial database tables in Supabase Postgres based on the specification document (Section 4: Data Model).
2.  Include tables: `user_settings` (with `user_id` as PK/FK to `auth.users`, `kelly_fraction`), `sports`, `events`, `markets`, `odds_history`.
3.  Ensure appropriate data types, primary keys (PK), foreign keys (FK), and basic indexing (e.g., on FKs and timestamps in `odds_history`).
4.  Make `user_settings.user_id` correctly reference `auth.users(id)` and enable Row Level Security (RLS) basics for `user_settings` (users can only access their own settings).
5.  Include default values where specified (e.g., `kelly_fraction`).
**Output:** A single SQL script containing `CREATE TABLE` statements and basic RLS policies for `user_settings`.

---

### Prompt 2: Set up Frontend Project (React/Vite) & Supabase Client

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Initialize a basic React frontend using Vite and set up the Supabase JavaScript client for interaction.
**Context:** Builds on Prompt 1 (Supabase project and DB schema exist).
**Task:**
1.  Provide commands to create a new React project using Vite with TypeScript.
2.  Show how to install the `@supabase/supabase-js` library.
3.  Create a Supabase client configuration file (`src/supabaseClient.ts`) that initializes the Supabase client using environment variables for the Supabase URL and Anon Key (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`).
4.  Modify the main application file (`src/App.tsx`) to demonstrate a basic check for the Supabase client initialization.
5.  Set up basic CSS or a UI library (like Tailwind CSS - optional, but recommended for consistency) for styling.
**Output:**
* Commands for project creation and dependency installation.
* Content for `src/supabaseClient.ts`.
* Modified content for `src/App.tsx` demonstrating client usage setup.
* Instructions on where to place Supabase credentials (e.g., `.env.local` file).

---

### Prompt 3: Implement Basic Auth UI & Logic

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement user sign-up, login, and logout functionality using Supabase Auth within the React frontend.
**Context:** Builds on Prompt 2 (React app with Supabase client setup). Assumes Supabase Email/Password provider is enabled.
**Task:**
1.  Create React components for:
    * `AuthForm`: A component allowing users to toggle between Sign Up and Login modes, taking email/password input.
    * `UserProfile`: A simple component displaying the logged-in user's email and a Logout button.
2.  In `App.tsx`, manage the user's session state using Supabase Auth listeners (`onAuthStateChange`).
3.  Conditionally render `AuthForm` if the user is not logged in, and `UserProfile` (along with future app content placeholders) if they are logged in.
4.  Implement the `signUp`, `signInWithPassword`, and `signOut` functions using the Supabase client instance.
5.  Include basic error handling and display feedback to the user (e.g., "Invalid login credentials").
**Output:**
* Content for `src/components/AuthForm.tsx`.
* Content for `src/components/UserProfile.tsx`.
* Updated content for `src/App.tsx` managing session state and rendering components conditionally.
* Helper functions for Supabase auth interactions.

---

### Prompt 4: Create `user_settings` UI (Frontend Only)

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create a simple UI component within the React app to display and allow editing of the user's preferred Kelly fraction (initially without backend connection).
**Context:** Builds on Prompt 3 (User can log in). The `user_settings` table exists in Supabase (from Prompt 1).
**Task:**
1.  Create a new React component `UserSettings.tsx`.
2.  Inside `UserSettings`, add a controlled form element (e.g., a select dropdown or radio buttons) allowing the user to choose a Kelly fraction (e.g., 1.0 for Full, 0.5 for Half, 0.25 for Quarter).
3.  Use React state (`useState`) within the component to manage the selected Kelly fraction.
4.  Display the currently selected value.
5.  Add a "Save Settings" button (it won't do anything yet).
6.  Include this `UserSettings` component within the authenticated view in `App.tsx` (e.g., alongside `UserProfile`).
**Output:**
* Content for `src/components/UserSettings.tsx`.
* Updated content for `src/App.tsx` to include the `UserSettings` component when logged in.

---

### Prompt 5: Connect Settings UI to Supabase

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Connect the `UserSettings` component to Supabase to load and save the user's preferred Kelly fraction to the `user_settings` table.
**Context:** Builds on Prompt 4 (UserSettings UI exists) and Prompt 1 (DB table and RLS exist).
**Task:**
1.  Modify `UserSettings.tsx`:
    * On component mount (using `useEffect`), fetch the current user's settings from the `user_settings` table in Supabase. Use the logged-in user's ID (available from Supabase Auth session). Handle cases where no settings exist yet (use a default value, e.g., 1.0).
    * Populate the form with the fetched or default value.
    * Implement the "Save Settings" button functionality:
        * Use Supabase client's `.upsert()` method to insert or update the user's record in the `user_settings` table with the selected `kelly_fraction`.
        * Provide user feedback on success or failure (e.g., simple alerts or status messages).
    * Ensure RLS policies defined in Prompt 1 allow this operation.
**Output:**
* Updated content for `src/components/UserSettings.tsx` including Supabase fetch/save logic and `useEffect`.

---

### Prompt 6: Create Supabase Edge Function Structure

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Set up the basic file structure and configuration for a Supabase Edge Function that will later handle scraping and calculations.
**Context:** Builds on Prompt 1 (Supabase project exists). Assumes Supabase CLI is installed and user is logged in.
**Task:**
1.  Provide Supabase CLI commands to create a new Edge Function named `Workspace-and-store-odds`.
2.  Show the basic file structure generated (`supabase/functions/fetch-and-store-odds/index.ts`).
3.  Provide the initial content for `index.ts`, including:
    * Importing necessary types from `@supabase/functions-js`.
    * Setting up the basic Supabase client *within* the function using environment variables (demonstrate accessing `SUPABASE_URL`, `SUPABASE_ANON_KEY`, and importantly `SUPABASE_SERVICE_ROLE_KEY` for backend operations).
    * A basic function handler that logs a message (e.g., "Fetch-and-store-odds function started").
    * Basic CORS header setup.
**Output:**
* Supabase CLI commands.
* Initial content for `supabase/functions/fetch-and-store-odds/index.ts`.

---

### Prompt 7: Define Mock Data Structures for APIs

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Define TypeScript interfaces representing the *expected* structure of JSON data returned by the Pinnacle and CrabSports APIs (based on manual reverse engineering). This is for mocking purposes initially.
**Context:** Builds on Prompt 6 (Edge Function structure exists). This step simulates the outcome of manual API investigation.
**Task:**
1.  Create TypeScript interfaces for *simplified* mock API responses. Assume the APIs provide lists of games/events, each with associated markets (Moneyline, Spread, Total) and odds.
2.  Define interfaces like:
    * `PinnacleOddsResponse`, `PinnacleEvent`, `PinnacleMarket`, `PinnacleOdds`.
    * `CrabSportsOddsResponse`, `CrabSportsEvent`, `CrabSportsMarket`, `CrabSportsOdds`.
3.  Focus on capturing key fields needed for the app: Sport, Event details (Teams, Start Time), Market type (ML, Spread, Total), Line value (for Spread/Total), and the actual odds for each outcome (e.g., Team A odds, Team B odds, Over odds, Under odds). Use American or Decimal odds as appropriate, but plan to convert to Decimal for calculations.
4.  Place these interfaces in a shared types file or directly within the Edge Function `index.ts` for now.
**Output:** TypeScript code defining the interfaces for mock API responses.

---

### Prompt 8: Implement Mock Scraper Functions

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement functions within the Edge Function that *simulate* fetching data from Pinnacle and CrabSports by returning hardcoded mock data conforming to the interfaces defined in Prompt 7.
**Context:** Builds on Prompt 7 (Mock data interfaces exist) and Prompt 6 (Edge Function exists).
**Task:**
1.  Inside `supabase/functions/fetch-and-store-odds/index.ts`, create two async functions:
    * `WorkspacePinnacleMockOdds(): Promise<PinnacleOddsResponse>`
    * `WorkspaceCrabSportsMockOdds(): Promise<CrabSportsOddsResponse>`
2.  Implement these functions to return hardcoded sample data matching the interfaces from Prompt 7. Include examples for NBA, MLB, or NHL, covering Moneyline, Spread, and Total markets. Ensure the structure is realistic enough for testing the next steps.
3.  Modify the main Edge Function handler to call these mock functions and log the received mock data.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` including the implementation of the two mock fetching functions and invocation in the main handler.

---

### Prompt 9: Implement Logic to Insert Mock Data into `odds_history`

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add logic to the Edge Function to process the mock data fetched in Prompt 8 and insert it into the `odds_history` table in the Supabase database.
**Context:** Builds on Prompt 8 (Mock data fetching) and Prompt 1 (DB schema).
**Task:**
1.  Inside `supabase/functions/fetch-and-store-odds/index.ts`, after calling the mock fetch functions:
    * Add data transformation logic to map the structure of the mock API responses (Pinnacle and CrabSports) to the structure of the `odds_history` table.
    * This involves identifying or creating corresponding records in `sports`, `events`, and `markets` tables first (use `.upsert()` based on unique identifiers like event names/times and market types/lines). Handle potential errors during these lookups/inserts.
    * Convert odds from the API format (e.g., American) to Decimal format if necessary before insertion.
    * Construct an array of `odds_history` records containing `market_id` (from the previous step), `bookmaker` ('Pinnacle' or 'CrabSports'), the decimal odds fields (`odds_a`, `odds_b`, `odds_over`, `odds_under` as applicable), and a current `timestamp`.
    * Use the Supabase client (initialized with the service role key) to perform a bulk `.insert()` into the `odds_history` table.
    * Log success or errors during the insertion process.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` with logic for data transformation and insertion into `sports`, `events`, `markets`, and `odds_history` using mock data.

---

### Prompt 10: Add Basic Error Handling to Mock Scraping/Storage

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add basic try/catch blocks and logging around the mock data fetching and database insertion steps within the Edge Function.
**Context:** Builds on Prompt 9 (Edge Function inserts mock data).
**Task:**
1.  Wrap the calls to the mock fetch functions (`WorkspacePinnacleMockOdds`, `WorkspaceCrabSportsMockOdds`) within try/catch blocks. Log any errors.
2.  Wrap the database operations (upserting events/markets, inserting odds history) within try/catch blocks. Log any errors encountered during DB interaction.
3.  Ensure the function returns an appropriate response (e.g., status 200 on success, status 500 on critical failure) with a meaningful message or error details.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` incorporating try/catch blocks and logging for error handling.

---

### Prompt 11: Implement Vig Removal Helper Functions

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement the three specified vig removal methods as helper functions within the Edge Function environment.
**Context:** Builds on Prompt 10 (Edge Function structure with error handling). These functions will operate on odds data. Assume odds are provided in Decimal format.
**Task:**
1.  Inside `supabase/functions/fetch-and-store-odds/index.ts` (or potentially a separate helper file imported by it):
    * Create a function `calculateImpliedProbability(decimalOdds: number): number`. Formula: `1 / decimalOdds`.
    * Create a function `removeVigAdditive(odds: number[]): number[]` which takes an array of decimal odds for all outcomes in a market, calculates implied probabilities, finds the overround (`sum(probabilities) - 1`), subtracts an equal portion of the overround from each probability, normalizes, and returns the 'true' probabilities.
    * Create a function `removeVigMultiplicative(odds: number[]): number[]` which calculates implied probabilities, finds the sum, and divides each probability by the sum to get 'true' probabilities.
    * Create a function `removeVigShin(odds: number[]): number[]` which implements the Shin method. This involves finding a `z` value such that the sum of `(impliedProbability / (1 - z + z * impliedProbability))` equals 1. This likely requires an iterative approach (e.g., binary search) to find `z`. Return the resulting 'true' probabilities.
    * Ensure functions handle edge cases (e.g., invalid odds, two-way vs three-way markets if necessary, although spec focuses on 2-way for now).
**Output:** TypeScript code for the vig removal helper functions (`calculateImpliedProbability`, `removeVigAdditive`, `removeVigMultiplicative`, `removeVigShin`).

---

### Prompt 12: Implement 'Worst-Case' True Odds Calculation

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement the logic to calculate the 'worst-case' true odds for a market using the vig removal functions created in Prompt 11.
**Context:** Builds on Prompt 11 (Vig removal functions exist).
**Task:**
1.  Create a function `calculateWorstCaseTrueOdds(pinnacleOdds: number[]): { trueOdds: number[], trueProbabilities: number[] }`. This function takes an array of Pinnacle decimal odds for a market (e.g., `[odds_a, odds_b]` or `[odds_over, odds_under]`).
2.  Inside this function:
    * Call each of the three vig removal methods (`removeVigAdditive`, `removeVigMultiplicative`, `removeVigShin`) with the input Pinnacle odds to get three sets of 'true' probabilities.
    * For *each outcome* in the market (e.g., outcome A, outcome B), determine the *highest* probability assigned to it across the three methods. These highest probabilities form the 'worst-case' probability set.
    * **Normalize** these 'worst-case' probabilities so they sum to 1. This is crucial.
    * Convert these final normalized 'worst-case' probabilities back into 'true decimal odds' (Formula: `1 / probability`).
    * Return both the final 'true odds' array and the corresponding 'true probabilities' array.
3.  Handle potential errors or invalid inputs gracefully.
**Output:** TypeScript code for the `calculateWorstCaseTrueOdds` function.

---

### Prompt 13: Implement EV Calculation Function

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement a function to calculate the Expected Value (EV) percentage.
**Context:** Builds on Prompt 12 ('Worst-case' true odds/probabilities can be calculated).
**Task:**
1.  Create a function `calculateEVPercentage(offeredDecimalOdds: number, trueDecimalOdds: number): number`.
2.  Implement the formula: `EV % = ((offeredDecimalOdds / trueDecimalOdds) - 1) * 100`.
3.  Ensure it handles potential division by zero or invalid inputs (return NaN or throw an error).
**Output:** TypeScript code for the `calculateEVPercentage` function.

---

### Prompt 14: Implement Kelly Criterion Calculation Function

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement a function to calculate the Kelly Criterion stake fraction.
**Context:** Builds on Prompt 12 ('Worst-case' true probabilities can be calculated).
**Task:**
1.  Create a function `calculateKellyFraction(offeredDecimalOdds: number, trueProbability: number): number`.
2.  Implement the Full Kelly formula: `f = ((offeredDecimalOdds - 1) * trueProbability - (1 - trueProbability)) / (offeredDecimalOdds - 1)`.
    * Note: `(offeredDecimalOdds - 1)` is the 'Net Odds' or 'b' in the common `(bp - q) / b` formula.
    * `trueProbability` is 'p'.
    * `(1 - trueProbability)` is 'q'.
3.  Handle edge cases:
    * If `offeredDecimalOdds <= 1`, return 0 (or handle error).
    * If the calculated fraction `f` is less than or equal to 0, return 0 (indicating no bet).
    * Ensure `trueProbability` is between 0 and 1.
**Output:** TypeScript code for the `calculateKellyFraction` function.

---

### Prompt 15: Integrate Calculations into Edge Function Flow

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Modify the main Edge Function (`Workspace-and-store-odds`) to use the calculation functions after storing odds data.
**Context:** Builds on Prompt 9 (Stores mock data), Prompt 12, 13, 14 (Calculation functions exist). *Note: This step doesn't store the calculated values yet, just performs the calculation as part of the flow.*
**Task:**
1.  Modify the main handler in `supabase/functions/fetch-and-store-odds/index.ts`.
2.  After successfully inserting the latest odds into `odds_history` (for both Pinnacle and CrabSports for a given market):
    * Retrieve the *most recent* Pinnacle odds for that specific market from the data just inserted or by querying `odds_history`.
    * Retrieve the *most recent* CrabSports odds for the same market.
    * Pass the Pinnacle odds array to `calculateWorstCaseTrueOdds` (from Prompt 12) to get the `trueProbabilities` and `trueOdds`.
    * For each outcome offered by CrabSports on that market:
        * Use the corresponding `trueProbability` and the `offeredDecimalOdds` from CrabSports to calculate the `kellyFraction` using `calculateKellyFraction` (from Prompt 14).
        * Use the corresponding `trueDecimalOdds` and the `offeredDecimalOdds` from CrabSports to calculate the `evPercentage` using `calculateEVPercentage` (from Prompt 13).
    * Log the calculated results (True Odds, EV%, Kelly Fraction) for demonstration purposes. *Do not store these results persistently yet.*
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` integrating the calculation steps after data insertion and logging the results.

---

### Prompt 16: Add Logic to Identify +EV Opportunities

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Filter the calculated results within the Edge Function to identify only those opportunities with a positive Expected Value (EV > 0%).
**Context:** Builds on Prompt 15 (Calculations are performed).
**Task:**
1.  Modify the logic in `supabase/functions/fetch-and-store-odds/index.ts` where EV is calculated.
2.  After calculating `evPercentage` for a specific bet opportunity (e.g., Team A ML at CrabSports odds):
    * Check if `evPercentage > 0`.
    * If it is positive, store this opportunity (including details like Event, Market, Bet, Bookmaker, Offered Odds, True Odds, EV%, Kelly Fraction) temporarily in an array within the function's execution context.
3.  At the end of processing all markets, log the array of identified +EV opportunities.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` showing the identification and temporary collection of +EV opportunities.

---

### Prompt 17: Create Supabase RPC for Fetching +EV Data

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create a Supabase Database Function (RPC) that the frontend can call to get the latest identified +EV opportunities. *Initially, this will simulate the calculation based on recent history, as we aren't storing EV results persistently yet.*
**Context:** Builds on Prompt 16 (Logic exists to identify +EV), Prompt 1 (DB Schema), Prompts 11-14 (Calculation functions - need to be accessible or reimplemented in SQL/plpgsql or called via Edge Function trigger if complex). *Let's opt for a simpler SQL-based approach for the RPC, potentially recalculating based on latest odds history.*
**Task:**
1.  Provide SQL code to create a Supabase database function `get_positive_ev_opportunities()`.
2.  This function should:
    * Query the `odds_history` table to find the *latest* Pinnacle and CrabSports odds for *active* markets (markets associated with future events in the `events` table). Group by market.
    * *Simulate* or perform the True Odds, EV, and Kelly calculations within the SQL function (or call other helper SQL functions if needed). *Simplification: For this RPC, maybe just return markets where CrabSports odds are better than Pinnacle raw odds as a placeholder for true EV calculation.* -- **Correction:** Let's make it slightly more realistic. The RPC should try to fetch the latest Pin odds, latest Crab odds for each market, calculate simplified True Odds (e.g., multiplicative vig removal), then calculate EV, and return rows where EV > 0.
    * The function should return a set of records, each representing a +EV opportunity, containing columns specified in the spec (Section 3.6: Sport, Event, Market, Bet, Bookmaker='CrabSports', Odds, Pinnacle True Odds [calculated], EV%, Kelly Stake [calculated Full Kelly]).
    * Ensure the function handles data type conversions (e.g., odds to numeric) and joins correctly between `events`, `markets`, `sports`, and `odds_history`.
    * Consider performance: Limit the query to recent odds or events starting soon.
**Output:** SQL script to create the `get_positive_ev_opportunities` PL/pgSQL function. Include necessary helper functions if calculations are done in SQL. *Alternatively, specify that this RPC should trigger the `Workspace-and-store-odds` Edge Function and retrieve results from a temporary table or cache if SQL calculation is too complex - however, the spec leans towards calculation within the function/backend.* Let's stick to SQL calculation for this prompt, perhaps using only multiplicative vig removal for simplicity within SQL.

---

### Prompt 18: Create Frontend Table Component for +EV Opportunities

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create a React component to fetch data from the `get_positive_ev_opportunities` RPC (created in Prompt 17) and display it in a table.
**Context:** Builds on Prompt 17 (RPC exists), Prompt 3 (Auth setup), Prompt 2 (Supabase client).
**Task:**
1.  Create a new React component `EvOpportunitiesTable.tsx`.
2.  Inside the component:
    * Use `useEffect` to call the Supabase RPC `get_positive_ev_opportunities` when the component mounts (or when relevant filters change later).
    * Store the fetched data in the component's state (`useState`).
    * Handle loading and error states during the fetch.
    * Render the fetched data in an HTML table.
    * Include columns as specified in the spec (Section 3.6): Sport, Event (Team A vs Team B, Start Time), Market, Bet, Bookmaker, Odds (Decimal), Pinnacle True Odds (Decimal), EV %, Kelly Stake. Format data appropriately (e.g., dates, percentages).
    * Initially, calculate and display the Full Kelly stake (Kelly Fraction * 100%).
3.  Add this `EvOpportunitiesTable` component to the authenticated view in `App.tsx`.
**Output:**
* Content for `src/components/EvOpportunitiesTable.tsx`.
* Updated content for `src/App.tsx` to include the table.

---

### Prompt 19: Integrate User's Kelly Setting into Frontend Table

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Modify the `EvOpportunitiesTable` component to use the user's saved Kelly fraction preference (from `UserSettings`) when calculating the displayed Kelly Stake.
**Context:** Builds on Prompt 18 (Table displays EV ops), Prompt 5 (User settings are saved/loaded).
**Task:**
1.  Modify `EvOpportunitiesTable.tsx`:
    * Fetch the user's saved `kelly_fraction` from their settings (either pass it down as a prop from `App.tsx` where session/settings state might be managed, or fetch it directly within the component if necessary - passing props is often cleaner). Assume the value (1.0, 0.5, 0.25) is available.
    * When displaying the "Kelly Stake" column, use the `kellyFraction` returned by the RPC (which should be the Full Kelly fraction) and multiply it by the user's preferred fraction (1.0, 0.5, or 0.25) before formatting it for display (e.g., as a percentage).
    * Ensure a default (e.g., Full Kelly, factor 1.0) is used if the user setting is unavailable.
**Output:** Updated content for `src/components/EvOpportunitiesTable.tsx` showing the adjustment to the Kelly Stake calculation based on user preference. May require minor adjustments to parent components (`App.tsx`) to pass down the setting.

---

### Prompt 20: Add Filter UI Controls to Frontend

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add UI elements (inputs, dropdowns) to the frontend above or near the `EvOpportunitiesTable` for filtering the displayed data.
**Context:** Builds on Prompt 18 (Table exists).
**Task:**
1.  Modify the component containing `EvOpportunitiesTable` (or create a new parent component `EvFinderPage.tsx`):
    * Add UI controls for:
        * Sport (multi-select checkboxes or dropdown - fetch available sports from `sports` table or hardcode initially: NBA, MLB, NHL).
        * Min EV % (numeric input).
        * Odds Range (min/max numeric inputs).
        * Event Timeframe (dropdown/radio: e.g., "Next 24h", "Next 3d", "All").
    * Use React state (`useState`) to manage the current values of these filter controls.
    * Do not implement the filtering logic yet, just the UI controls and their state management.
**Output:** Updated React component code showing the added filter UI controls and their associated state hooks.

---

### Prompt 21: Implement Client-Side Filtering Logic

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement logic within the React component to filter the EV opportunities data (fetched from the RPC) based on the user's selections in the filter UI controls.
**Context:** Builds on Prompt 20 (Filter UI controls exist) and Prompt 18 (Data is fetched and displayed).
**Task:**
1.  Modify the component managing the filters and the table (`EvFinderPage.tsx` or similar).
2.  After fetching the data from the Supabase RPC, apply filtering logic *before* rendering the `EvOpportunitiesTable`.
3.  Use the state variables holding the filter values (Sport, Min EV%, Odds Range, Timeframe) to filter the array of opportunities.
    * Implement filtering for each criterion.
    * For timeframe, parse the event `start_time` and compare it against the current time plus the selected duration.
    * Pass the *filtered* data array as a prop to `EvOpportunitiesTable`.
4.  Ensure the filtering logic is triggered whenever a filter value changes. Use `useMemo` or similar hooks to optimize recalculation if needed.
**Output:** Updated React component code showing the client-side filtering logic applied to the data before passing it to the table.

---

### Prompt 22: Implement Client-Side Sorting Logic

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add functionality to the `EvOpportunitiesTable` to allow users to sort the displayed data by clicking on column headers.
**Context:** Builds on Prompt 21 (Filtered data is displayed in the table).
**Task:**
1.  Modify `EvOpportunitiesTable.tsx`:
    * Add state variables to track the current sort column (`sortKey`) and sort direction (`sortDirection`: 'asc' or 'desc').
    * Make the relevant table headers (`<th>`) clickable. Add `onClick` handlers to them.
    * When a header is clicked, update the `sortKey` and `sortDirection` state. If the same key is clicked, toggle the direction; otherwise, set direction to default (e.g., 'desc' for EV%, 'asc' for Start Time).
    * Implement a sorting function that takes the (filtered) data array and the sort state, and returns a sorted array. Handle different data types (numbers, strings, dates).
    * Apply this sorting function to the data *before* mapping it to table rows (`<tr>`). `useMemo` is recommended here to avoid resorting on every render.
    * Visually indicate the current sort column and direction (e.g., add an arrow icon to the header).
**Output:** Updated content for `src/components/EvOpportunitiesTable.tsx` including state, click handlers, sorting logic, and UI indicators for sorting.

---

### Prompt 23: Add Filter Columns to `user_settings` Table

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add new columns to the `user_settings` table in Supabase to store the user's preferred filter settings.
**Context:** Builds on Prompt 1 (DB Schema exists).
**Task:**
1.  Provide the SQL `ALTER TABLE` statements to add the following columns to the `user_settings` table:
    * `min_ev_percentage` (NUMERIC, nullable, default 0)
    * `min_odds` (NUMERIC, nullable)
    * `max_odds` (NUMERIC, nullable)
    * `event_timeframe_hours` (INTEGER, nullable, e.g., 24, 72)
    * `sports_filter` (TEXT[], nullable - PostgreSQL array to store selected sport names like `{'NBA', 'MLB'}`)
        * *Alternative:* JSONB if more complex sport filtering is anticipated. TEXT[] is simpler for now.
2.  Ensure the RLS policies still allow users to update their own settings.
**Output:** SQL script containing the `ALTER TABLE` statements.

---

### Prompt 24: Implement Filter Settings UI Management

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create UI elements for managing the new filter settings and connect them to load/save these settings from/to the `user_settings` table.
**Context:** Builds on Prompt 23 (DB table updated), Prompt 5 (Basic settings save/load exists).
**Task:**
1.  Modify the `UserSettings.tsx` component (or create a separate `FilterSettings.tsx` component):
    * Add form fields corresponding to the new columns: Min EV%, Min Odds, Max Odds, Event Timeframe (dropdown), Sports Filter (checkbox group or multi-select).
    * On component mount, extend the existing logic to fetch these *additional* filter settings along with the Kelly fraction from the user's record in `user_settings`. Populate the form fields with fetched values or defaults.
    * Modify the "Save Settings" functionality to also save the values from these new form fields into the corresponding columns in the `user_settings` table using `.upsert()`. Handle potential null values appropriately.
    * Provide user feedback on save success/failure.
**Output:** Updated React component code (`UserSettings.tsx` or new `FilterSettings.tsx`) showing the UI and Supabase interaction for managing filter preferences.

---

### Prompt 25: Connect Filter UI State Saving/Loading

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Connect the main filter UI (from Prompt 20) to use the saved filter preferences (from Prompt 24) as initial defaults and potentially offer a "Save Filters" button.
**Context:** Builds on Prompt 24 (Filter settings can be saved/loaded to user profile), Prompt 20 (Filter UI exists), Prompt 21 (Client-side filtering logic exists).
**Task:**
1.  Modify the component managing the filters (`EvFinderPage.tsx` or similar):
    * When the component mounts and the user is logged in, fetch their saved filter preferences from `user_settings` (similar to how `UserSettings` does it, or ideally, read from a shared state/context if settings are loaded centrally in `App.tsx`).
    * Use these fetched preferences to set the *initial* state of the filter UI controls (Min EV%, Odds Range, Timeframe, Sports). Use defaults if no saved preferences exist.
    * *(Optional but good UX)*: Add a "Save Current Filters" button near the filter controls that triggers the same Supabase upsert logic used in `UserSettings` (or calls a shared save function) to persist the current filter UI state to the user's profile.
    * Ensure the client-side filtering logic (Prompt 21) uses the current state of the filter UI controls.
**Output:** Updated React component code (`EvFinderPage.tsx` or similar) showing loading of default filter values from user settings and the optional "Save Current Filters" functionality.

---

### Prompt 26: Integrate Charting Library

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Add a JavaScript charting library (Chart.js) to the React frontend project.
**Context:** Builds on Prompt 2 (React project setup).
**Task:**
1.  Provide commands to install `chart.js` and `react-chartjs-2`.
2.  Demonstrate a basic setup by creating a simple, non-functional chart component (`SimpleChart.tsx`) that imports `Line` from `react-chartjs-2` and sets up the basic options and data structure for a line chart, perhaps with static sample data.
3.  Include this `SimpleChart` temporarily somewhere in the app (e.g., `App.tsx`) just to verify the library integration works.
**Output:**
* NPM/Yarn install commands.
* Content for `src/components/SimpleChart.tsx`.
* Minimal changes to `App.tsx` or similar to render the `SimpleChart`.

---

### Prompt 27: Create Supabase RPC for Market Odds History

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create a Supabase Database Function (RPC) to efficiently query the `odds_history` table for a specific market ID, returning data suitable for plotting.
**Context:** Builds on Prompt 1 (DB Schema exists), Prompt 9 (Data is being inserted, albeit mock initially).
**Task:**
1.  Provide SQL code to create a Supabase database function `get_market_odds_history(p_market_id BIGINT)`.
2.  This function should:
    * Take a `market_id` as input.
    * Query the `odds_history` table, filtering by the provided `p_market_id`.
    * Select relevant columns: `timestamp`, `bookmaker`, and the specific odds columns relevant to that market type (e.g., `odds_a`, `odds_b` for Moneyline/Spread; `odds_over`, `odds_under` for Totals). *This might require a dynamic approach or separate functions per market type if the schema stores odds differently, but assume unified columns for now.*
    * Order the results by `timestamp` ascending.
    * Consider adding a `LIMIT` clause or filtering by `timestamp` (e.g., last 7 days) to keep the payload size reasonable.
    * Return the results as a JSON array or set of records.
**Output:** SQL script to create the `get_market_odds_history` PL/pgSQL function.

---

### Prompt 28: Create Frontend Chart Component

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Create a dedicated React component that uses `react-chartjs-2` to display the odds history for a market, fetching data using the RPC from Prompt 27.
**Context:** Builds on Prompt 26 (Charting library integrated), Prompt 27 (RPC for history data exists).
**Task:**
1.  Create a new React component `OddsHistoryChart.tsx`.
2.  The component should accept a `marketId` as a prop.
3.  Use `useEffect` to trigger a data fetch when `marketId` changes (and is valid).
    * Call the Supabase RPC `get_market_odds_history` with the `marketId`.
    * Handle loading and error states.
4.  Process the fetched data:
    * Separate the data points for Pinnacle and CrabSports.
    * Format the data into the structure required by `react-chartjs-2`'s `Line` component (labels: timestamps, datasets: one for Pinnacle odds, one for CrabSports odds). Handle the different odds types (e.g., plot Team A odds, Over odds depending on market).
5.  Configure the `Line` chart options (axis labels, tooltips, colors, etc.).
6.  Render the `Line` chart with the processed data. Display loading/error messages as needed.
**Output:** Content for `src/components/OddsHistoryChart.tsx`.

---

### Prompt 29: Add Interaction to Trigger Chart Display

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Modify the `EvOpportunitiesTable` to allow users to click on a row (or a specific button/icon in a row) to display the `OddsHistoryChart` for that specific market, potentially in a modal dialog or an expandable section.
**Context:** Builds on Prompt 28 (Chart component exists), Prompt 22 (Table component exists).
**Task:**
1.  Modify `EvOpportunitiesTable.tsx`:
    * Add state to manage the `selectedMarketId` for which to show the chart (initially null).
    * Add a clickable element (e.g., the entire row `<tr>`, or a dedicated icon/button within a cell) to each row representing an EV opportunity.
    * The click handler should update the `selectedMarketId` state with the `market_id` associated with that row's opportunity.
2.  Conditionally render the `OddsHistoryChart` component somewhere accessible (e.g., below the table, or in a modal triggered by the state change).
    * Pass the `selectedMarketId` state variable as a prop to `OddsHistoryChart`.
    * Ensure the chart is only rendered or fetched when `selectedMarketId` is not null.
    * Add a way to close or hide the chart (e.g., clicking the row again, a close button, clicking outside the modal) by setting `selectedMarketId` back to null.
    * Consider using a modal library (like `react-modal`) for better UX.
**Output:** Updated content for `src/components/EvOpportunitiesTable.tsx` (and potentially its parent) showing the row click interaction and conditional rendering of the `OddsHistoryChart`. Include modal implementation if used.

---

### Prompt 30: Replace Mock Scrapers with Actual HTTP Fetch Logic (Pinnacle)

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Replace the mock Pinnacle data fetching function in the Edge Function with actual HTTP requests based on manually reverse-engineered API details.
**Context:** Builds on Prompt 8 (Mock function exists), Prompt 10 (Error handling structure exists). *Crucially assumes the developer has manually determined the correct Pinnacle API endpoint URL, necessary HTTP headers (User-Agent, Authorization, Cookies, etc.), and the structure of the JSON response.*
**Task:**
1.  Modify the `WorkspacePinnacleMockOdds` function in `supabase/functions/fetch-and-store-odds/index.ts`. Rename it to `WorkspacePinnacleOdds`.
2.  Replace the mock data return with actual `Workspace` API calls.
    * Use the determined Pinnacle API endpoint URL.
    * Construct the necessary `headers` object, including User-Agent, and any required authentication tokens or cookies. **Store sensitive tokens/keys securely using Supabase Secrets**, accessed via `Deno.env.get()`, do not hardcode them.
    * Handle the asynchronous response: check for successful status codes (e.g., 200 OK). Throw an error for non-successful responses (4xx, 5xx).
    * Parse the JSON response body.
    * **Crucially:** Validate the structure of the received JSON against the expected `PinnacleOddsResponse` interface (or a more refined interface based on actual data). Use a validation library (like Zod) or manual checks. If validation fails, throw an error.
    * Return the validated data.
3.  Update the main function handler to call `WorkspacePinnacleOdds` instead of the mock version.
4.  Enhance the try/catch block around this call to handle specific errors (network errors, HTTP status errors, JSON parsing errors, validation errors) and log informative messages. Implement basic retry logic (e.g., simple retry once on network failure) if desired.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` with the real `WorkspacePinnacleOdds` implementation, secret handling, response validation, and enhanced error handling. Include comments indicating where placeholders (URL, headers) need to be filled in.

---

### Prompt 31: Replace Mock Scrapers with Actual HTTP Fetch Logic (CrabSports)

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Replace the mock CrabSports data fetching function in the Edge Function with actual HTTP requests based on manually reverse-engineered API details.
**Context:** Builds on Prompt 30 (Real Pinnacle fetch implemented). *Crucially assumes the developer has manually determined the correct CrabSports API endpoint URL, necessary HTTP headers, and the structure of the JSON response.*
**Task:**
1.  Modify the `WorkspaceCrabSportsMockOdds` function in `supabase/functions/fetch-and-store-odds/index.ts`. Rename it to `WorkspaceCrabSportsOdds`.
2.  Replace the mock data return with actual `Workspace` API calls, following the same principles as in Prompt 30:
    * Use the determined CrabSports API endpoint URL.
    * Construct necessary `headers`. Use Supabase Secrets for any sensitive values.
    * Handle the response (status codes, JSON parsing).
    * Validate the received JSON against the expected `CrabSportsOddsResponse` interface (or refined version). Use Zod or manual checks. Throw errors on failure.
    * Return the validated data.
3.  Update the main function handler to call `WorkspaceCrabSportsOdds`.
4.  Enhance the try/catch block for this call with specific error handling and logging.
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` with the real `WorkspaceCrabSportsOdds` implementation, secret handling, response validation, and enhanced error handling. Include comments for placeholders.

---

### Prompt 32: Implement Robust Error Handling & Logging in Edge Function

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Enhance the overall error handling and logging within the `Workspace-and-store-odds` Edge Function to be more robust and informative.
**Context:** Builds on Prompts 30 & 31 (Real fetching with basic try/catch).
**Task:**
1.  Review the entire `Workspace-and-store-odds` Edge Function (`index.ts`).
2.  Implement more specific error catching:
    * Distinguish between scraping errors (Pinnacle vs. CrabSports), data validation errors, database insertion errors, and calculation errors.
    * For scraping errors: Log the target bookmaker, URL (if possible without leaking secrets), status code (if applicable), and error message. Consider logging response body snippets on parsing/validation errors.
    * For database errors: Log the operation attempted (e.g., insert into `odds_history`), the data involved (partially, avoid overly verbose logs), and the Supabase error message.
    * For calculation errors: Log the inputs that caused the error (e.g., odds that led to division by zero).
3.  Standardize log messages (e.g., prefix with `[INFO]`, `[WARN]`, `[ERROR]`).
4.  Ensure the function aims to complete as much work as possible. E.g., if Pinnacle scraping fails but CrabSports succeeds, still attempt to store CrabSports odds (though calculations needing both will fail). If one market fails during processing, try to continue with the next market.
5.  Return a meaningful summary in the function's final response (e.g., number of markets processed, number of errors encountered, list of specific critical failures).
**Output:** Updated content for `supabase/functions/fetch-and-store-odds/index.ts` with significantly improved, more granular error handling and logging throughout the scraping, validation, storage, and calculation phases.

---

### Prompt 33: Set up Supabase Scheduled Function

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Configure a Supabase scheduled function to invoke the `Workspace-and-store-odds` Edge Function at a regular interval (e.g., every 5 minutes).
**Context:** Builds on Prompt 32 (Edge Function `Workspace-and-store-odds` is implemented). Assumes Supabase CLI is set up.
**Task:**
1.  Provide the necessary Supabase CLI command (`supabase functions schedule`) or SQL statement using `pg_cron` to schedule the `Workspace-and-store-odds` Edge Function.
2.  Set the schedule using cron syntax (e.g., `*/5 * * * *` for every 5 minutes).
3.  Explain how to deploy this schedule using the Supabase CLI or manage it via the Supabase Dashboard.
4.  Briefly mention how to view logs for scheduled function invocations in the Supabase Dashboard.
**Output:**
* Supabase CLI command or SQL statement for scheduling.
* Explanation of deployment and monitoring.

---

### Prompt 34: Implement Basic Data Purging Logic

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Implement logic to periodically delete old data from the `odds_history` table to prevent excessive growth.
**Context:** Builds on Prompt 1 (DB Schema), Prompt 33 (Scheduling is possible).
**Task:**
1.  Provide SQL code to create a Supabase database function `purge_old_odds_history()`.
2.  This function should:
    * Define a retention period (e.g., 7 days, 30 days).
    * Delete records from the `odds_history` table where the `timestamp` is older than the current time minus the retention period.
    * Log the number of rows deleted (e.g., using `RAISE NOTICE`).
    * Handle potential locking issues or run duration by deleting in batches if necessary (though likely not needed initially unless the table is huge).
3.  Provide the Supabase CLI command or SQL statement using `pg_cron` to schedule this `purge_old_odds_history` function to run periodically (e.g., once daily `0 3 * * *` - at 3 AM).
**Output:**
* SQL script to create the `purge_old_odds_history` function.
* Supabase CLI command or SQL statement to schedule the purge function.

---

### Prompt 35: Write Unit Tests for Calculation Functions

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Write unit tests for the core calculation helper functions (Vig Removal, True Odds, EV, Kelly).
**Context:** Builds on Prompts 11, 12, 13, 14 (Calculation functions exist). Assumes a testing framework like `jest` or Deno's built-in tester is set up.
**Task:**
1.  Provide example unit test code (using Jest syntax or Deno standard library) for:
    * `calculateImpliedProbability`: Test with known odds.
    * `removeVigMultiplicative`: Test with known inputs and expected outputs.
    * `removeVigAdditive`: Test with known inputs and expected outputs.
    * `removeVigShin`: Test with known inputs (might be harder to get exact expected output, test for probability sum ≈ 1).
    * `calculateWorstCaseTrueOdds`: Test with known Pinnacle odds, check if output probabilities sum to 1 and odds are derived correctly. Test which method 'wins' for different scenarios.
    * `calculateEVPercentage`: Test positive EV, negative EV, zero EV cases.
    * `calculateKellyFraction`: Test positive fraction, zero fraction, non-positive fraction cases, edge cases like odds = 1.
2.  Include tests for edge cases and potential error conditions where applicable.
3.  Show basic setup for running these tests.
**Output:** Example unit test file(s) (`*.test.ts`) containing tests for the calculation functions. Include necessary import statements assuming functions are exported from their respective files.

---

### Prompt 36: Configure Frontend Build & Deployment Prep

**Project:** EV Betting Web Application (Version 1.0)
**Goal:** Prepare the React/Vite frontend application for static deployment.
**Context:** Builds on all previous frontend prompts (React app exists).
**Task:**
1.  Ensure environment variables (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`) are correctly handled for production builds (Vite typically includes `VITE_` prefixed vars).
2.  Provide the command to create a production build of the Vite application (`npm run build` or `yarn build`).
3.  Explain where the static output files are located (usually `dist` directory).
4.  Briefly describe how to configure a static hosting provider (like Vercel or Netlify) to deploy this `dist` directory. Include typical settings like:
    * Build command (`npm run build`).
    * Publish directory (`dist`).
    * Setting environment variables in the hosting provider's UI.
    * Handling routing for Single Page Applications (SPA) - usually a rewrite rule to serve `index.html` for all paths.
**Output:**
* Build command.
* Explanation of output directory.
* Configuration guidance for static hosting providers (Vercel/Netlify).