---
name: env-checker
description: Detect and verify local development environment configurations including Python, pip, pipx, Git proxy, system proxy (Clash), and other CLI tools. Provides structured diagnostics and configuration suggestions.
license: MIT
compatibility: opencode
metadata:
  category: system-admin
  platform: cross-platform
  extensible: true
---

## What I do

I perform comprehensive environment configuration checks on the local machine and report findings in a structured format.

### Built-in Checks

1. **Python Ecosystem**
   - `python` / `python3` installation and version
   - `pip` installation, version, and configured index URL (domestic mirror detection)
   - `pipx` installation and version
   - `virtualenv` / `venv` availability

2. **Version Control**
   - `git` installation and version
   - Git HTTP proxy configuration (`http.proxy`, `https.proxy`)
   - Git SSH configuration

3. **System Proxy**
   - `HTTP_PROXY` / `HTTPS_PROXY` environment variables
   - Common proxy ports detection (7890, 7897, 1080, 1087, 10808 for Clash/V2Ray/Shadowsocks)
   - Windows proxy settings (if applicable)

4. **Node.js Ecosystem** (if requested)
   - `node` / `nodejs` installation and version
   - `npm` / `pnpm` / `yarn` installation and version
   - `bun` installation and version
   - npm registry configuration (domestic mirror detection)

5. **Other Common Tools** (if requested)
   - `docker` / `podman`
   - `gh` (GitHub CLI)
   - `brew` (macOS/Linux)
   - `winget` / `scoop` (Windows)
   - `cargo` (Rust)
   - `go` (Golang)

## When to use me

Invoke me when:
- Setting up a new development machine
- Debugging environment-related issues (proxy, package installation failures)
- Verifying if required tools are installed before starting a project
- Checking if domestic mirror proxies are properly configured
- Before asking "why can't I install X?" — let me check first

## How I work

### Running Checks

When asked to check the environment, I will:

1. Run detection commands via `bash` tool
2. Parse configuration files via `read` tool when needed
3. Present results in a structured table format:

   | Tool | Status | Version | Config | Notes |
   |------|--------|---------|--------|-------|
   | python | ✅/❌ | 3.11.2 | - | Found at /usr/bin/python3 |
   | pip | ✅ | 23.0 | Tsinghua mirror | index-url configured |
   | git proxy | ✅ | - | http.proxy=127.0.0.1:7890 | Clash detected on port 7890 |

### Detection Methods

- **Installation check**: `which <tool>` or `where <tool>` (Windows), or `<tool> --version`
- **Version extraction**: Parse `--version` / `-V` output
- **Pip config**: `pip config get global.index-url` or read `~/.pip/pip.ini` / `%APPDATA%\pip\pip.ini`
- **Git proxy**: `git config --global http.proxy`, `git config --global https.proxy`
- **System proxy**: Check env vars `HTTP_PROXY`, `HTTPS_PROXY`, `ALL_PROXY`, `http_proxy`, `https_proxy`
- **Proxy process detection**: `lsof -i :7890` (macOS/Linux), `netstat -ano | findstr :7890` (Windows), or `Get-Process -Name *clash*` (PowerShell)

### Extending Checks

To add a new tool check (e.g., `bun`):

1. Run: `bun --version` to detect installation
2. Check config: `bun config get registry` or relevant config file
3. Add a row to the results table

I can be instructed to check any specific tool by name, and I will attempt to detect it using standard CLI patterns.

## Output Format

```
🔍 Environment Check Report
==========================

✅ Python Ecosystem
   • Python 3.11.2 (/usr/bin/python3)
   • pip 23.0 (Tsinghua mirror: https://pypi.tuna.tsinghua.edu.cn/simple)
   • pipx 1.2.0

✅ Git Configuration
   • Git 2.40.0
   • HTTP proxy: 127.0.0.1:7890
   • HTTPS proxy: 127.0.0.1:7890

⚠️  System Proxy
   • Env vars: HTTP_PROXY=http://127.0.0.1:7890
   • Clash detected on port 7890 (process: clash-verge)
   • Note: Ensure git proxy matches system proxy if using CLI behind GFW

❌ Node.js Ecosystem (not installed or not in PATH)
   • node: not found
   • npm: not found
   • bun: not found
```

## Proxy Troubleshooting Guide

If the user is in mainland China and experiencing slow package downloads:

1. **Pip domestic mirrors** (if not configured):
   ```bash
   # Tsinghua University
   pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

   # Alibaba Cloud
   pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
   ```

2. **Git proxy for Clash** (if Clash is running):
   ```bash
   # HTTP protocol repos
   git config --global http.proxy http://127.0.0.1:7890
   git config --global https.proxy http://127.0.0.1:7890

   # For SSH protocol, use ProxyCommand instead
   ```

3. **npm registry** (if Node.js is installed):
   ```bash
   npm config set registry https://registry.npmmirror.com
   ```

4. **Verify proxy is working**:
   ```bash
   curl -v https://www.google.com  # Should show proxy connection
   ```

## Important Notes

- Always check both `python` and `python3` on Unix-like systems
- On Windows, use `where` instead of `which`, and check `%APPDATA%` / `%USERPROFILE%` for config paths
- If a tool is installed but not in PATH, note the installation location if found
- When checking proxies, distinguish between:
  - System-level proxy (env vars)
  - Application-level proxy (git config, pip config, npm config)
  - Running proxy processes (Clash, v2rayN, etc.)
- Be careful not to expose sensitive proxy credentials in output
- If Clash/V2Ray is found but not configured for a tool, suggest the appropriate configuration
