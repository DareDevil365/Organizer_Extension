# Event Organizer project guide

## Current implementation

This project is the React website exported from Figma Make described in
[README.md](README.md). It is backed up in `DareDevil365/Organizer_Extension`.
The current folder contains a browser application, with no browser-extension
manifest or extension packaging. Adding extension support would be future work.

The interface includes sign-in, an event dashboard, event details, ticket and
payment screens, announcements, important dates, history, and a profile page.
Authentication, payment processing, and QR scanning are prototype flows:
authentication sets a local user, payment success is simulated with a timer,
and the scanner contains simulated event selection. They do not establish a
server-verified identity, actual payment, or verified ticket admission.

## Architecture and data flow

- `index.html` loads `src/main.tsx`, which mounts the React application and
  imports the global styles.
- `src/app/App.tsx` wraps the application in `AppProvider` and uses React Router
  to select pages. `PrivateRoute` checks for a user in client state.
- `src/app/context/AppContext.tsx` holds users, events, tickets, announcements,
  and important dates. Initial state is loaded from localStorage or seeded
  examples; changes to persisted state are written back by React effects.
- `src/app/components/` contains page components and interactions.
  `components/ui/` supplies reusable controls, and `hooks/useTheme.ts` handles
  shared theme behavior. Styles live in `src/styles/`.
- `vite.config.ts` configures React, Tailwind, and the `@` source-directory alias.
  `package.json` records dependencies and the development/build commands.

A user action updates component or context state. React rerenders the affected
views, context effects persist selected state in the browser, and React Router
handles navigation. Data is local to the browser; cloning the source does not
restore a user's existing localStorage data. BrowserRouter hosting requires an
HTML fallback for direct navigation to application routes.

## Technologies and tradeoffs

The rationale below is inferred from the code and export structure rather than
recorded author decisions.

- React and TypeScript support reusable interactive views and typed application
  models. A static site would be simpler but would not directly support this
  application's stateful workflows.
- Vite handles local development and production bundling. React and Tailwind
  plugins are retained as required by comments in the existing Vite config.
  Tailwind supplies utility styling; CSS files define the shared theme.
- Radix/shadcn-style UI controls provide reusable interaction primitives, with
  Lucide icons and Sonner notifications. The dependency manifest also includes
  MUI, animation, chart, form, calendar, drag-and-drop, and QR libraries inherited
  from the export; declaring a dependency does not mean every page uses it.
- React Context and localStorage avoid operating a backend for the prototype.
  A server and database would be needed for shared multi-user state and trusted
  authorization, payments, and ticket verification. These are proposals, not
  implemented capabilities.
- No dependency lockfile was present in the supplied folder. Version ranges may
  resolve differently on later installs, so this backup does not promise a
  reproducible dependency installation.

## Restore and run

Install Node.js with npm, then:

```powershell
git clone https://github.com/DareDevil365/Organizer_Extension.git
cd Organizer_Extension
npm install
npm run dev
```

Use `npm run build` to create `dist/`. Installed dependencies and generated
output are excluded from Git because they can be regenerated. See
[ATTRIBUTIONS.md](ATTRIBUTIONS.md) for the existing component and image notices.

## Backup changes and validation

The October 2026 backup adds this guide and a Git ignore file, and publishes
only the supplied `Event Organizer Website` folder. No application behavior was
changed. The sibling game engine, research companion, and prototype projects
are outside this repository. Publication is a source backup, not a deployment.

Validation for this backup checks Git whitespace and confirms the pushed commit
matches the local commit. Dependency installation, production compilation, and
browser workflows were not tested. No test script is defined in the supplied
package manifest. Whitespace review found existing trailing spaces and extra
blank lines in the supplied export; these were preserved in this source backup.
