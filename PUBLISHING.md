# Publishing Dark++ to the Zed extension registry

## Release information

- Repository: https://github.com/codetin/DarkPPZed
- Extension ID: `darkplusplus-theme`
- Display / theme name: `Dark++`
- Version: `0.1.0`
- License: MIT
- Extension root: repository root (no `path` override needed)

A GitHub release or ZIP is not a registry submission. Zed builds and publishes extensions after an index pull request is accepted and merged.

## Before submitting

1. Read the [publishing prerequisites](https://zed.dev/docs/extensions/publishing/prerequisites), [publishing guide](https://zed.dev/docs/extensions/publishing/publishing-guide), and [AI policy](https://github.com/zed-industries/extensions/blob/main/AI_POLICY.md).
2. Install the exact release commit as a **dev extension** and manually test it in Zed. Testing only a loose JSON theme is not the same as testing extension installation. Check the UI, syntax highlighting, terminal, and the documented optional C# semantic mappings.
3. Confirm there is no equivalent theme already in the registry. This is a port of the original customized Dark++, not a claim to provide the stock VS Code Dark+ theme.
4. Review the source and take responsibility for its behavior. The registry requires human-authored pull request explanations and does not allow autonomous agents to contribute on your behalf.

## Submit the registry entry yourself

Fork `zed-industries/extensions` to your personal GitHub account. Clone that fork **outside this theme repository**, then run the following from the fork's directory:

```sh
git switch -c add-darkplusplus-theme
git submodule add https://github.com/codetin/DarkPPZed.git extensions/darkplusplus-theme
git -C extensions/darkplusplus-theme checkout v0.1.0
```

The tagged release commit must also remain reachable from the theme repository's `master` branch. Manually test this exact commit before submitting.

Add the following entry to the fork's top-level `extensions.toml`:

```toml
[darkplusplus-theme]
submodule = "extensions/darkplusplus-theme"
version = "0.1.0"
```

Install the index repository's dependencies and run its required sorting command:

```sh
pnpm install --frozen-lockfile
pnpm sort-extensions
git diff --check
git add .gitmodules extensions.toml extensions/darkplusplus-theme
git commit -m "Add Dark++ theme"
git push -u origin add-darkplusplus-theme
```

Open a pull request against `zed-industries/extensions:main`. Write the explanation in your own words, describe how you tested the extension, and complete the contribution checklist. Do not claim that optional C# settings are installed automatically. Respond to maintainer feedback within the time required by the publishing guide.

Only after the PR is merged and registry packaging succeeds should the README advertise installation from the extension store.

## Future updates

Increment `extension.toml`'s version, test and push the release commit, then update the submodule revision and matching version in the registry through a new PR.
