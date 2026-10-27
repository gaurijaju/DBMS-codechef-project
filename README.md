# NutriFit – Personalized Diet & Workout Planner

NutriFit (also called FitTrack in some project discussion) is a full-stack web application that helps users generate meal and workout plans and track their daily progress.

**Submitted by:** Gauri Jaju & Shravani Birdawade

> The planner is **rule-based**. It uses predefined conditions and seed data. It is not a trained machine-learning model.

## Features

- **Fitness profile**: collects user details and goals through an onboarding wizard.
- **Diet planner**: generates a seven-day meal plan from a predefined recipe library, with meal swapping and regeneration.
- **Workout planner**: builds a weekly schedule from a predefined exercise library, with a rest timer.
- **Daily tracker**: records calories, macros, water, completed workouts and notes.
- **Progress charts**: weight history and nutrition charts.
- **Grocery list**: categorized checklist built from the meal plan.

## Tech Stack

| Technology | Role |
|---|---|
| React | Interactive UI built from reusable components |
| TypeScript | Types for values and data structures |
| Node.js | Server runtime |
| Express | HTTP API routes and middleware |
| JSON file storage | Persists state in `server/data/store.json` |
| Vite | Frontend dev server and build |
| npm | Dependencies and scripts |
| Recharts | Charts in the frontend |

> Storage is a JSON file, **not PostgreSQL**. No database setup is required unless the code is changed.

## Architecture

1. The user enters information in the React interface.
2. React state holds the values being edited or displayed.
3. The API client sends HTTP requests to the Express backend.
4. Express routes read the request and call a service or database method.
5. Services calculate metrics or generate meal and workout plans.
6. The database module reads or writes the JSON store.
7. The backend returns JSON and React updates the screen.

## Project Structure

| Path | Purpose |
|---|---|
| `client/src/main.tsx` | Frontend entry point |
| `client/src/App.tsx` | Shared state, initial data loading, tab navigation, event handlers |
| `client/src/api/client.ts` | Central place for frontend API requests |
| `client/src/components/` | OnboardingModal, DashboardOverview, MealPlanner, RecipeModal, WorkoutPlanner, DailyTracker, ProgressCharts, GroceryList, Navbar |
| `server/src/index.ts` | Express setup, middleware, routes, error handling, port |
| `server/src/routes/api.routes.ts` | Maps API endpoints to services and storage |
| `server/src/services/calculator.service.ts` | BMR, TDEE, target calories, macros, BMI, estimates |
| `server/src/services/diet.service.ts` | Recipe filtering, seven-day plan, meal swaps |
| `server/src/services/workout.service.ts` | Weekly workout schedule |
| `server/src/db/database.ts` | Reads and writes the JSON store |
| `server/src/data/seedData.ts` | Initial recipe and exercise data |

Check the exact path of the shared type definitions in your copy of the code.

## Calculations

All outputs are estimates, not medical measurements.

- **BMR (Mifflin–St Jeor):** male `10 × kg + 6.25 × cm − 5 × age + 5`; female `… − 161`.
- **TDEE:** BMR × activity factor (sedentary 1.2, light 1.375, moderate 1.55, active 1.725, very active 1.9).
- **Target calories:** fat loss subtracts the smaller of 500 kcal or 20% of TDEE; muscle gain +350; athletic +150; maintenance +0. Minimum target is 1,200 kcal/day (a software rule).
- **Macros (standard diet):** protein 28%, carbs 47%, fat 25% of calories.
- **BMI:** kg ÷ m².
- **Water:** 35 ml per kg plus an activity-based amount.
- **Timeline:** weight difference ÷ 0.5 kg per week, at least one week.

Worked example: male, 21, 70 kg, 175 cm, moderate activity, fat loss gives BMR ≈ 1,694, TDEE ≈ 2,626, target ≈ 2,126 kcal, BMI ≈ 22.9.

## Workout Schedules

| Training days | Style |
|---|---|
| Up to 3 | Full-body |
| 4 | Upper / lower split |
| 5–6 | Push / pull / legs |

Exercises are filtered by location, equipment, fitness level and muscle group. Sets, reps and rest adjust to fitness level.

## Getting Started

From the project root (the root `package.json` expects `client` and `server` folders):

```powershell
# install all dependencies
npm run install:all

# start frontend and backend together
npm run dev

# or in two terminals
npm run dev:server
npm run dev:client
```

The backend commonly runs on port **5000** and Vite on **5173**. Check the terminal output for the actual URLs. If a command fails, check folder names and each `package.json` script.

## Known Limitations

- The meal generator sums recipe nutrition but does not optimize to meet the daily calorie target.
- Grocery items keep the first occurrence of an ingredient; duplicate quantities are not merged.
- Backend validation of profile and request data is incomplete.
- JSON storage has no relational constraints, transactions or multi-user isolation.
- No authentication.
- No automated tests.

## Future Improvements

- Optimize meal selection toward calorie and macro targets.
- Aggregate duplicate grocery ingredients in compatible units.
- Validate all profile fields and requests on the backend.
- Migrate to SQLite or PostgreSQL.
- Add authentication and per-user data.
- Add tests for formulas, meal swaps, workout generation and storage.

## Health Note

Calorie, BMI, hydration and weight-timeline results are estimates. They are not medical advice or a guarantee of any result.
