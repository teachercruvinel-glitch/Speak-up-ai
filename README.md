# Speak Up Units 5–8 · AI grading starter

This is an adapted development starter for the 10 questions from Units 5–8. The original website is unchanged. **Not production-tested.**

## Setup
1. Create a Supabase project. In its SQL editor run `schema.sql`.
2. Create a **private** Supabase Storage bucket named `recordings`.
3. In Supabase Authentication create a teacher account with email/password. Do not enable public signups for teachers.
4. Set Netlify environment variables: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `GEMINI_API_KEY`, `TEACHER_EMAIL` (same email as your teacher login).
5. Deploy the whole folder to Netlify using GitHub (Netlify Functions need a build/deployment, not a standalone HTML file).
6. Open `/` for students and `/teacher.html` for the teacher dashboard.

## Important
- AI scoring is **Cambridge-inspired**, not an official Cambridge assessment. Pronunciation judgments may be unreliable.
- Students should be told that their voice and name will be stored and processed by third-party providers. Obtain appropriate consent, especially for minors, and set a school-approved retention policy.
- Audio uploads go to a private bucket; the dashboard uses short-lived signed playback URLs.
- This starter uses a teacher email allowlist and a server-side authentication check. For multi-teacher deployments use role-based access controls.
- Audio uploads and AI calls can cost money. Test with a small group before full-class rollout.
- The grade endpoint evaluates submitted audio clips and may take time. Netlify's function timeout can be a constraint for 10 audio clips; consider a background job/queue for production use.

## Deployment note
Drag-and-drop static publishing may not deploy functions. Use a Git-connected Netlify build or Netlify CLI (`netlify deploy --build --prod`) from the project root. Configure variables on Netlify, not in the HTML.

## Critical limitations before student rollout
- The starter uploads ten base64 clips in a single request. This may exceed request-size limits, especially for longer responses. Implement per-file direct-to-storage uploads and resumable retries for classroom use.
- Grading all ten recordings synchronously may time out. Use a background job and polling for robust operation.
- Add abuse protection/rate limiting, consent and deletion workflows, and test authentication and RLS.
- Do not use with real student recordings until a small end-to-end test and school privacy review succeed.
