# containerfiles

## elixir-ls

Image for [`elixir-ls`, the most-used elixir language server](https://github.com/elixir-lsp/elixir-ls).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "elixir"
language-server = { command = "podman", args = ["run", "--rm", "--network=none", "-i", "-v", "/home/oskar/development/:/home/oskar/development/", "ghcr.io/oskarkook/containerfiles/elixir-ls:latest"], config = { elixirLS.dialyzerEnabled = false } }
```

Change the volume path as needed.

## marksman

Image for [`marksman`, a Markdown language server](https://github.com/artempyanykh/marksman/).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "markdown"
language-server = { command = "podman", args = ["run", "--rm", "--network=none", "-i", "-v", "/home/oskar/:/home/oskar/", "ghcr.io/oskarkook/containerfiles/marksman:latest"] }
```

Change the volume path as needed.
