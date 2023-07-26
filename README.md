# containerfiles

## elixir-ls

Image for [`elixir-ls`, the most-used elixir language server](https://github.com/elixir-lsp/elixir-ls).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "elixir"
language-server = { command = "podman", args = ["run", "--rm", "-i", "-v", "/home/oskar/development/:/home/oskar/development/:z", "ghcr.io/oskarkook/containerfiles/elixir-ls:latest"], config = { elixirLS.dialyzerEnabled = false } }
```

Change the volume path as needed. `elixir-ls` needs direct access to the source files. Notably, the source directory path in the container has to match the host, as the path is passed as an argument to the language server by the editor.