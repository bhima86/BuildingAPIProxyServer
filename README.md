# Building an API Proxy Server

> Hide your public API keys, add rate limiting, and cache responses — using **Node.js + Express**.

A learning project that puts your own server in front of a third‑party API so the API key never reaches the browser. Along the way it adds rate limiting (to stop abuse) and caching (to cut upstream calls and speed things up).

📺 Based on the [Traversy Media tutorial](https://youtu.be/ZGymN8aFsv4)

---

## The problem in one picture

```
❌ WITHOUT a proxy — key is exposed in the browser
   Browser ──(api.openweather.com?appid=YOUR_KEY)──▶ Third‑party API
            anyone can read YOUR_KEY in DevTools / source

✅ WITH a proxy — key stays on the server
   Browser ──(/api?q=Miami)──▶ Your Proxy ──(?appid=YOUR_KEY)──▶ Third‑party API
                                 │
                                 ├─ injects the secret key
                                 ├─ rate‑limits callers
                                 └─ caches responses
```

The browser only ever talks to **your** server. The secret lives in a `.env` file that is never shipped to the client.

---

## What you'll build

A small weather app whose front end calls **your** `/api` endpoint instead of OpenWeather directly. The proxy attaches the API key, limits how often clients can call it, and caches results for a couple of minutes.

## Tech stack

| Concern | Package |
|---|---|
| Web server | `express` |
| HTTP requests (server→API) | `needle` |
| Load env vars | `dotenv` |
| Cross‑origin requests | `cors` |
| Rate limiting | `express-rate-limit` |
| Response caching | `apicache` |
| Auto‑restart in dev | `nodemon` (dev) |

## Quick start

```bash
# 1. Install dependencies
npm install

# 2. Create your env file from the template
cp .env.example .env
#    then fill in the three values (see below)

# 3. Run in dev mode (auto‑reloads on save)
npm run dev
```

Open <http://localhost:5000>.

### `.env` values (OpenWeather example)

```env
API_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
API_KEY_NAME = "appid"
API_KEY_VALUE = "your_openweather_api_key"
```

> Swap these three values to point the proxy at **any** key‑based public API — that's the whole idea.

## Project structure

```
.
├── index.js              # Express app: middleware, rate limit, static, routes
├── routes/
│   └── index.js          # The proxy route — injects key + forwards query params
├── middleware/
│   └── error.js          # Central error handler
├── public/               # Demo client (weather app) served as static files
│   ├── index.html
│   ├── main.js           # Calls /api?q=city — NO key here
│   └── style.css
├── .env                  # Your secrets (gitignored — never commit)
├── .env.example          # Template with empty values (safe to commit)
└── package.json
```

## 📖 Documentation (Wiki)

The full step‑by‑step guide lives in the **[Wiki](../../wiki)**:

1. [Introduction — why a proxy?](../../wiki/01-Introduction)
2. [Project Setup](../../wiki/02-Project-Setup)
3. [Basic Express Server](../../wiki/03-Basic-Express-Server)
4. [Hiding API Keys](../../wiki/04-Hiding-API-Keys)
5. [Rate Limiting](../../wiki/05-Rate-Limiting)
6. [Caching](../../wiki/06-Caching)
7. [Connect the Client App](../../wiki/07-Connect-Client-App)
8. [Deployment](../../wiki/08-Deployment)
9. [Cheatsheet](../../wiki/09-Cheatsheet)

## Credits

Tutorial by **Bpatil**. This repository is my learning write‑up and notes. Original source code is MIT‑licensed.
