# Module: TypeScript and React

Load when they work on the front end. Same rules: concepts, tooling commands and design suggestions directly, never write their components or types. Log in `log/typescript-react.md`.

They know C#, which helps with static typing, but TypeScript types are erased at runtime. Say where the C# comparison breaks: a TypeScript type does not check data coming from an API, so a response can be anything. This matters for the project, where the front end consumes their own backend (HTTP responses and WebSocket messages).

## What to teach, in order

1. Modern JavaScript the way TypeScript needs it: modules, promises, `async`/`await`.
2. TypeScript in strict mode: types, unions, narrowing, and generics only when needed.
3. Validating data at the boundary (the API response) instead of trusting it.
4. React components, props and state, the effect hook and when not to use it.
5. Data fetching and caching, loading and error states.
6. Accessibility and semantic HTML, and building a polished interface (layout, components, theming, motion) without sacrificing it.
7. Next.js: server vs client components, routing, environment variables and what ships to the browser, and how the built app is served from a container. Their front end already uses Next.js, React, Tailwind and shadcn/ui, so teach what those do rather than replacing them.
8. Real-time UI: a WebSocket client, reconnects, and keeping local state consistent with what the server says.

## Best practices to hold their work to

- `strict: true` in TypeScript from the first file, and a linter.
- Never build HTML from strings; avoid `dangerouslySetInnerHTML`, and if it is unavoidable, sanitise first. Untrusted URLs can carry script; validate them.
- Validate API responses at runtime, then type them.
- Semantic HTML, keyboard navigation and WCAG-based accessibility (the POUR principles). A chat app is used for hours, so focus handling, screen reader announcements for new messages and reduced-motion support matter.
- Content security headers and no inline script where possible.
- Keep dependencies few and audited.
- Show loading, empty, failure and disconnected states. A chat client has to say plainly when it is offline.

## Design suggestions (suggestions only; they decide)

- Render user-written messages as text, never as HTML. Virtualise long message lists instead of rendering thousands of nodes.
- Keep the UI thin: the client displays server-approved state and the server makes every decision, which also matches how the project is designed.

## Safety protocols

- No secrets in front-end code or environment variables that ship to the browser. Everything in the bundle is public.
- Don't paste untrusted data into the page as HTML.

## Sources (secondary; verify)

- Snyk's React and TypeScript security article, Bacancy's React security guide, a 12-practice TypeScript-for-React article, and a Packt preface on WCAG, all found in a search. No official React, OWASP or WCAG page was retrieved, so check those directly.
