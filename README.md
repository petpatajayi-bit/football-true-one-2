# FootForge Football Training Tracker

FootForge is a football training tracker with saved local profiles, sessions, routines, match stats, Bluetooth heart-rate support, built-in training tips, and a free Wikipedia football lookup. The optional OpenAI endpoint supports conversational answers and photo understanding when configured by the site owner.

## Free option: GitHub Pages

You do not need to pay for an AI API or use Vercel to keep the tracker online. The GitHub Pages version can provide the built-in football tips and free online article summaries. Update the repository's root `index.html` with the latest `index.html` from this project, commit the change to `main`, and wait for GitHub Pages to publish it. The updated page detects `github.io` and does not call the unavailable AI server.

This free option is not ChatGPT: it cannot freely answer every question or analyze photos. Profiles and training records stay in each user's browser. Online lookups need an internet connection.

## Run locally

1. Install Node.js 20 or later.
2. Run `npm install`.
3. Copy `.env.example` to `.env.local` and add your OpenAI API key to `.env.local`. Keep this secret file private; it is ignored by Git.
4. Run `npm run dev` and open the local URL shown by Vercel.

## Deploy worldwide with Vercel

1. Push this project to a GitHub repository and import it into Vercel.
2. Create an Upstash Redis database and add `OPENAI_API_KEY`, `UPSTASH_REDIS_REST_URL`, and `UPSTASH_REDIS_REST_TOKEN` as secret environment variables in Vercel project **Settings → Environment Variables**. Optionally set `OPENAI_MODEL` (defaults to `gpt-4.1-mini`). The AI route stays unavailable on production/preview deployments until shared rate limiting is configured.
3. Deploy or redeploy the project. Vercel serves the page over HTTPS and runs `api/chat.js` without exposing the key to visitors.
4. Use the Vercel HTTPS URL to share the tracker worldwide.

If chat reports **405**, the site host is not running the `api/chat.js` POST function. Deploy the whole project folder to Vercel (not just `index.html` to a static-only host), confirm the deployment includes the `api` folder, and open the site root rather than visiting `/api/chat` directly. A browser GET to `/api/chat` is expected to return 405; the chat itself calls it with POST.

## Use the free coach on GitHub Pages

GitHub Pages hosts static files only and cannot run `api/chat.js`. The page detects `github.io` and uses the built-in football tips plus free online article summaries instead of calling the unavailable AI endpoint. Upload/push the updated `index.html` to the repository and wait for GitHub Pages to redeploy. Photo analysis and ChatGPT-style answers require a separately hosted AI backend such as the Vercel setup above.

OpenAI API usage may incur charges. Set usage limits in the OpenAI dashboard, enable Vercel Firewall rate limiting, and monitor usage before sharing widely. The app sends baseline security headers; the chat endpoint checks same-origin requests, input types, image sizes, and request rates. Hosted deployments use a shared Redis-backed per-IP limit and fail closed if it is unavailable. Rate limits cannot stop all abuse, especially distributed traffic. The page has no login system, and its inline scripts mean the Content Security Policy allows inline scripts; keep edits reviewed and consider moving scripts/styles into separate files before tightening that policy further.

## Data and photo privacy

Player profile, routines, sessions, and match stats stay in that browser's local storage; there are no accounts or cloud syncing. Unrecognized football questions are sent to Wikipedia's public API for a short article summary; the search phrase is shared with Wikipedia. If the optional AI service is configured, chat text and attached photos are sent to OpenAI when a user sends a message. Photos are limited to 3 MB and supported image formats. Do not upload private or sensitive images.

Bluetooth pairing requires a compatible browser and a watch that broadcasts the standard Bluetooth heart-rate service. Steps and team possession are not read from the watch by this page.
