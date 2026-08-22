# LingoQuest — the published site

Live at **https://megumi-joy.github.io/skills-github-pages/**

This repository holds the *built* site and nothing else: a folder of static
HTML, CSS and JavaScript that GitHub Pages serves from the `main` branch.
There is no source here and nothing to run — editing these files by hand
would be overwritten by the next publish.

## Where the source lives

[megumi-joy/LanguageLearningAdventure](https://github.com/megumi-joy/LanguageLearningAdventure)
— a Next.js app under `web/`. Its workflow `.github/workflows/pages.yml`
builds a static export and uploads it as the `site` artifact; that artifact
is what gets committed here.

The split exists because a workflow's token only writes to its own
repository. The build happens there, the publish happens here.

## Publishing an update

1. Run **Build the static site** in the source repository (it also runs on
   push to `main` and `deploy/pages`).
2. Download the `site` artifact from that run.
3. Replace everything in this repository except `LICENSE` and this README
   with the artifact's contents, then commit to `main`. Pages picks it up
   within a minute or so.

## Two details that break the site if lost

- **`.nojekyll`** — without it Pages runs the folder through Jekyll, which
  drops every directory starting with an underscore, `_next/` among them.
  The symptom is a page with no styles, which looks like broken markup
  rather than a missing file.
- **The base path** — a project site is served from `/skills-github-pages`,
  not from the domain root, so the build sets
  `NEXT_PUBLIC_BASE_PATH=/skills-github-pages`. Publishing to a different
  repository, or putting a custom domain in front, means rebuilding with a
  different value.

## What is not here

The app's server side — the visit-tracking endpoint and the Supabase session
middleware — cannot run on Pages and is left out of this build. It stays in
the source repository for the server deployment; the client-side visit
tracker stays silent here unless a backend URL is configured at build time.
