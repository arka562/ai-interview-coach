# AI Interview Prep Frontend

React/Vite frontend for the AI Interview Preparation Platform.

## Frontend Project Bullet Points

- Built a protected React application with login, registration, dashboard, interview, resume, analytics, and history pages.
- Used React Router to separate public authentication screens from protected application routes.
- Managed authentication state with Redux Toolkit and reusable selector/hooks.
- Connected the UI to the Express backend through a centralized Axios API client.
- Created dashboard cards for total sessions, questions answered, average score, weak topics, quick actions, and recent activity.
- Built resume upload and resume profile screens for personalized interview preparation.
- Added interview session pages for viewing generated questions, submitting answers, and reviewing AI feedback.
- Added session history and detail pages so users can review previous practice sessions.
- Built analytics views for topic performance, weak areas, score trends, and AI recommendations.
- Added browser speech recognition and speech synthesis hooks for voice-based interview practice.
- Implemented reusable UI components such as buttons, cards, inputs, loaders, answer boxes, feedback panels, question lists, and session headers.
- Added PDF report generation with jsPDF so users can export interview results and feedback.

## Tech Stack

- React
- Vite
- React Router
- Redux Toolkit
- Axios
- Tailwind CSS
- jsPDF

## Local Development

```bash
npm install
npm run dev
```

Create a `.env` file with:

```txt
VITE_API_BASE_URL=http://localhost:5000/api/v1
```

## Build

```bash
npm run build
```
