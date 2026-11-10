# Contributing

Thanks for helping. Issues and pull requests are both welcome.

## Reporting a bug

Please include:

- `flutter --version`
- the smallest test that shows the problem (a `testWidgets` with an inline
  widget is ideal)
- what you expected, and the full failure output

## Making a change

```sh
flutter pub get
dart format .
flutter analyze
flutter test
(cd example && flutter pub get && flutter analyze && flutter test)
```

CI runs the package steps on the oldest supported Flutter (3.27, with
`flutter pub get --no-example`) and on the current stable. The example needs
Flutter 3.32 (Dart 3.8) or newer for `flutter_riverpod` 3, so CI runs it on
the current stable only.

Some ground rules:

- **No runtime dependencies** beyond `flutter` and `flutter_test`. State
  libraries belong in `example/`.
- **Every public API has dartdoc**, enforced by `public_member_api_docs`.
- **Failure messages are part of the API.** If you change one, change the test
  that pins it, and keep it telling the reader *what* rebuilt.
- Keep commits small, with messages that say why.

## Pull requests

Run the commands above before pushing; every CI check must pass before a
pull request can be merged. The `PR rules` check enforces these:

- **Issue first.** Open an issue and wait until a maintainer labels it
  `accepted`, then write `Closes #N` in the pull request.
- **Tests.** Every change to `lib/`, `bin/` or `example/lib/` needs a test
  change under `test/` or `example/test/`. A bug fix needs a test that fails
  without the fix.
- **Small.** At most 400 changed lines, one issue per pull request.
- **No suppressions.** No `// ignore:`, no `// ignore_for_file:`, no changes
  to `analysis_options.yaml` (in the package or `example/`). Fix the finding.
- **At most 2 open pull requests** per person.

If a rule really does not fit your change, say why in the description. A
maintainer can waive it with a label (`no-tests-needed`, `large-change` or
`suppression-ok`).

## Legal notice

By submitting a contribution you confirm that you wrote it, that you have the
right to submit it, and that it may be distributed under the project license
(see [LICENSE](LICENSE)).

## AI-assisted contributions

Using AI tools is fine; please just be open about it.

- Say in the PR description which AI tools you used, if any: none, some or
  most, plus the tool or agent and the model (for example "Claude Code,
  Opus 5.5" or "Copilot, GPT-5").
- Please write the PR description in your own words; short and plain is fine.
  It shows us what you understood about the change. Please don't paste
  generated text.
- Read, run and test every line you submit, and be ready to explain it in
  review. If you don't understand what a line does, don't submit it.
- Keep the change focused. Agents like to add unrelated "improvements",
  long comments and dead code; remove them before you open the pull request.
- Pull requests that are raw, unreviewed agent output will be closed.
- If you use an agent, point it at [AGENTS.md](AGENTS.md).
- If the disclosure is missing, a check adds a reminder to the PR. It does
  not fail the PR, but a maintainer will ask about it before merging.

## Releasing

1. Update `version` in `pubspec.yaml` and add a `CHANGELOG.md` entry.
2. `dart pub publish --dry-run` must report zero warnings.
3. Tag `vX.Y.Z` and publish.

## Stay in touch

Don't forget to follow [@saimskywalker](https://github.com/saimskywalker) on
GitHub to see new releases and other open-source projects as they come out.
