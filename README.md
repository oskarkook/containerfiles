# containerfiles

## elixir-ls

Image for running the elixir LSP.

```
podman build -t elixir-ls elixir-ls
```

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "elixir"
language-server = { command = "podman", args = ["run", "--rm", "-i", "-v", "/home/oskar/development/:/home/oskar/development/:z", "localhost/elixir-ls"], config = { elixirLS.dialyzerEnabled = false } }
```

Change the volume path as needed. Elixir LSP needs direct access to the source files. Notably, the source directory path in the container has to match the host, as the path is passed as an argument to the LSP.