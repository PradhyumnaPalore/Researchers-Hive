# Researcher's Hive

> Originally built as a team project with **SENewTeam** for a software engineering course; re-hosted here as part of my personal portfolio.

Researcher's Hive is a research tool designed to help PhD students and researchers manage and organize their research knowledge. It provides efficient search, storage, visualization, commenting, and recommendation features to enhance the research workflow.

## Features
- Efficient Paper Search Quickly find relevant research papers based on topics of interest.
- Organized Research Knowledge Profile page organizes papers and notes based on recent activity.
- Graphical Visualization Visualize connections between research papers through authors and citations.
- Enhanced Notes and Annotations Add images, tables, and text-based comments to your research notes.
- Research Discoveries Get real-time alerts for similar research papers.
- AI-Enhanced Commenting Use AI to enhance the quality of comments.

## Tech Stack
- UI Design Figma
- Frontend ReactJS
- Backend Django
- Database MongoDB (for storing research papers and notes)
- Third-Party Tools
  - Semantic Scholar for gaining paper information
  - GooglePalm for AI integration in the comment section

## Setup Instructions (local development)

### Backend
1. `cd backend`
2. `pip install -r requirements.txt`
3. `python -m spacy download en_core_web_sm` (required by the paper-parsing NLP pipeline)
4. Copy `.env.example` to `.env` and fill in `MONGO_URI` and `GEMINI_API_KEY` (get a key at https://aistudio.google.com/apikey)
5. `python3 manage.py migrate`
6. `python3 manage.py runserver`

### Frontend
1. `cd frontend`
2. `npm i --force` (needed because `@toast-ui/react-editor` still pins a React 17 peer dependency; everything else on this project uses React 18)
3. Copy `.env.example` to `.env.local` if you want to point at a deployed backend instead of `localhost:8000`
4. `npm run dev`
5. Open http://localhost:5173

### nginx (optional, for serving both from one origin locally)
1. Install nginx
2. Update `nginx.conf` with your own paths
3. `nginx.exe -t` to test the config, `nginx.exe -s reload` to reload

## Deploying

This repo is set up to deploy as two separate services plus a managed database:

1. **Database** — create a free [MongoDB Atlas](https://www.mongodb.com/atlas) cluster, get its connection string.
2. **Backend** — deploy `backend/` to [Render](https://render.com) (the included `render.yaml` blueprint does most of the setup). Set `MONGO_URI`, `DJANGO_CORS_ORIGINS`, and `GEMINI_API_KEY` in the Render dashboard.
3. **Frontend** — deploy `frontend/` to [Vercel](https://vercel.com) or [Netlify](https://netlify.com) (build command `npm run build`, output dir `dist`). Set `VITE_API_BASE_URL` to the backend's Render URL. `vercel.json` / `public/_redirects` are already included for client-side routing.
4. Update the backend's `DJANGO_CORS_ORIGINS` to the frontend's final domain once you have it.

> Heads-up: this project uses `djongo` as the Django↔MongoDB adapter. It's an older, lightly-maintained package — if you hit ORM errors on deploy that don't reproduce locally, that's the most likely culprit.

