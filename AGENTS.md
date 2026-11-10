# AGENTS.md

Rules for AI coding agents working in this repository. People should read
[CONTRIBUTING.md](CONTRIBUTING.md); everything here applies to them too. The
`PR rules` check enforces the rules marked (checked); a pull request that
breaks one cannot be merged.

## What rebuildcop is

A Flutter test helper that counts widget rebuilds through Flutter's
`debugOnRebuildDirtyWidget` hook and fails a `flutter test` run over a rebuild
budget. Counting lives in `lib/src/tally/` (`RebuildWindow`), the matchers and
baseline files in `lib/src/verdict/`, the failure tables in
`lib/src/output/`, and the guard that keeps the package out of an app's
`lib/` in `lib/guard.dart`, `lib/src/guard_*.dart` and `bin/check.dart`. The
package is in maintenance: new features are on hold.

Read before you touch:

- the window or a matcher: `doc/api.md`, `doc/budgets.md` and the
  "Failure messages are part of the API" rule in CONTRIBUTING.md.
- the guard: the "Guard" section of `doc/api.md`, `test/guard/` and the
  package fixtures in `test/support/package_fixture.dart`.
- an example: `example/README.md` and `doc/examples.md`; the examples in
  `example/lib/` are tested in `example/test/`.

## Commands

Use the Flutter SDK, 3.27 or newer for the package; the example needs 3.32
(Dart 3.8) or newer. Every one of these must pass before you open or update a
pull request. CI runs the same ones:

```bash
flutter pub get --no-example
dart format --output=none --set-exit-if-changed bin lib test
flutter analyze bin lib test
flutter test
(cd example && flutter pub get && dart format --output=none --set-exit-if-changed . \
  && flutter analyze && flutter test)
flutter pub get && flutter pub publish --dry-run
```

CI runs the package steps on Flutter 3.27.4 and 3.47.7 (formatting on 3.47.7
only) and the example and the dry run on 3.47.7.

Run one test file while iterating: `flutter test test/window_test.dart`.

## Stop and ask the human

Some things must come from the real world. An agent must never invent them.
Stop and ask the person you work for to provide them:

- **Rebuild counts and output.** Every count in a test, a baseline file, the
  README or `doc/` is measured with `--dart-define=PRINT_REBUILDS=true` on a
  stated Flutter version. Paste real output; never write a count, a failure
  table or a print-mode line by hand.
- **Flutter behaviour.** Which widgets the hook reports (render-object widgets
  are mostly not counted), and why, comes from the Flutter source, docs or an
  issue. Cite it; if you cannot find one, say so; do not guess.
- **Supported versions.** A claim that something works on a Flutter or state
  library version needs a real run on that version.
- **An accepted issue.** If the change has no issue labelled `accepted`, stop
  and ask the human to open one and wait for a maintainer.

## Code

- **English only** for code, identifiers, comments, commit messages and docs.
  Answer the person you work for in their language.
- Target the minimum SDK in `pubspec.yaml` (Dart 3.6, Flutter 3.27; the
  example: Dart 3.8). Do not use language or library features newer than
  that.
- `bin/check.dart` and the guard it imports must not import Flutter: they run
  on the plain Dart VM.
- Public API has doc comments (`public_member_api_docs`). Other comments are
  sparse: only where the code is not self-evident. Do not add comments, doc
  comments or type annotations to code you did not change.

## Tests (checked)

- Every change to `lib/`, `bin/` or `example/lib/` comes with a test change
  under `test/` or `example/test/`. A bug fix has a test that fails without
  the fix; check that it does before you open the pull request.
- A changed failure message changes the test that pins it, and still tells
  the reader what rebuilt.

## Never

- Never silence the analyzer (checked): no `// ignore:`,
  `// ignore_for_file:`, no changes to either `analysis_options.yaml`. Fix the
  finding instead.
- Never delete, skip, loosen or rewrite an existing test, expectation or
  budget to make CI pass. Raising a budget is a review decision, not a fix.
  If a test looks wrong, say so in the issue.
- Never change what users depend on: `RebuildWindow` and its members, the
  matcher names and parameters, failure message wording, the
  `PRINT_REBUILDS` and `UPDATE_REBUILD_BUDGETS` defines, the baseline JSON
  layout, `expectRebuildBudgetIsTestOnly` and `dart run rebuildcop:check`.
- Never bump the version in `pubspec.yaml`, edit a released `CHANGELOG.md`
  section, publish, tag or release. Add your entry at the top of
  `CHANGELOG.md` under `## Unreleased` (create the heading if it is missing).
- Never add a dependency (the package depends only on `flutter` and
  `flutter_test`; state libraries belong in `example/`) or change CI
  workflows unless the issue asks for it.
- Never add unrelated "improvements": no drive-by refactors, renames,
  reformatting of untouched code or dead code.
- Never open pull requests, issues or comments on your own. The human reviews
  your work and posts it.

## Pull requests (checked)

- First an issue, labelled `accepted` by a maintainer. The pull request says
  `Closes #N`.
- One issue per pull request, at most 400 changed lines.
- At most 2 open pull requests per person.
- Fill in the pull request template, including `### AI use` with the tool and
  model.
- The person submitting has read, run and understood every line, and writes
  the description in their own words. Pull requests that are raw, unreviewed
  agent output will be closed.
