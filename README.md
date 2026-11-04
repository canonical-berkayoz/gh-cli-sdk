# GitHub CLI SDK for Workshop

This SDK provides the official GitHub CLI (`gh`) for working with pull
requests, issues, releases and other GitHub resources from the terminal. A
GitHub token can be supplied through the `github-token` secret plug, and
`gh`'s configuration and login state are persisted on the host across
workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: gh-demo
base: ubuntu@24.04
sdks:
  - name: gh-cli
    channel: latest/stable
```

This demonstrates a workshop with `gh` available on the `PATH`.

---

## Using the SDK

### Prerequisites, project layout

1. No other SDKs are required.
2. `gh` works inside any project directory; it picks up the repository from
   the project's git remotes.
3. Launch and refresh don't change your git configuration. To let `git` use
   `gh` for HTTPS authentication, run `gh auth setup-git` inside the
   workshop.

### Authenticate

Either log in interactively; the login is kept across workshop updates:

```bash
workshop exec -- gh auth login
```

Or supply a token without storing it in the workshop. This is optional; see
[`github-token`](#github-token) for the one-time setup.

For a single command, a `GH_TOKEN` or `GITHUB_TOKEN` you pass in (for
example with `workshop exec --env GH_TOKEN`) takes precedence over the
plug.

### Use GitHub from the workshop

```bash
workshop exec -- gh pr list
workshop exec -- gh issue create
```

---

## Plugs (resources this SDK consumes)

### `gh-config`

- Interface: `mount`
- Workshop target: `/home/workshop/.config/gh`
- Purpose: Persists `gh` configuration and login state between workshop
  updates.
- Connection: automatic.

### `github-token`

- Interface: `secret`
- Purpose: Supplies a GitHub token to `gh` as `GH_TOKEN` on each
  invocation. If the plug is not connected, `gh` falls back to its own
  login state.
- Connection: manual. Store the token in your host keyring, add a `secret`
  slot to the `system` SDK in your workshop definition, then run
  `workshop connect <workshop>/gh-cli:github-token <workshop>/system:<slot>`.
  See the [Workshop secrets documentation](https://ubuntu.com/workshop/docs/how-to/develop-with-workshops/provide-secrets)
  for the slot definition.
- Exposure: while connected, any process in the workshop can request the
  token. The token is also visible to `gh` and every process `gh` starts.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [GitHub CLI manual](https://cli.github.com/manual/)

---

## Community and support

- [GitHub CLI discussions](https://github.com/cli/cli/discussions)
- Please review our [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct)
  before participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- Open issues or pull requests on the [official repository](https://github.com/canonical-berkayoz/gh-cli-sdk).

---

## License and copyright

Copyright 2026 Canonical Ltd.

This SDK's packaging is licensed under the GNU General Public License
version 3 only. It ships the GitHub CLI, copyright GitHub, Inc., which is
licensed under the [MIT License](https://github.com/cli/cli/blob/trunk/LICENSE).
