# AI-Powered Career Coach

FastAPI + vanilla JavaScript, suitable for local development and Vercel deployment.

## Features
- Adaptive, one-question-at-a-time AI mock interviews
- Interview feedback and local-browser history (last 10 sessions)
- CV extraction from PDF, DOCX, and TXT, plus pasted text
- CV review, job-fit / skill-gap analysis, and 4-week learning roadmap
- Markdown report download

## Local setup

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Set your environment variable `OPENAI_API_KEY` (and optionally `OPENAI_MODEL`, default `gpt-4.1-mini`). Do not commit API keys.

```bash
uvicorn api.index:app --reload
```

Open http://127.0.0.1:8000 and check http://127.0.0.1:8000/api/health.

## Deploy to Vercel
1. Push the **contents of this folder** to a GitHub repository, with `vercel.json` at repository root.
2. In Vercel, import that repository. Set Framework Preset to **Other** and leave Root Directory at the repository root (`./`).
3. Add `OPENAI_API_KEY` to Project Settings > Environment Variables; optionally add `OPENAI_MODEL`.
4. Deploy and open `/api/health`. It should return `configured: true`.
5. Test `/api/interview`, `/api/extract`, and `/api/analyze` from the UI.

Vercel serverless functions are stateless. This project sends interview history in each request rather than relying on server sessions. Interview reports are saved only in browser localStorage, not in a server database.

## Notes
- API requests require a paid OpenAI API key, separate from a ChatGPT subscription.
- The app does not implement live job discovery or company web research.
- Scanned image-only PDFs need OCR before upload.
- For public production deployment, add authentication, rate limits, persistent storage, stricter HTML sanitization, and privacy/retention controls.
- This is a functional student MVP, not a production-hardened service.

## Vercel import error fix
Static files mount is conditional; Vercel serves public/ independently. Redeploy and test /api/health. GET /api/interview returns 405 as it requires POST.
