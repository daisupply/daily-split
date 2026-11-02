# Daily Split

One word, hiding another. A free daily word puzzle: every day at midnight CT an
innocent word drops with a straight dictionary clue — find where it breaks into
two smaller words in 4 guesses, then solve the daily one-word riddle.

## How it works

- Puzzles are **invented by Google Gemini**, then **verified by plain code**
  before they can appear: a split ships only if its halves spell the word
  exactly (letter for letter); a riddle ships only if its answer is a single
  clean word that never appears inside the riddle text. Failures are discarded
  and the AI retries. If every attempt fails, a hand-written puzzle from the
  built-in bank runs instead, so the site never has a broken or empty day.
- The winning puzzle for each date is **pinned in Upstash Redis** — the whole
  world plays the same puzzle all day. Generation is lazy: the first visit of
  the day triggers it (with a short head-start wait), everyone else reads the pin.
- No accounts. Streaks and game state live in the visitor's browser
  (localStorage). Guesses are checked server-side so answers are not sitting
  in the page source.

## Run it

```
node server.js          # zero dependencies, Node 18+
```

Environment:

| var | purpose |
| --- | --- |
| `PORT` | listen port (default 8080) |
| `GEMINI_API_KEY` (+ `_KEY2..4`) | puzzle generation; without keys the bank runs |
| `SPLIT_UPSTASH_REST_URL`, `SPLIT_UPSTASH_REST_TOKEN` | pinning puzzles per date (falls back to `UPSTASH_REDIS_REST_URL/TOKEN`; without a store, pins live in memory) |

## Deploy (Render)

Create a web service from this repo — `render.yaml` is included — set the env
vars above (reuse the Gemini keys; make one small Upstash database for this
site), and deploy. Point a domain at it when ready, then apply for AdSense
once there's a few weeks of daily content and steady traffic.
