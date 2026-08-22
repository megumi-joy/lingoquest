# LingoQuest

**Live: https://megumi-joy.github.io/lingoquest/**

A learning app for school-age students: short lessons that explain a rule,
exercises that check whether it was understood, and a map that opens as the
work gets done. Languages first, then physics, programming, chess and
robotics.

This repository serves the built site. The application source is private —
happy to walk through it.

## What is in it

- **Eleven tracks** — English (from the alphabet up), Spanish, German,
  Catalan, Ukrainian, Russian, physics, programming, chess and robotics for
  younger children.
- **A worked Ukrainian course** — *звуки, склади, наголос*: four lessons,
  each running Словник → Теорія → Практика → Тест, and each ending where the
  rule stops working rather than where it is convenient.
- **456 exercises** across four kinds — multiple choice, typed answer,
  fill-the-gap and build-the-word from letters.
- Placement test, practice sets, lecture pages, group and club schedules,
  a world map and a learner dashboard.

## How it is built

Next.js 16 (App Router) and React 19, TypeScript, Tailwind CSS 4. Supabase
for authentication in the server deployment.

The same codebase produces two builds. The usual server build is unchanged.
Setting `STATIC_EXPORT=1` produces a folder of files instead, which is what
this repository serves — no server, nothing to fall over, and the site costs
nothing to host.

Making that work took more than flipping the flag:

- **Middleware and the API route cannot exist in a static export.** They are
  not deleted or commented out — a build script moves them aside for the
  length of the export and restores them afterwards, so the server build
  keeps working from the same source.
- **Dynamic routes are enumerated at build time** through
  `generateStaticParams`, driven by the course data itself, so a new course
  gets a page without a second list to keep in step.
- **A project site is served from a sub-path**, not the domain root, which
  breaks every link the framework does not rewrite. The build asserts that
  no root-absolute link survives into the output, because the failure looks
  like broken CSS and points nowhere near its cause.
- **The build verifies the site, not just the compiler**: entry pages, a
  lesson page, the asset paths, and the `.nojekyll` marker whose absence
  makes GitHub Pages silently drop the framework's own directory.

## Two decisions worth explaining

**No personal data is collected.** Visit logging records the path, the
timestamp and a coarse country — no IP, no cookie, no user-agent. An
audience that includes children rules out casual IP logging, which is
personal data under GDPR whether or not a name is attached. In this static
build the tracker stays silent unless a backend is configured at build time.

**The exercise content is generated, not copied.** The Ukrainian school
textbooks behind it are copyrighted and carry no free licence. What was used
are facts copyright does not cover — which words occur, how often, and the
order in which a primer introduces letters — and every exercise was composed
from those facts by a generator. No page, scan or sentence from the books is
published here.
