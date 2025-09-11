<div align="center">
  <img src="assets/shimmy-logo.png" alt="Shimmy Logo" width="300" height="auto" />
  
  # The 5MB Alternative to Ollama

  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
  [![Rust](https://img.shields.io/badge/rust-stable-brightgreen.svg)](https://rustup.rs/)
  [![CI](https://github.com/Michael-A-Kuykendall/shimmy/workflows/CI/badge.svg)](https://github.com/Michael-A-Kuykendall/shimmy/actions)
  [![Tests](https://img.shields.io/badge/Tests-Passing-brightgreen)](https://github.com/Michael-A-Kuykendall/shimmy/actions)
  [![Quality](https://img.shields.io/badge/Quality-Assured-success)](https://github.com/Michael-A-Kuykendall/shimmy/actions)
  [![Sponsor](https://img.shields.io/badge/❤️-Sponsor-ea4aaa?logo=github)](https://github.com/sponsors/Michael-A-Kuykendall)
</div>

**Shimmy will be free forever.** No asterisks. No "free for now." No pivot to paid.

**Fast, reliable local AI inference.** Shimmy provides OpenAI-compatible endpoints for GGUF models with comprehensive testing and automated quality assurance.

## What is Shimmy?

Shimmy is a **5.1MB single-binary** local inference server that provides OpenAI API-compatible endpoints for GGUF models. It's designed to be the **invisible infrastructure** that just works.

| Metric | Shimmy | Ollama | 
|--------|--------|--------|
| **Binary Size** | 5.1MB 🏆 | 680MB |
| **Startup Time** | <100ms 🏆 | 5-10s |
| **Memory Overhead** | <50MB 🏆 | 200MB+ |
| **OpenAI Compatibility** | 100% 🏆 | Partial |
| **Port Management** | Auto 🏆 | Manual |
| **Configuration** | Zero 🏆 | Manual |

## 🎯 Perfect for Developers

- **Privacy**: Your code stays on your machine  
- **Cost**: No per-token pricing, unlimited queries  
- **Speed**: Local inference = sub-second responses  
- **Integration**: Works with VSCode, Cursor, Continue.dev out of the box  

**BONUS:** First-class LoRA adapter support - from training to production API in 30 seconds.

## Quick Start (30 seconds)

### Installation

```bash
# Install from crates.io (Linux, macOS, Windows)
cargo install shimmy

# Or download pre-built binary (Windows only)
curl -L https://github.com/Michael-A-Kuykendall/shimmy/releases/latest/download/shimmy.exe
```

> **⚠️ Windows Security Notice**: Windows Defender may flag the binary as a false positive. This is common with unsigned Rust executables. **Recommended**: Use `cargo install shimmy` instead, or add an exclusion for shimmy.exe in Windows Defender.

### Get Models

Shimmy auto-discovers models from:
- **Hugging Face cache**: `~/.cache/huggingface/hub/`
- **Ollama models**: `~/.ollama/models/`
- **Local directory**: `./models/`
- **Environment**: `SHIMMY_BASE_GGUF=path/to/model.gguf`

```bash
# Download models that work out of the box
huggingface-cli download microsoft/Phi-3-mini-4k-instruct-gguf --local-dir ./models/
huggingface-cli download bartowski/Llama-3.2-1B-Instruct-GGUF --local-dir ./models/
```

### Start Server

```bash
# Auto-allocates port to avoid conflicts
shimmy serve

# Or use manual port
shimmy serve --bind 127.0.0.1:11435
```

Point your AI tools to the displayed port - VSCode Copilot, Cursor, Continue.dev all work instantly!

## 📦 Download & Install

### Package Managers
- **Rust**: [`cargo install shimmy`](https://crates.io/crates/shimmy)
- **VS Code**: [Shimmy Extension](https://marketplace.visualstudio.com/items?itemName=targetedwebresults.shimmy-vscode)
- **npm**: `npm install -g shimmy-js` *(coming soon)*
- **Python**: `pip install shimmy` *(coming soon)*

### Direct Downloads
- **GitHub Releases**: [Latest binaries](https://github.com/Michael-A-Kuykendall/shimmy/releases/latest)
- **Docker**: `docker pull shimmy/shimmy:latest` *(coming soon)*

### 🍎 macOS Installation & Setup

**Full compatibility confirmed!** Shimmy works flawlessly on macOS with Metal GPU acceleration.

#### Prerequisites

```bash
# Install Xcode Command Line Tools (required for compilation)
xcode-select --install

# Install dependencies
brew install cmake rust

# Verify Xcode path (important for compilation)
xcode-select --print-path
```

#### Installation

```bash
# Method 1: Install from crates.io (recommended)
cargo install shimmy --features llama

# Method 2: Install from source (for latest features)
git clone https://github.com/Michael-A-Kuykendall/shimmy.git
cd shimmy
cargo install --path . --features llama
```

#### macOS-Specific Configuration

**SDK Path Issues**: If you encounter compilation errors like `'stdio.h' file not found`, set the correct SDK path:

```bash
# Check available SDKs
ls "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/"

# Set SDK path (add to ~/.zshrc for persistence)
export SDKROOT="/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk"

# Reload shell configuration
source ~/.zshrc
```

**Non-standard Xcode Location**: If Xcode is installed in a custom location (e.g., Downloads folder):

```bash
# Point to correct Xcode installation
export SDKROOT="/Users/username/Downloads/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk"

# Or move Xcode to standard location (recommended)
sudo mv "/Users/username/Downloads/Xcode.app" "/Applications/"
```

#### Development vs Production Commands

```bash
# Production (globally installed shimmy)
shimmy serve                     # Auto-allocates port
shimmy generate ./models/your-model.gguf --prompt "Hello" --max-tokens 50

# Development (from source directory)
cargo run --features llama --bin shimmy -- serve
cargo run --features llama --bin shimmy -- generate ./models/Phi-3-mini-4k-instruct-fp16.gguf --prompt "Hello" --max-tokens 50
```

#### Troubleshooting macOS Issues

**Command Not Found Error**:
```bash
# Check if shimmy is installed globally
which shimmy
shimmy --version

# If not found, ensure ~/.cargo/bin is in PATH
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Version Mismatch Issues**:
```bash
# Check versions
shimmy --version                 # Global version
cargo run --bin shimmy -- --version  # Local development version

# Update global installation
cargo install shimmy --features llama --force
```

**Build Failures**:
```bash
# Clean and rebuild
cargo clean
export SDKROOT="/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk"
cargo build --features llama
```

#### Metal GPU Acceleration

**✅ Verified Features:**
- Intel and Apple Silicon Macs (M1, M2, M3, M4)
- Automatic Metal GPU acceleration
- Xcode 17+ compatibility (Version 26.0 build 17A321)
- All LoRA adapter features
- Memory-mapped model loading

**Performance on Apple Silicon:**
- Phi-3-mini-4k (3.8B): ~50-100 tokens/sec
- Automatic GPU memory management
- Efficient memory usage with mmap

#### Model Setup for macOS

```bash
# Download models to local directory
mkdir -p ./models
cd models

# Download via Hugging Face CLI
pip install huggingface_hub
huggingface-cli download microsoft/Phi-3-mini-4k-instruct-gguf Phi-3-mini-4k-instruct-fp16.gguf --local-dir .

# Or set environment variable
export SHIMMY_BASE_GGUF="/path/to/your/model.gguf"
```

#### Quick Verification

```bash
# 1. Test model loading
shimmy probe ./models/Phi-3-mini-4k-instruct-fp16.gguf

# 2. Test generation
shimmy generate ./models/Phi-3-mini-4k-instruct-fp16.gguf --prompt "Write a haiku about macOS" --max-tokens 50

# 3. Start server
shimmy serve --bind 127.0.0.1:11435

# 4. Test API endpoint
curl -X POST http://localhost:11435/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model": "default", "prompt": "Hello from macOS!", "max_tokens": 30}'
```

## Integration Examples

### VSCode Copilot
```json
{
  "github.copilot.advanced": {
    "serverUrl": "http://localhost:11435"
  }
}
```

### Continue.dev
```json
{
  "models": [{
    "title": "Local Shimmy",
    "provider": "openai", 
    "model": "your-model-name",
    "apiBase": "http://localhost:11435/v1"
  }]
}
```

### Cursor IDE
Works out of the box - just point to `http://localhost:11435/v1`

## 🔄 Recent Updates (v1.3.0)

### ✅ **macOS Compatibility Enhancements**
- **Full Xcode 17+ support** (Version 26.0 build 17A321) with detailed troubleshooting
- **Enhanced SDK path handling** for non-standard Xcode installations
- **Complete installation guide** with both production and development workflows
- **Metal GPU acceleration** verified on M1-M4 Apple Silicon chips

### 🚀 **CLI Improvements** 
- **Direct GGUF file support**: CLI commands now accept both model names AND direct `.gguf` file paths
  ```bash
  # Works with registered models
  shimmy generate phi3-lora --prompt "Hello" 
  
  # NEW: Works with direct file paths
  shimmy generate ./models/Phi-3-mini-4k-instruct-fp16.gguf --prompt "Hello"
  ```
- **Shell escaping fixes**: Resolved "Hello!" prompt bug caused by bash history expansion
- **Comprehensive help text** with special character handling guidance

### 🔧 **Developer Experience**
- **Enhanced server welcome page** with interactive model browser and API documentation
- **Improved error messages** with clear troubleshooting steps for common issues
- **Version management clarity** between development vs production installations
- **Comprehensive test coverage** including special character handling in prompts

### 📖 **Documentation Updates**
- **Expanded macOS section** with step-by-step installation and troubleshooting
- **Shell escaping best practices** with examples for special characters
- **Updated examples** showing both CLI usage patterns
- **Performance benchmarks** and verification commands

### 🔍 **Testing & Quality**
- **Property-based testing** for CLI argument parsing with special characters
- **Integration test improvements** with better error handling
- **Workflow test enhancements** with comprehensive edge case coverage
- **Cache performance testing** infrastructure

### 🛠 **Infrastructure**
- **Port management improvements** for development workflows  
- **Enhanced metrics endpoint** with detailed system information
- **Better WebSocket handling** and streaming capabilities
- **Improved build system** with feature flag management

These updates focus on making Shimmy more reliable and user-friendly, especially for macOS developers, while maintaining the core promise of being a fast, lightweight alternative to larger AI inference solutions.

## Why Shimmy Will Always Be Free

I built Shimmy because I was tired of 680MB binaries to run a 4GB model.

**This is my commitment**: Shimmy stays MIT licensed, forever. If you want to support development, [sponsor it](https://github.com/sponsors/Michael-A-Kuykendall). If you don't, just build something cool with it.

> Shimmy saves you time and money. If it's useful, consider sponsoring for $5/month — less than your Netflix subscription, infinitely more useful.

## Performance Comparison

| Tool | Binary Size | Startup Time | Memory Usage | OpenAI API |
|------|-------------|--------------|--------------|------------|
| **Shimmy** | **5.1MB** | **<100ms** | **50MB** | **100%** |
| Ollama | 680MB | 5-10s | 200MB+ | Partial |
| llama.cpp | 89MB | 1-2s | 100MB | None |

## API Reference

### Endpoints
- `GET /health` - Health check
- `POST /v1/chat/completions` - OpenAI-compatible chat
- `GET /v1/models` - List available models
- `POST /api/generate` - Shimmy native API
- `GET /ws/generate` - WebSocket streaming

### CLI Commands
```bash
# Production (installed version)
shimmy serve                    # Start server (auto port allocation)
shimmy serve --bind 127.0.0.1:8080  # Manual port binding
shimmy list                     # Show available models  
shimmy discover                 # Refresh model discovery
shimmy generate ./models/your-model.gguf --prompt "Hi" --max-tokens 50  # Test generation
shimmy probe model-name         # Verify model loads

# Development (from source)
cargo run --features llama --bin shimmy -- serve  # Start development server
cargo run --features llama --bin shimmy -- generate ./models/Phi-3-mini-4k-instruct-fp16.gguf --prompt "Hi" --max-tokens 50
```

> **💡 Shell Tip**: When using prompts with special characters (like `!`), wrap them in single quotes:  
> `shimmy generate model-name --prompt 'Hello!' --max-tokens 50`
> 
> **Common Shell Escaping Issues:**
> - `"Hello!"` ❌ (bash history expansion)
> - `'Hello!'` ✅ (single quotes prevent expansion)
> - `"Hello\!"` ✅ (escaped exclamation)
> - `"Hello"'!'` ✅ (mixed quoting)

> **💡 Shell Tip**: When using prompts with special characters (like `!`), wrap them in single quotes:  
> `shimmy generate model-name --prompt 'Hello!' --max-tokens 50`

## Technical Architecture

- **Rust + Tokio**: Memory-safe, async performance
- **llama.cpp backend**: Industry-standard GGUF inference
- **OpenAI API compatibility**: Drop-in replacement
- **Dynamic port management**: Zero conflicts, auto-allocation
- **Zero-config auto-discovery**: Just works™

## Community & Support

- **🐛 Bug Reports**: [GitHub Issues](https://github.com/Michael-A-Kuykendall/shimmy/issues)
- **💬 Discussions**: [GitHub Discussions](https://github.com/Michael-A-Kuykendall/shimmy/discussions)
- **📖 Documentation**: [docs/](docs/)
- **💝 Sponsorship**: [GitHub Sponsors](https://github.com/sponsors/Michael-A-Kuykendall)

### Sponsors

See our amazing [sponsors](SPONSORS.md) who make Shimmy possible! 🙏

**Sponsorship Tiers:**
- **$5/month**: Coffee tier - My eternal gratitude + sponsor badge
- **$25/month**: Bug prioritizer - Priority support + name in SPONSORS.md  
- **$100/month**: Corporate backer - Logo on README + monthly office hours
- **$500/month**: Infrastructure partner - Direct support + roadmap input

**Companies**: Need invoicing? Email [michaelallenkuykendall@gmail.com](mailto:michaelallenkuykendall@gmail.com)

## Quality & Reliability

Shimmy maintains high code quality through comprehensive testing:

- **Comprehensive test suite** with property-based testing
- **Automated CI/CD pipeline** with quality gates
- **Runtime invariant checking** for critical operations
- **Cross-platform compatibility testing**

See our [testing approach](docs/ppt-invariant-testing.md) for technical details.

---

## License & Philosophy

MIT License - forever and always.

**Philosophy**: Infrastructure should be invisible. Shimmy is infrastructure.

**Testing Philosophy**: Reliability through comprehensive validation and property-based testing.

---

**Forever maintainer**: Michael A. Kuykendall  
**Promise**: This will never become a paid product  
**Mission**: Making local AI development frictionless

*"The best code is code you don't have to think about."*  
*"The best tests are properties you can't break."*