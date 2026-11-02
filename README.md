# Wakeshift for Antigravity CLI

[Wakeshift](https://wakeshift.ai) wakes your AI agent's conversation when something happens in an app it's connected to. This is Wakeshift's plugin for [Antigravity CLI](https://antigravity.google) (`agy`). Its hooks deliver Wakeshift messages to the conversation that subscribed to them: within a minute after the agent finishes a turn, or else at the start of its next one.

One install serves every app built on Wakeshift.

## Install

```sh
agy plugin install https://github.com/wakeshift/antigravity-plugin
```

Then restart agy and reopen your conversation (`agy --continue`). There's nothing to approve: agy runs a plugin's hooks once it's installed. To update, run the same command again. agy replaces the installed copy.

You don't normally type this yourself. When your agent first needs it, a Wakeshift app's tool result offers the command, and your agent asks you before it runs it.

## What it does

The plugin is this repository's root, as agy installs it into `~/.gemini/config/plugins/wakeshift`:

- **`hooks.json`:** agy's hooks (`SessionStart`, `PostToolUse` for MCP tool calls, `PreInvocation` and `Stop`), each running `bin/wakeshift`.
- **`bin/wakeshift`:** a small POSIX shell launcher that the hooks run.
- **`wakeshift.env`:** the Wakeshift server the plugin trusts, and the version of the connector it pins.
- **`SHA256SUMS`:** the connector's checksum for each platform.

On a hook's first run, the launcher does three things:

1. **Downloads the connector.** It fetches it from that version's [signed release](https://github.com/wakeshift/connectors/releases), checks it against the plugin's `SHA256SUMS`, and installs it to `~/.wakeshift/bin/wakeshift`. A download that doesn't match is deleted, and nothing is installed.
2. **Sets up the connector.** It runs `wakeshift install --harness antigravity --issuer <server> --yes --plugin`. That makes the connector trust that Wakeshift server. It also removes the `wakeshift` entry an earlier `curl … | sh` install wrote to `~/.gemini/config/hooks.json`, so the hooks don't run twice.
3. **Runs the hook.**

Later runs only check that the connector is the pinned version or newer, so they use no network. A connector that's already newer stays.

## Other Wakeshift servers

The `main` branch trusts production, `https://app.wakeshift.ai`. The `staging` branch trusts Wakeshift's staging server, `https://app.staging.wakeshift.ai`:

```sh
agy plugin install https://github.com/wakeshift/antigravity-plugin/tree/staging
```

## Uninstall

```sh
agy plugin uninstall wakeshift
```

Then `~/.wakeshift/bin/wakeshift uninstall --harness antigravity` removes what the connector set up: the server it trusted and the conversations it connected.

## Contributing and security

This repository is generated from Wakeshift's main repository: the plugin from `plugins/agent-plugins/standalone/antigravity`, its launcher from `plugins/agent-plugins/launcher.sh`, and its pin by Wakeshift's release workflow. The launcher's tests are published with the connector in [wakeshift/connectors](https://github.com/wakeshift/connectors), under `plugins/agent-plugins`. See [CONTRIBUTING](CONTRIBUTING.md).

Report a security problem privately to support@wakeshift.ai, not in an issue ([SECURITY](SECURITY.md)).

## License

Apache License 2.0; see [LICENSE](LICENSE) and [NOTICE](NOTICE).
