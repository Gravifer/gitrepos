# Gitrepos - Curated Development Tools Collection

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Repository Overview

This is a curated collection repository containing 35+ git submodules organized into useful categories. It serves as a centralized hub for essential development tools, libraries, and projects rather than a single buildable application. Each submodule is an independent project with its own build system and requirements.

## Working Effectively

### Essential Commands - Always Use These First

Initialize the repository and specific submodules:
```bash
# Initialize all submodules (required before working with any)
git submodule init

# Update specific submodules only (recommended approach)
git submodule update --init [path/to/submodule]

# NEVER run full recursive update unless absolutely necessary - takes hours
# git submodule update --init --recursive  # AVOID - extremely time-consuming
```

### Core Directory Structure

- **Libraries/**: Core libraries (gcc, openssl, libgit2, libsodium, libzmq, mxe, three.js)
- **Tools/**: Development tools (FFmpeg, bash-wakatime, gh-cli, listen1, neovim, tmux, vcpkg, vim-plug)  
- **Wolfram/**: Wolfram/Mathematica-related projects and tools
- **Templates/**: Project templates
- **Tunneling/**: VPN/tunneling tools (Qv2ray, v2rayN)
- **powerline-fonts**, **powerline-shell**: Terminal customization
- **WSL-Hello-sudo**: Authentication tools for WSL

### Timing Expectations - NEVER CANCEL OPERATIONS

Critical build and update timings (always set appropriate timeouts):

- **Individual submodule updates**: 1-30 seconds for small tools, 30-60 seconds for libraries  
- **Go builds (gh-cli)**: ~45 seconds. NEVER CANCEL. Set timeout to 120+ seconds.
- **Python builds (powerline-shell)**: ~1 second. Set timeout to 30+ seconds.
- **C/C++ library builds (libsodium)**: ~40 seconds. NEVER CANCEL. Set timeout to 120+ seconds.
- **Large submodules (gcc, FFmpeg)**: Can take hours. NEVER CANCEL. Set timeout to 4+ hours.
- **Full repository initialization**: Multiple hours. AVOID unless specifically needed.

## Validated Build Workflows

### Successfully Tested Tools

#### GitHub CLI (Go-based)
```bash
# Update and build - validated working
git submodule update --init Tools/gh-cli
cd Tools/gh-cli
make bin/gh  # Takes ~45 seconds, produces working binary
./bin/gh --version  # Verify build success
```

#### Powerline Shell (Python-based)  
```bash
# Update and build - validated working
git submodule update --init powerline-shell
cd powerline-shell
python3 setup.py build  # Takes ~1 second
python3 setup.py install --user  # Install for current user
```

#### LibSodium (C library)
```bash
# Update and build - validated working
git submodule update --init Libraries/libsodium
cd Libraries/libsodium
./autogen.sh -s && ./configure && make -j4  # Takes ~40 seconds
make check  # Run tests (optional)
```

#### Powerline Fonts
```bash
# Update and install - validated working
git submodule update --init powerline-fonts
cd powerline-fonts
./install.sh  # Installs fonts to ~/.local/share/fonts
```

#### WSL Hello Sudo (Rust-based)
```bash
# Update - build requires Windows toolchain
git submodule update --init WSL-Hello-sudo
cd WSL-Hello-sudo
# Note: Full build requires both Linux and Windows cargo
# See Makefile for cross-compilation requirements
```

### Common Dependencies

Ensure these are available before building:
- **Go**: Version 1.19+ (for Tools/gh-cli and other Go projects)
- **Python 3**: With setuptools (for Python-based tools)
- **GCC/Build tools**: `build-essential` package (for C/C++ libraries)
- **Autotools**: `autoconf`, `automake`, `libtool` (for autotools-based projects)
- **Rust**: `cargo` (for Rust-based tools like WSL-Hello-sudo)

Install on Ubuntu/Debian:
```bash
sudo apt-get update
sudo apt-get install build-essential autoconf automake libtool python3-setuptools golang-go
# Rust: curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Validation Scenarios

Always test functionality after building tools:

### GitHub CLI Validation
```bash
cd Tools/gh-cli
./bin/gh --version  # Should show version info
./bin/gh --help     # Should show help menu
```

### Powerline Shell Validation  
```bash
cd powerline-shell
python3 -c "import powerline_shell; print('Import successful')"
```

### LibSodium Validation
```bash
cd Libraries/libsodium  
make check  # Run test suite - should pass all tests
```

## Submodule Management Best Practices

### Working with Specific Categories

Only initialize submodules you actually need:
```bash
# For development tools
git submodule update --init Tools/neovim Tools/tmux Tools/gh-cli

# For cryptographic libraries
git submodule update --init Libraries/libsodium Libraries/openssl

# For Wolfram development
git submodule update --init Wolfram/WolframClientForPython Wolfram/lsp-wl
```

### Status and Maintenance
```bash
# Check submodule status
git submodule status

# Update specific submodule to latest
cd [submodule-path]
git checkout main  # or master, stable
git pull
cd ../..
git add [submodule-path]
git commit -m "Update [submodule-name] to latest"
```

### Large Submodule Warnings

These submodules are extremely large and take significant time:
- **Libraries/gcc**: Multi-gigabyte, takes hours to clone
- **Tools/FFmpeg**: Large multimedia codebase  
- **Libraries/mxe**: Cross-compilation environment, very large
- **Tools/vcpkg**: Microsoft package manager, large

Only initialize these if specifically needed and allow ample time.

## Troubleshooting

### Common Issues and Solutions

**Submodule not found/empty**: Run `git submodule init` then `git submodule update --init [path]`

**Build fails with missing dependencies**: Install the required development packages listed above

**Long build times**: This is normal - many tools compile from source. Wait for completion.

**Permission errors during install**: Use `--user` flag for Python installs or proper sudo for system installs

**Git submodule update hangs**: Allow more time - some repositories are large and take time to clone

### Network-Dependent Operations

Some submodules may fail to clone in restricted network environments:
- GCC uses `git://` protocol which may be blocked
- Some repositories are large and may timeout on slow connections
- Use `git config --global url."https://".insteadOf git://` if git:// is blocked

## Repository Characteristics

- **Not a single project**: Each submodule is independent
- **No top-level build**: Build individual tools as needed  
- **Organizational structure**: Serves as curated bookmarks
- **Mixed technologies**: Go, Python, C/C++, Rust, JavaScript, Wolfram Language
- **Selective usage**: Initialize only what you need

Remember: This repository is a collection tool - work with individual submodules according to their specific requirements and documentation.