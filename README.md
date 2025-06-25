<div align = "right">
<a href="docs/readme/README.zh-cn.md">简体中文(待更新)</a>
</div>

# agenterra-rmcp

[![Crates.io Version](https://img.shields.io/crates/v/agenterra-rmcp)](https://crates.io/crates/agenterra-rmcp)
[![docs.rs](https://docs.rs/agenterra-rmcp/badge.svg)](https://docs.rs/agenterra-rmcp)
![Coverage](docs/coverage.svg)

An Agenterra fork of the official Rust Model Context Protocol SDK implementation with tokio async runtime.

> **Note**: This is a fork of the [official Rust SDK](https://github.com/modelcontextprotocol/rust-sdk) for the Model Context Protocol. We maintain this fork to provide a stable, published version on crates.io while staying in sync with upstream changes.

## Attribution

This project is based on the official Model Context Protocol Rust SDK developed by Anthropic and the MCP community. The original repository can be found at: https://github.com/modelcontextprotocol/rust-sdk

All credit for the core implementation goes to the original authors. This fork exists to:
- Provide published crates on crates.io under the `agenterra-rmcp` namespace
- Enable easier integration for projects that need a stable, versioned dependency
- Maintain compatibility with upstream changes


This repository contains the following crates:

- [agenterra-rmcp](crates/rmcp): The core crate providing the MCP protocol implementation (If you want to get more information, please visit [agenterra-rmcp](crates/rmcp/README.md))
- [agenterra-rmcp-macros](crates/rmcp-macros): A procedural macro crate for generating MCP tool implementations (If you want to get more information, please visit [agenterra-rmcp-macros](crates/rmcp-macros/README.md))

## Usage

### Import the crate

```toml
agenterra-rmcp = { version = "0.1.5", features = ["server"] }
## or from git
agenterra-rmcp = { git = "https://github.com/agenterra/agenterra-rmcp", branch = "main" }
```
### Third Dependencies
Basic dependencies:
- [tokio required](https://github.com/tokio-rs/tokio)
- [serde required](https://github.com/serde-rs/serde)



### Build a Client
<details>
<summary>Start a client</summary>

```rust, ignore
use agenterra_rmcp::{ServiceExt, transport::{TokioChildProcess, ConfigureCommandExt}};
use tokio::process::Command;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = ().serve(TokioChildProcess::new(Command::new("npx").configure(|cmd| {
        cmd.arg("-y").arg("@modelcontextprotocol/server-everything");
    }))?).await?;
    Ok(())
}
```
</details>

### Build a Server

<details>
<summary>Build a transport</summary>

```rust, ignore
use tokio::io::{stdin, stdout};
let transport = (stdin(), stdout());
```

</details>

<details>
<summary>Build a service</summary>

You can easily build a service by using [`ServerHandler`](crates/rmcp/src/handler/server.rs) or [`ClientHandler`](crates/rmcp/src/handler/client.rs).

```rust, ignore
let service = common::counter::Counter::new();
```
</details>

<details>
<summary>Start the server</summary>

```rust, ignore
// this call will finish the initialization process
let server = service.serve(transport).await?;
```
</details>

<details>
<summary>Interact with the server</summary>

Once the server is initialized, you can send requests or notifications:

```rust, ignore
// request
let roots = server.list_roots().await?;

// or send notification
server.notify_cancelled(...).await?;
```
</details>

<details>
<summary>Waiting for service shutdown</summary>

```rust, ignore
let quit_reason = server.waiting().await?;
// or cancel it
let quit_reason = server.cancel().await?;
```
</details>


## Examples

See [examples](examples/README.md)

## OAuth Support

See [oauth_support](docs/OAUTH_SUPPORT.md) for details.


## Related Resources

- [MCP Specification](https://spec.modelcontextprotocol.io/specification/2024-11-05/)
- [Schema](https://github.com/modelcontextprotocol/specification/blob/main/schema/2024-11-05/schema.ts)

## Related Projects
- [containerd-mcp-server](https://github.com/jokemanfire/mcp-containerd) - A containerd-based MCP server implementation

## Development

### Tips for Contributors
See [docs/CONTRIBUTE.MD](docs/CONTRIBUTE.MD) to get some tips for contributing.

### Using Dev Container
If you want to use dev container, see [docs/DEVCONTAINER.md](docs/DEVCONTAINER.md) for instructions on using Dev Container for development.