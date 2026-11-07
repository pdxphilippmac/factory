<!-- BEGIN:nextjs-agent-rules -->

## This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

## Factory (project rules)
### Docs
1. Für Next-APIs zuerst die lokalen Guides unter `node_modules/next/dist/docs/` (wie oben).
2. Context7 zusätzlich, wenn etwas unklar ist oder nicht in den lokalen Docs steht.
3. Trainingswissen zu Next nicht als Wahrheit behandeln.
4. Context7-Antworten kurz halten — keine Doc-Walls.
### Done
- Nach Änderungen: `npm run lint`, dann `npx tsc --noEmit`
- `git diff` zeigen, keine Commits ohne Auftrag
- Scope klein halten