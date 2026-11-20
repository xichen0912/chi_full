# CHI 2027 — merged prototype site

One site built from four team prototypes:

| Part | Came from | Now lives in |
|---|---|---|
| Program, session detail, Today view, Ask CHI chatbot | `zayyyseuc/chi-prototyping` (React) | `app/` → `/program/` |
| Map: floor plans, room details, directions | `xichen0912/map_test` | `pages/map/` → `/map/` |
| People, Affiliations, Recommendations | `dengyixin2026-lang/CHI-2027` | `pages/people/` → `/people/` |
| Awards, Accessibility FAQ | `Huanggedv/CHI-Website-optimization-Accessibility-Award` | `pages/awards/`, `pages/accessibility/` |
| My (new) | merged from the four "My …" lists | `pages/my/` → `/my/` |

## What was merged or removed

- **One sidebar** (`shared/shell.js`) on every page, replacing the four separate navigations. Every item points inside the site; links to programs.sigchi.org and dead `#` links are gone.
- **One conference identity:** CHI 2027 · Pittsburgh · May 10–14, 2027. Venue layout, accessibility FAQ and award list are adapted from CHI 2026, and the sidebar says so.
- **Duplicates removed:** Program's minimal Map, People and My pages, plus Map's own Program list. Each section has a single page now.
- **One data source** (`app/src/data/`, exported to `shared/data.js`): sessions, rooms and venue geometry are shared by Program, Map, People and Ask CHI. Session rooms are real map rooms, e.g. `P1 · Room 123`.
- **One "My" store** in localStorage (`chi-plan`, `chi-saved-papers`, `chi-award-plan`, `chi-people-saves`, `chi-user-interests`), so a change on any page shows up everywhere.
- **The People researchers are Program authors.** Each profile links to the real session and room.
- **The Quiet Room is Room M219** on the Map, matching the Accessibility FAQ. Prayer rooms (M220/M221) and childcare (P2) are marked too.
- **One palette** for every page: paper `#f4f3ef`, ink `#171816`, deep green `#314f44`, lime `#d3fb63` (on dark surfaces only), rust `#a54b25`.

## Visual spec

See `docs/VISUAL_SPEC.md`. Tokens and the spec layer live in `shared/site.css`.

## Cross-page links

- Program session or room → Map with that room selected. "← Back to Program" uses the browser's history.
- Program author → People profile (curated people) or a list of their papers (any other author).
- People / Recommendations → Program session, Map room; "Add to My plan" / "Save paper" go to My.
- Awards → Map room; "Add to My day" goes to My and appears in the Map's room schedule.
- Accessibility floors and spaces → Map (`?level=`, `?room=`); Map → Accessibility (`?need=sens`).
- Ask CHI (on every page) → source cards open the matching page. The conversation is kept between pages.
- Deep links: `map/?room=…`, `map/?find=quiet`, `people/?person=…`, `awards/?day=Tue#paper-…`, `program/?session=…`.

## Build and run

```bash
cd app
npm install
npm run build      # builds Program + shared scripts and assembles the whole site into ../dist
npm run preview    # serves ../dist — open /program/
```

`npm run dev` runs only Program (React, with the local `/api/chat` route) for fast iteration. The full site with the sidebar comes from `npm run build`.

Ask CHI runs in **Demo mode** on static hosting (GitHub Pages). To use the live AI route, deploy the `/api/chat` handler from `app/vite.config.ts` on a server and build with `VITE_CHAT_API=/api/chat`. Keep `OPENAI_API_KEY` on the server.

## Deploy

`.github/workflows/deploy.yml` builds `app/` and publishes `dist/` to GitHub Pages on every push to `main`. All paths are relative, so the site works under any repository sub-path.

## Layout

```
app/          React Program (Vite). src/data = shared data, src/widget.tsx = Ask CHI for static pages
pages/        map/ people/ awards/ accessibility/ my/ (static HTML)
shared/       site.css, shell.js (sidebar + store), awards-data.js, dc-lite.js, favicon.svg
scripts/      assemble.mjs (copies pages + shared into dist/)
docs/         original notes from the Program prototype
```
