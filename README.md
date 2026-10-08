# Loggerhead MCP server

[![M8ven Score](https://m8ven.ai/badge/mcp/rubics-code-loggerhead-mcp-so8512?v=a299c758f1e53f0a562b1036791bde0b)](https://m8ven.ai/mcp/rubics-code-loggerhead-mcp-so8512?s=readme)

**Your coding agent reads your app's logs, metrics, and traces.**

[Loggerhead](https://getloggerhead.com) is a native Mac app that receives OpenTelemetry logs, metrics, and traces from the software you run on your own Mac, and stores them in a DuckDB file on your disk. The MCP server runs inside the app. This repository holds `loggerhead-mcp`, the small bridge that connects Claude, Cursor, Codex, Windsurf, or any other MCP client to it.

The tools read the telemetry. Seven tools change the lint rules of a workspace, each with a reason that the user sees and can undo in the app. One tool marks a finding fixed after a code change. The endpoint listens on loopback only. Nothing leaves the Mac.

> **The Loggerhead app must be installed and running.** [Download it](https://getloggerhead.com/download) first. The bridge exits when the app is not running. The free tier includes the MCP server.

## Install

### Claude Desktop

Download `loggerhead-mcp.mcpb` from the [latest release](https://github.com/rubics-code/loggerhead-mcp/releases/latest) and open it. Claude Desktop installs the extension.

### Claude Code

```bash
claude mcp add loggerhead -- "/Applications/Loggerhead.app/Contents/Resources/loggerhead-mcp"
```

### Cursor, Codex, Windsurf, and other stdio clients

Every stdio client takes the same two values. The `.mcp.json` in this repository is the file to copy:

```json
{
  "mcpServers": {
    "loggerhead": {
      "command": "/Applications/Loggerhead.app/Contents/Resources/loggerhead-mcp"
    }
  }
}
```

The app itself writes this entry for you: open **Settings › AI** and click **Add** beside your tool.

The bridge finds the app by itself on a fixed ladder of local ports. You set no port, and it needs no permission to read the app's files. Full instructions: [Set up the MCP server](https://getloggerhead.com/docs/mcp-server/setup/).

## Tools

| Tool | Purpose |
| --- | --- |
| `query_logs` | Search log records by text, severity, and time. |
| `query_metrics` | Read time-series metrics by name and time range. |
| `query_traces` | Find traces by service, operation, duration, and time. |
| `get_trace` | Read every span of one trace. |
| `list_environments` | List every workspace and environment on this Mac, with the receiver ports. |
| `get_receiver_info` | Report the effective OTLP ports and the addresses to send telemetry to. |
| `query_improvements` | List the telemetry-lint findings: noisy logs, unbounded labels, secrets, lost trace context. |
| `list_lint_rules` | List the lint rules of a workspace with their state, thresholds, exceptions, ignored findings, and recent changes. |
| `query_ai_findings` | List the findings of the last AI analysis run. It does not start a run. |
| `set_lint_rule_enabled` | Switch a lint rule on or off for a workspace, with a reason. |
| `set_lint_threshold` | Set one threshold of a lint rule for a workspace, with a reason. |
| `reset_lint_threshold` | Put one threshold of a lint rule back to its default, with a reason. |
| `add_lint_exception` | Hide the findings of one lint rule for a service, metric, attribute key, operation, or log message, with a reason. |
| `remove_lint_exception` | Remove one lint rule exception, with a reason. |
| `ignore_finding` | Ignore one lint finding by its id, with a reason. |
| `unignore_finding` | Stop the ignore of one lint finding, with a reason. |
| `mark_finding_fixed` | Mark one lint finding fixed after a code change, so the app judges it by the telemetry after the fix. |

One resource, `loggerhead://stats`, reports the total counts of logs, metrics, and spans. Every parameter is in the [tools reference](https://getloggerhead.com/docs/mcp-server/tools-reference/).

## Privacy Policy

The bridge and the server read telemetry from the Loggerhead app on the same Mac and hand it to the MCP client you connected. They collect nothing about you, they store nothing of their own, they share nothing with a third party, and they open no network connection except to `127.0.0.1`. Data retention is the app's: the telemetry lives in a DuckDB file on your disk for as long as the app's retention setting keeps it, and you can delete the file. The full policy of the publisher, Rubics Code, is at https://getloggerhead.com/privacy. Contact: support@getloggerhead.com.

## Registry

The registry name of this server is `com.getloggerhead/loggerhead`.

- MCP Registry name: `mcp-name: com.getloggerhead/loggerhead`

## Licence

The `loggerhead-mcp` bridge and the files in this repository are under the [MIT licence](LICENSE).

The bridge connects to Loggerhead, a proprietary Mac app by Rubics Code (ABN 40 440 756 434), Australia. The app has a free tier, and the source of the app is not public. The [Loggerhead terms of service](https://getloggerhead.com/terms) apply to the app.
