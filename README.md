# containerfiles

## Language server images

Available images:

* `ghcr.io/oskarkook/containerfiles/elixir-ls:latest` - [`elixir-ls`](https://github.com/elixir-lsp/elixir-ls)
* `ghcr.io/oskarkook/containerfiles/marksman:latest` - [`marksman`](https://github.com/artempyanykh/marksman/)
* `ghcr.io/oskarkook/containerfiles/bash-language-server:latest` - [`bash-language-server`](https://github.com/bash-lsp/bash-language-server)
* `ghcr.io/oskarkook/containerfiles/dockerfile-language-server:latest` - [`dockerfile-language-server-nodejs`](https://github.com/rcjsuen/dockerfile-language-server-nodejs)

Some language servers monitor the parent process that started the server, as documented in the [language server protocol specification](https://github.com/microsoft/language-server-protocol/blob/gh-pages/_specifications/specification-3-16.md#server-lifetime). Most commonly, this can be encountered with language servers that are built on top of [vscode-languageserver-node](https://github.com/microsoft/vscode-languageserver-node/). To achieve this, the client provides the PID that started the server process during the initialization sequence.

When starting containers in rootless mode, podman creates a new PID namespace and therefore the server will be unable to see the process that spawned it. This will cause the server to close almost immediately after starting up. To make these language servers work with containers, the best option is to not allow the editor to send the PID to the server. In some editors this is configurable (e.g. using [`before_init` in neovim](https://neovim.io/doc/user/lsp.html#lsp-core)).

In Helix, this is not configurable. To make Helix work with all LSPs, [I have a fork where the process ID is always set to null](https://github.com/oskarkook/helix/tree/lsp-process-id-null). If you need this, you are welcome to build and use Helix from the fork.

To use these images with helix, one option is to directly invoke podman:

```toml
[[language]]
name = "elixir"
language-server = { command = "podman", args = ["run", "--network=none", "--rm", "-i", "-v", "/home:/home", "ghcr.io/oskarkook/containerfiles/elixir-ls:latest"] }
```

Another option is to wrap the call to podman in a script and have Helix call the script. This is left as an exercise to the reader.

