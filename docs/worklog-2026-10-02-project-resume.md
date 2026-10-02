# Focused Project Compact & Resume compatibility

## Scope and source

Fork base: `e8ef8701409e002d27664ed8f44d5e0b010d03cb` (2.1.20).
Selectively adapted from Maximapple's upstream [#883](https://github.com/totec448-spec/chat-on-steroids/pull/883),
[#888](https://github.com/totec448-spec/chat-on-steroids/pull/888) and
[#901](https://github.com/totec448-spec/chat-on-steroids/pull/901).
The upstream reports measured ChatGPT retaining undisplayed page surfaces and reusing the
composer during Project navigation. This work reproduced the corresponding source-level
failures using the shipped DOM/Fiber readers; it did not install or exercise a live app.

Only Project entry and current-page filtering are incorporated. Core mention settling,
continuation-marker grammar, unrelated renderer fallbacks, versions and dependencies are
unchanged. The base tree retains the fork's Helium, late-Stop retry and `sourceTurnId`
customizations. No merge, deployment or local-computer action is part of this patch.

## First wrong boundaries and repairs

- `composer()` selected the first legacy editor even when it belonged to an older
  `display:none` page. Select only current-page candidates; two displayed editors remain
  ambiguous, including the id-less native editor.
- Project entry required a new editor node and the removed folder-icon test id. Require one
  same-origin native header link to the exact Project, then the Project route, empty writable
  attachment-free editor and no current-page turns. Preserve the 60-second source readiness
  and separate 12-second transition deadlines, one click and cancellation checks.
- DOM turns, shell generation hints and MAIN Fiber turns included retained source pages.
  Skip native page surfaces with computed `display:none`, including nested ancestors, using
  the same rule in both worlds. The destination's conversation identity stays independent
  of the retained source conversation.
- Update the existing Project contract paragraph in `AGENTS.md` and record original credit
  in `CONTRIBUTORS.md`; do not replace any other repository instructions.

## Regression evidence

Before changing production files, seven focused new upstream-derived DOM/Fiber cases failed
against the fork's original readers: wrong composer, old transcript included, ambiguous
composer accepted, three Project entry waits timed out, and source Fiber turns included.
After the patch, current-page positive cases and neighboring negative cases pass: same editor,
retained headers/turns, nested hidden surfaces, id-less editor, two editors/links, foreign
header origin/route, source loading, destination draft/read-only/attachment, cancellation,
source still shown and bounded transition failure.

## Actual checks

The files were materialized at the exact fork SHA. Focused test dependencies used the fork's
versions (`vitest` 5.0.0, `jsdom` 30.0.1), installed outside the source tree with scripts disabled;
no tracked manifest or lockfile was changed.

Commands run against the final source:

```sh
./node_modules/.bin/vitest run test/chatgpt-dom-input.test.ts test/fiber.test.ts --maxWorkers=1
# 2 files passed; 255 tests passed
./node_modules/.bin/vitest run test/content-script.test.ts -t 'Project resume enters|acquires its id when a Project|fresh chat the app opened' --maxWorkers=1
# 1 file passed; 40 tests passed, 727 unrelated tests skipped
node --check extension/chatgpt-dom.js
node --check extension/fiber.js
git diff --check
```

The two JavaScript syntax checks and whitespace check passed. The existing Vite config
emits a CommonJS/ESM future-loader warning, without test failure. No repository-wide suite,
`verify`, `verify:ci`, build, packaging or full typecheck was run, following the repository's
upstream-integration exception. No installed-extension or signed-in live Project compaction
is claimed; that remains a separate acceptance gate.
