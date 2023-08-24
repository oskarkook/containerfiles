# containerfiles

## Language server images

In this section, images that wrap language servers are documented.

Some language servers monitor the parent process that started the server, as documented in the [language server protocol specification](https://github.com/microsoft/language-server-protocol/blob/gh-pages/_specifications/specification-3-16.md#server-lifetime). Most commonly, this can be encountered with language servers that are built on top of [vscode-languageserver-node](https://github.com/microsoft/vscode-languageserver-node/). To achieve this, the client provides the PID that started the server process during the initialization sequence. When starting containers in rootless mode, podman creates a new PID namespace and therefore the server will be unable to see the process that spawned it.

To make these language servers work with containers, the best option is to not allow the editor to send the PID to the server. In some editors this is configurable (e.g. using [`before_init` in neovim](https://neovim.io/doc/user/lsp.html#lsp-core)).

In Helix, this is not configurable. To make Helix work with all LSPs, [I have a fork where the process ID is always set to null](https://github.com/oskarkook/helix/tree/lsp-process-id-null). If you need this, you are welcome to build and use Helix from the fork.

### elixir-ls

Image for [`elixir-ls`, the most-used elixir language server](https://github.com/elixir-lsp/elixir-ls).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "elixir"
language-server = { command = "podman", args = ["run", "--rm", "--network=none", "-i", "-v", "/home/oskar/development/:/home/oskar/development/", "ghcr.io/oskarkook/containerfiles/elixir-ls:latest"], config = { elixirLS.dialyzerEnabled = false } }
```

Change the volume path as needed.

### marksman

Image for [`marksman`, a Markdown language server](https://github.com/artempyanykh/marksman/).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "markdown"
language-server = { command = "podman", args = ["run", "--rm", "--network=none", "-i", "-v", "/home/oskar/:/home/oskar/", "ghcr.io/oskarkook/containerfiles/marksman:latest"] }
```

Change the volume path as needed.

### bash-language-server

Image for [`bash-language-server`](https://github.com/bash-lsp/bash-language-server).

To use with Helix, add to `languages.toml` configuration:

```toml
[[language]]
name = "bash"
language-server = { command = "podman", args = ["run", "--rm", "--network=none", "-i", "-v", "/home/oskar/:/home/oskar/", "ghcr.io/oskarkook/containerfiles/bash-language-server:latest"] }
```

Change the volume path as needed.
