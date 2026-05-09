Now I have a complete picture of the entire codebase. Here's a thorough breakdown:Now I have everything I need to give you a comprehensive breakdown of the entire codebase.Here's the complete knowledge breakdown of your **HabitFlow** codebase:

What is HabitFlow?
------------------

A full-stack **habit tracking web app** (also a PWA) built for mindful, consistent daily habits. It's editorial in style — calm, non-militant, focused on gentle progress over guilt-driven tracking.

Tech Stack
----------

**Frontend:** React 19 + Vite + Tailwind CSS + React Router**Backend:** Node.js + Express 5 + Mongoose**Database:** MongoDB Atlas (cloud)**Auth:** Clerk (third-party auth SaaS — handles login, JWT, sessions)**Build/PWA:** Vite + vite-plugin-pwa with service worker for offline support

Backend Deep Dive
-----------------

### server.js

The Express entry point. Loads: cors, morgan (dev logging), clerkMiddleware (injects req.auth). Mounts all routes under /api/. Has a fallback error handler.

### config/db.js

Connects to MongoDB Atlas via MONGO\_URI using Mongoose. Exits process on failure.

### Authentication Flow (middleware/auth.js)

Two middlewares:

*   protect — used on ALL private routes. Reads req.auth.userId (injected by Clerk). Looks up the user in MongoDB by clerkId. If not found, returns 401 with code: 'USER\_NOT\_SYNCED' so the frontend knows to call the sync endpoint first.
    
*   protectSync — lighter guard, just checks Clerk token exists. Used on the sync route.
    

### Models (MongoDB Collections)

ModelKey FieldsUnique IndexUserclerkId, email, name, userType, customIllustration, profileInfoclerkId, emailHabituser (ref), title, type (yes\_no/count/duration), target, unit—Loguser, habit (ref), date (YYYY-MM-DD), status, note, value1 log per habit per dayMooduser, emoji, energy (1–100), date1 mood per dayReviewuser, month (YYYY-MM), rating, wins\[\], losses\[\], lessons1 review per monthGoaluser, title, motivation, impact, status (Active/Completed/Dropped)—

### Controllers (Business Logic)

*   **authController** — syncUser (create/find user on Clerk login), getMe, updateOnboarding (set userType), updateProfile (age, hobbies, health conditions)
    
*   **habitController** — CRUD for habits (get all, create, delete)
    
*   **logController** — getLogs (filtered by month/year), toggleLog (upsert — creates or updates a log entry)
    
*   **moodController** — upsert mood per day
    
*   **goalController** — get, create, delete goals
    
*   **reviewController** — upsert monthly review
    
*   **analyticsController** — getStats (today's score, 7-day weekStats, total habits), getCorrelation (14-day habit-completion vs energy-level data)
    

### Routes

All protected by protect middleware except /auth/sync. Full CRUD mapped cleanly — e.g., GET/POST /api/habits, DELETE /api/habits/:id.

Frontend Deep Dive
------------------

### Entry (main.jsx)

Wraps the app in (for Clerk auth hooks) → (custom context bridging Clerk to our MongoDB user) → .

### AuthContext.jsx

The core auth bridge. On every Clerk sign-in event:

1.  Gets a JWT from Clerk (getToken())
    
2.  Stores it in localStorage
    
3.  Calls POST /api/auth/sync to create/find the MongoDB user
    
4.  Stores the MongoDB user object in React state
    

Exposes: user (MongoDB user object), updateOnboarding(), logout(), loading.

Also sets window.getClerkToken = getToken so the Axios interceptor can call it.

### api/axios.js

Axios instance pointing to http://localhost:5000/api. Has two interceptors:

*   **Request**: reads JWT from localStorage, falls back to window.getClerkToken(). Attaches as Authorization: Bearer .
    
*   **Response**: on any 401, clears storage and redirects to /login. Has an isRedirecting guard to prevent redirect loops.
    

### App.jsx — Routing

*   Public: /home (LandingPage), /login, /signup
    
*   Protected (via ProtectedRoute): /dashboard, /tracker, /analytics, /goals, /mood, /review, /settings
    
*   /onboarding — protected but outside the (no sidebar)
    
*   ProtectedRoute checks both Clerk's isSignedIn AND whether the user has a userType in the DB. If signed in but no userType → redirect to /onboarding.
    

### Pages

PageWhat it doesLandingPageHero section with auto-rotating image carousel (4s interval), feature cards, responsive mobile navLogin / SignupClerk's / components, styled with a transparent dark glass card over a hero photo backgroundOnboarding2-step flow: pick user type → pick avatar illustration. Saves to backend via updateOnboarding + /auth/syncDashboardGreeting, today's score vs total habits, streak calculation, weekly completion %, quick habit toggle buttonsTrackerFull monthly matrix table (habits × days). Left-click to toggle log, right-click to open note modal. Quick-add suggestions based on userType. Summary row at bottom.Analytics3 KPI cards, weekly bar chart (Recharts), habit vs energy correlation chart (Composed chart), GitHub-style calendar heatmapGoalsCreate goals with title/motivation/impact. Each goal card shows hardcoded "recommended protocols" based on impact categoryMoodPick emoji mood + energy slider (1–100). Saves once per day (upsert). Shows last 14 moods as history cards.ReviewMonthly reflection form: rating slider, wins/losses (newline-separated lists), lessons text. Archives past months.SettingsDark mode toggle (via Tailwind's dark class on ), email notification toggle (UI only/demo), JSON data export

### Components

*   **Layout** — Collapsible sidebar (64px collapsed / 256px open). Shows (page content) + floating in the bottom-right corner
    
*   **FocusTimer** — Pomodoro timer: 25 min work / 5 min break. Minimizes to a floating button. Auto-switches modes. Plays a beep sound on completion.
    
*   **ProfileModal** — Profile editing modal (referenced in Dashboard but lightly integrated)
    

Data Flow for a Typical Action
------------------------------

**User checks off a habit on Dashboard:**

1.  toggleHabit(habitId) → POST /api/logs with {habitId, date, status: !current}
    
2.  Backend toggleLog: finds existing log or creates new one (upsert by user+habit+date index)
    
3.  Frontend refreshes both logs and stats in parallel
    

Key Design Decisions Worth Knowing
----------------------------------

*   **Clerk handles all auth** — passwords, OAuth, sessions. Your backend only stores clerkId to link Clerk users to your MongoDB records. jwtTokens.js exists but is currently unused (Clerk replaced it).
    
*   **One log per habit per day** — enforced via a compound unique MongoDB index on {user, habit, date}.
    
*   **Logs use date as a string (YYYY-MM-DD)** — not a Date object, which makes client-side string comparison easy but means date arithmetic happens in JS.
    
*   **Analytics are computed on every request** — no pre-aggregation or caching. The 7-day and 14-day loops run N queries each. Fine for now, would need optimization at scale.
    
*   **PWA-ready** — vite-plugin-pwa generates a service worker and manifest in /dist/, making it installable on mobile.
    
*   **Goal.impact** is a free-text string on the model but the frontend only sends health, productivity, or mental — and the SOLUTIONS lookup in Goals.jsx uses these as keys.
    

Let me know if you want to go deeper on any specific feature, fix something, or add new functionality!
