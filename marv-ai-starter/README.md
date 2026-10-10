# Marv AI — starter project

This is a starter MVP for a movie-story and screenplay generator.

## What works in the starter

- Responsive interface for phone and computer
- Story idea, genre, setting, scene count, tone, and ending controls
- Copy and download generated script
- Sample-script button for testing the interface without AI
- Secure server-side AI request using a Cloudflare Worker secret
- Input validation and output-token limit
- No payment integration

**The real AI generator will not work until you configure a Gemini API key and deploy the Worker.** The sample button is only demo text; it does not call an AI model.

## Free/low-cost deployment plan

This project is designed for Cloudflare Workers with static assets. Cloudflare has a free plan with limits; check current plan and AI provider terms before launch. Gemini API free-tier eligibility, limits, model availability, and data-use terms can change. Do not promise unlimited free generation to customers.

### Requirements

- A Cloudflare account
- A Google AI Studio account and Gemini API key
- A computer for the easiest deployment (the website itself is mobile responsive)

## Deploy from a computer

1. Install Node.js LTS from https://nodejs.org/
2. Unzip this folder.
3. Open a terminal inside the `marv-ai-starter` folder.
4. Install Wrangler:
   `npm install -g wrangler`
5. Log in:
   `wrangler login`
6. Create/get a Gemini API key from Google AI Studio: https://aistudio.google.com/app/apikey
7. Set the key as a secret. Do not paste it into `public/index.html` or commit it to a repository:
   `wrangler secret put GEMINI_API_KEY`
   When prompted, paste the key into the terminal.
8. Deploy:
   `wrangler deploy`
9. Open the `*.workers.dev` URL printed by Wrangler.
10. Test the sample button, then enter a story idea and choose “Generate my story”.
11. Check the health endpoint by opening `/api/health` on your site. It should show `aiConfigured: true`. This only confirms a secret is present, not that the AI request has succeeded; test an actual story too.

Cloudflare's current dashboard and CLI labels may differ slightly. If you use a public Git repository, never add your API key to it.

## Deploy from an iPhone

The easiest path is to use a computer for the first deployment because installing Wrangler and securely setting secrets is simpler there. On iPhone, you can:
- Download and unzip the project using the Files app.
- Edit the HTML with a code editor, or edit the project through a browser-based development environment that supports Cloudflare Workers.
- Use the Cloudflare dashboard to create/manage the Worker and set `GEMINI_API_KEY` under Worker Settings → Variables and Secrets, marked as a secret.
- Upload the static assets using the dashboard if that workflow is available in your account, and deploy the Worker source. The dashboard's exact options can change.

If the phone workflow does not expose Worker asset deployment or secret setup, use the computer for deployment. Do not put the API key in browser code as a shortcut.

## Security and cost notes

- The API key is read only on the server as `env.GEMINI_API_KEY`.
- The request validates the story input and restricts scene counts.
- The model output is capped at 3,500 tokens per request.
- This starter does not include user accounts, persistent quotas, billing, or durable per-user rate limits. It is not yet ready for unrestricted public launch.
- Before inviting many users, add a durable rate limit (for example, Cloudflare rate-limiting rules or a suitable storage-backed limiter), abuse protection, usage monitoring, and terms/privacy pages.
- A model/API free tier can have limits and may change. Never promise that generation will remain free or unlimited.
- Do not enter real customer secrets or sensitive personal data into prompts.

## Troubleshooting

- `AI is not configured yet`: run `wrangler secret put GEMINI_API_KEY`, then deploy again.
- `The AI service could not complete this request`: verify your key, account eligibility, quota, and current model name in Google AI Studio / official Gemini API docs.
- `The AI service is temporarily rate-limited`: wait, then retry; check provider quotas.
- If the website returns a 404, confirm `public/index.html` exists and that `wrangler.toml` is in the project root.

## Official docs

- Gemini API setup: https://ai.google.dev/gemini-api/docs/get-started
- Gemini API pricing: https://ai.google.dev/gemini-api/docs/pricing
- Cloudflare Workers limits: https://developers.cloudflare.com/workers/platform/limits/
- Cloudflare Workers pricing: https://developers.cloudflare.com/workers/platform/pricing/
