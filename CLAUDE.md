# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Study/practice project (VibeCoding Study-06). The main artifact is `index.html` — a self-contained Korean-language shopping list app (쇼핑 리스트) written in vanilla HTML/CSS/JS with no build step and no framework. All markup, styles, and script live in that single file. State is an array of `{id, name, done}` items persisted to a Supabase Postgres table `public.shopping_items` (project `ypskzcwchyysnhhsjykj`), accessed from the browser via `@supabase/supabase-js` loaded as an ES module from the jsDelivr CDN. RLS is enabled with permissive anon CRUD policies (study project — no auth). Every mutation awaits the Supabase call, updates the in-memory `items` array, then does a full `render()` re-draw of the list.

There is no build, lint, or test tooling. To run the app, open `index.html` directly in a browser (e.g. `start index.html` in PowerShell); it needs network access to reach the CDN and Supabase.

The git remote is `jundalsong/Shopping-List-App` on GitHub, which is connected to the Vercel project `shopping-list-app` (team `hogongvibe`) — pushing to `main` auto-deploys to https://shopping-list-app-chi-nine.vercel.app/. `.vercelignore` keeps CLAUDE.md and `.claude/` out of the public deployment.

UI text is in Korean; keep new user-facing strings in Korean to match.

## Custom skill

`.claude/skills/auto-code-review/` defines an `auto-code-review` skill (Korean). Invoke it when the user asks for a code review ("리뷰", "코드 리뷰", "review", "이 코드 봐줘" 등). Its rules: read the full target files (not just diffs), verify each finding with a concrete failure scenario before reporting, order findings correctness > security > performance > cleanup, report with `file:line` references sorted by severity, and only apply fixes if the user explicitly asks ("고쳐줘" / `--fix`).
