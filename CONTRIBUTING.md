# Contributing

This repository is generated from Wakeshift's main repository. The plugin comes from `plugins/agent-plugins/standalone/antigravity` there, and its `bin/wakeshift` is the shared launcher, `plugins/agent-plugins/launcher.sh`. Wakeshift's release workflow writes the plugin's pin (`wakeshift.env`, `SHA256SUMS`) and the `version` in `plugin.json`. The launcher's tests are published with the connector in [wakeshift/connectors](https://github.com/wakeshift/connectors), under `plugins/agent-plugins`.

We read every issue. Report bugs, ask questions and suggest changes in this repository's issues.

Pull requests are welcome, though we don't guarantee we'll accept every one. We apply an accepted pull request to Wakeshift's main repository, keeping you as the commit's author. It arrives here with the next publish, and we close the pull request here with a link to the commit.

Before you open one:

- Run the launcher's tests in [wakeshift/connectors](https://github.com/wakeshift/connectors): `go test ./plugins/agent-plugins/`.
- Check the plugin with agy: `agy plugin validate --strict .`
- Keep the change focused, and say what it fixes or adds.
- Don't include keys, tokens or other secrets, including in tests.
- Change only the code. The generated files (`LICENSE`, `.source-commit`, `bin/wakeshift`, `wakeshift.env`, `SHA256SUMS` and the `version` in `plugin.json`) are rewritten at each publish.

Report a security problem privately to support@wakeshift.ai, not in an issue: see [SECURITY](SECURITY.md).

By contributing, you agree your contribution is licensed under the Apache License 2.0.
