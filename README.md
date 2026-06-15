# AI Interview Preparation Platform

A full-stack AI interview preparation application that helps users practice technical and behavioral interviews with resume-aware questions, adaptive difficulty, answer evaluation, and performance analytics.

## Project Bullet Points

- Built a full-stack interview preparation platform using React, Vite, Redux Toolkit, Express.js, MongoDB, and JWT authentication.
- Implemented secure user registration and login with protected frontend routes and token-based backend authorization.
- Developed an AI-powered interview question generator that creates role-specific questions based on job role, experience level, selected topics, and question count.
- Added resume-based interview preparation where users upload a PDF or text resume, the backend extracts resume content, analyzes skills and experience, and generates personalized interview questions.
- Designed adaptive interview sessions that adjust the next question's topic and difficulty using previous answer scores and the user's weakest skill areas.
- Integrated multi-provider LLM routing with fallback support for Gemini, OpenRouter, Groq, and Hugging Face to improve reliability when one provider fails or times out.
- Created an AI answer evaluation workflow that scores user responses, identifies strengths and weaknesses, generates ideal answers, suggests follow-up questions, and stores attempt history.
- Built a skill profile system that tracks topic-wise performance, overall score, accuracy rate, response time, strongest topics, weakest topics, and improvement history.
- Developed analytics dashboards showing total sessions, completed attempts, average score, weak topics, score trends, recommendations, and recent activity.
- Added interview session history and detailed session views so users can revisit previous questions, attempts, scores, and feedback.
- Implemented voice-based interview support using browser speech recognition and speech synthesis hooks for a more realistic practice experience.
- Added PDF report generation on the frontend so users can export interview feedback and performance summaries.
- Secured the backend with Helmet, CORS allowlists, request body limits, compression, centralized error handling, and AI endpoint rate limiting.
- Used MongoDB models for users, sessions, questions, answer attempts, resume profiles, and skill profiles to maintain structured interview progress data.
- Prepared deployment configuration for hosting the frontend on Netlify, backend on Render, and database on MongoDB Atlas.

## Core Features

- Authentication: register, login, protected dashboard, and authenticated API access.
- Interview sessions: create, list, view, complete, and delete interview sessions.
- AI questions: generate questions from role, experience, topics, or uploaded resume.
- Answer evaluation: receive scores, feedback, ideal answers, strengths, weaknesses, and follow-up prompts.
- Adaptive practice: next questions are selected from weak topics and adjusted by performance.
- Resume analysis: extract resume text, detect target role, skills, experience level, projects, strengths, and gaps.
- Analytics: track progress across sessions, attempts, topics, scores, and recommendations.
- Voice mode: practice answering with speech input and listen to prompts with speech synthesis.
- PDF reports: export performance summaries and feedback from the frontend.

## Tech Stack

- Frontend: React, Vite, React Router, Redux Toolkit, Axios, Tailwind CSS, jsPDF.
- Backend: Node.js, Express.js, MongoDB, Mongoose, JWT, Multer, pdf-parse.
- AI: Gemini, OpenRouter, Groq, Hugging Face through a fallback router.
- Deployment: Netlify for frontend, Render for backend, MongoDB Atlas for database.

## Local Development

Frontend:

```bash
cd frontend/ai_interview_prep
npm install
npm run dev
```

Backend:

```bash
cd backend
npm install
npm run dev
```

Required backend environment variables:

```txt
MONGO_URI=your MongoDB Atlas connection string
JWT_SECRET=your JWT secret
CLIENT_URL=http://localhost:5173
GEMINI_API_KEY=your Gemini key
```

Optional AI fallback variables:

```txt
OPENROUTER_API_KEY=...
GROQ_API_KEY=...
HF_API_KEY=...
HF_BASE_URL=...
```

Frontend environment variable:

```txt
VITE_API_BASE_URL=http://localhost:5000/api/v1
```

## Deployment

See `DEPLOYMENT.md` for Netlify, Render, and MongoDB Atlas setup steps.
