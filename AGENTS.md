# Repository Guidelines

## Project Structure & Module Organization
`src/` contains the Vite + React renderer. Keep UI in `src/components/`, shared hooks in `src/hooks/`, IPC/data access in `src/lib/api` and `src/lib/query`, config presets in `src/config/`, and shared utilities in `src/utils/`. Static images live in `assets/` and `src/assets/`. Frontend tests live in `tests/`.

`src-tauri/` contains the Rust backend and desktop packaging. Put Tauri commands in `src-tauri/src/commands/`, higher-level application logic in `src-tauri/src/services/`, and database, proxy, MCP, and session code in their existing module folders. Rust integration tests belong in `src-tauri/tests/`. End-user docs and release notes live in `docs/`.

## Build, Test, and Development Commands
- `pnpm dev`: run the full Tauri desktop app locally.
- `pnpm dev:renderer`: run only the Vite renderer on `http://localhost:3000`.
- `pnpm build`: build desktop bundles.
- `pnpm build:renderer`: build the frontend only.
- `pnpm typecheck`: run TypeScript checks without emitting files.
- `pnpm test:unit`: run Vitest once.
- `pnpm test:unit:watch`: run Vitest in watch mode.
- `cargo test --manifest-path src-tauri/Cargo.toml`: run Rust tests.
- `pnpm format` / `pnpm format:check`: format or verify frontend files.
- `cargo fmt --all --manifest-path src-tauri/Cargo.toml`: format Rust code.

## Coding Style & Naming Conventions
Frontend code follows Prettier defaults already used in the repo: 2-space indentation, double quotes, and semicolons. Use `PascalCase` for React components (`ProviderList.tsx`), `camelCase` for hooks/utilities (`useProxyStatus.ts`), and keep imports rooted at `@/` when referencing `src/`.

Rust files use `snake_case` module names and standard `cargo fmt` output. Match existing boundaries: UI state stays in hooks/components, while filesystem, proxy, and provider logic stays in Tauri services/commands.

## Testing Guidelines
Use Vitest with Testing Library for renderer tests. Name files `*.test.ts` or `*.test.tsx` under `tests/`. Shared setup, jsdom polyfills, and MSW mocks are already configured in `tests/setupGlobals.ts` and `tests/setupTests.ts`.

Add Rust regression tests in `src-tauri/tests/` for command, proxy, import/export, and config behavior. No fixed coverage threshold is enforced, but Vitest emits `text` and `lcov`, so add tests for new UI states and cross-process logic.

## Commit & Pull Request Guidelines
Recent commits use concise conventional prefixes such as `feat`, `fix`, `docs`, and `style`, sometimes with a scope like `feat(providers): ...`. Keep commits small, imperative, and focused.

PRs should explain the user-visible change, list the commands/tests you ran, link the relevant issue, and include screenshots for UI updates. Call out config format changes, migrations, or platform-specific behavior explicitly.
