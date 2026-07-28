# AI Study Assistant

An AI-powered web application that helps students understand any topic. Enter a subject and the AI explains it in simple language, provides a real-world example, and generates a 5-question quiz to test understanding.

## The Problem

Students often struggle when textbook explanations are too dense, lectures move too fast, or a tutor isn't available. Concepts stay unclear, examples feel abstract, and there's no quick way to check whether the material actually sunk in. AI Study Assistant solves this by turning any topic into a clear explanation, a concrete real-world example, and an instant quiz — all generated on demand.

## Features

- **Topic input** — Students can enter any study topic they want to understand.
- **Simple explanation** — The AI breaks the topic down into clear, student-friendly language.
- **Real-world example** — One concrete everyday example makes the concept stick.
- **5-question quiz** — Auto-generated multiple-choice questions with instant scoring.
- **Copy response** — Copy the full explanation, example, and quiz to the clipboard.
- **Loading animation** — A polished loading state while the AI generates the response.
- **Professional homepage** — Describes the app and the problem it solves.
- **Responsive design** — Works across mobile, tablet, and desktop.

## AI Feature Explanation

The app uses the OpenAI Chat Completions API (`gpt-4o-mini`) with the following system prompt:

> "You are an AI Study Assistant. Explain topics in simple language for students, provide real-world examples, and generate 5 quiz questions for practice."

For each request the app sends the student's topic and asks the model to return a structured JSON object containing:

1. `explanation` — a simple-language explanation of the topic.
2. `realWorldExample` — one real-world example.
3. `quiz` — exactly 5 multiple-choice questions, each with 4 options and a correct answer index.

The OpenAI API key is kept server-side in a Supabase Edge Function so it is never exposed to the browser. The frontend calls the edge function, which proxies the request to OpenAI and returns the parsed JSON.

## Technologies Used

- **React** — UI library
- **TypeScript** — type safety
- **Tailwind CSS** — styling
- **Vite** — build tool and dev server
- **lucide-react** — icons
- **Supabase Edge Functions** — serverless proxy for the OpenAI API
- **OpenAI API** — AI explanation, example, and quiz generation

## Project Structure

```
.
├── index.html
├── package.json
├── tailwind.config.js
├── postcss.config.js
├── vite.config.ts
├── vercel.json
├── README.md
├── supabase/
│   └── functions/
│       └── ai-study/
│           └── index.ts        # Edge function proxying OpenAI
└── src/
    ├── main.tsx
    ├── App.tsx                 # View routing (home / study)
    ├── index.css
    ├── components/
    │   ├── Home.tsx            # Landing page
    │   ├── StudyInterface.tsx  # Topic input + results
    │   ├── LoadingAnimation.tsx
    │   └── Quiz.tsx
    ├── lib/
    │   ├── supabase.ts          # Supabase client + function URL
    │   └── ai.ts                # fetchStudyTopic helper
    └── types/
│       └── study.ts            # Shared TypeScript types
```

## Installation

### Prerequisites

- Node.js 18+
- An OpenAI API key
- A Supabase project (the edge function hosts the server-side proxy)

### Setup

1. **Clone the repository**

   ```bash
   git clone <your-repo-url>
   cd ai-study-assistant
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Configure environment variables**

   Create a `.env` file in the project root:

   ```env
   VITE_SUPABASE_URL=your_supabase_project_url
   VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
   ```

4. **Add the OpenAI API key as a Supabase secret**

   The edge function reads the key from the `OPENAI_API_KEY` secret. Set it in your Supabase project's Edge Function secrets (Project Settings → Edge Functions → Secrets):

   ```env
   OPENAI_API_KEY=your_openai_api_key
   ```

5. **Deploy the edge function**

   Deploy the `ai-study` edge function from `supabase/functions/ai-study/index.ts` using the Supabase dashboard or CLI.

6. **Run the dev server**

   ```bash
   npm run dev
   ```

7. **Build for production**

   ```bash
   npm run build
   ```

## Deployment

### Vercel

1. Push the repository to GitHub.
2. Import the project in Vercel.
3. Add the environment variables (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`) in the Vercel project settings.
4. Deploy. The included `vercel.json` handles SPA routing.

### GitHub

The repository is ready to push as-is. The `.gitignore` excludes `node_modules`, build output, and environment files.
