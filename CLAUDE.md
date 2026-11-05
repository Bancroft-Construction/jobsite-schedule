# Jobsite Schedule Board

Bancroft Construction's shared job schedule. See `README.md` for features and one-time Supabase setup.

- Live site: https://bancroft-construction.github.io/jobsite-schedule/
- Hosting: GitHub Pages, published from the `main` branch. A push to `main` goes live within a minute
  or two, so there is no separate staging copy.

## Making changes

1. Edit the source, never `index.html` directly:
   - `src/app.html`: the board (views, scheduling engine, imports, exports, calendar).
   - `src/platform.html`: the sign-in screen and the Supabase database layer.
2. Rebuild: `powershell -ExecutionPolicy Bypass -File build.ps1`. This writes `index.html`.
3. Commit the source files and the rebuilt `index.html` together, then push to `main`.

## Data and sign-in

- Data is in Supabase, table `public.sched_docs`. Only signed-in users can read or write it.
- Everyone signs in with one shared Supabase login, set in `config.js`, and then types their name for
  the change history. The anon key in `config.js` is meant to be public.
- Supabase does not grant new tables automatically in this project. Grant them to `authenticated`
  explicitly, and never to `anon`.
- Never commit a `migration/` folder or real exported job and billing data. `.gitignore` excludes them.

## Calendar colors (Bancroft Carpentry only)

On the project whose name contains "Bancroft Carpentry", calendar bars, on-site chips, notes and the
calendar PDF are colored by carpenter: Lucas is orange, Jason is green, and both together are blue.
Names are read from the task's subcontractor and name, with the contact used only when neither has a
name. The rule is in `crewOf()` in `src/app.html`. Every other project keeps its status colors.

## Related site

The Staffing Planner (https://bancroft-construction.github.io/staffing-planner/) exports
`schedule-board.csv`, which this board reads with **Import from planner** (`projectsFromSheets`).
