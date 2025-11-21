# Build Targets Configuration

This document explains how to manage build targets in the GitHub Actions release workflow.

## Current Build Targets

The release workflow (`.github/workflows/release.yml`) currently builds for the following platforms:

| Platform | Architecture | Target | Status |
|----------|-------------|--------|--------|
| Linux | x86_64 | `x86_64-unknown-linux-musl` | ✅ Enabled |
| Linux | ARM64 | `aarch64-unknown-linux-musl` | ✅ Enabled |
| macOS | x86_64 | `x86_64-apple-darwin` | ✅ Enabled |
| macOS | ARM64 (M1/M2) | `aarch64-apple-darwin` | ✅ Enabled |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | ✅ Enabled |
| Windows | x86_32 | `i686-pc-windows-msvc` | ❌ Disabled |
| Windows | ARM64 | `aarch64-pc-windows-msvc` | ❌ Disabled |

## How to Disable Build Targets

To disable a build target, simply comment out the corresponding lines in the matrix section of `.github/workflows/release.yml`.

### Example 1: Disable Linux ARM builds

```yaml
# Comment out these lines:
# - build: linux arm64
#   os: ubuntu-latest
#   target: aarch64-unknown-linux-musl
```

### Example 2: Disable all macOS builds

```yaml
# Comment out these lines:
# - build: macos x86
#   os: macos-latest
#   target: x86_64-apple-darwin
# - build: macos arm64
#   os: macos-latest
#   target: aarch64-apple-darwin
```

### Example 3: Disable Windows builds

```yaml
# Comment out these lines:
# - build: win x86
#   os: windows-latest
#   target: x86_64-pc-windows-msvc
```

## Why is Windows 32-bit (i686) Disabled?

The 32-bit Windows build (`i686-pc-windows-msvc`) has been disabled due to compatibility issues with some dependencies, particularly image processing libraries that may not fully support 32-bit Windows architectures.

To re-enable it, uncomment the following lines in `.github/workflows/release.yml`:

```yaml
- build: win x86_32
  os: windows-latest
  target: i686-pc-windows-msvc
```

## Updating Release Notes

When you disable a build target, remember to also update the release body table in the workflow file (around line 114) to remove the corresponding entry. This ensures users don't see references to builds that are no longer created.

Example:
```yaml
body: "|Arch|Filename|
|:--: |:--:|
|MacOS ARM| caesiumclt-v*-aarch64-apple-darwin.tar.gz|
|MacOS x86_64| caesiumclt-v*-x86_64-apple-darwin.tar.gz|
|Linux ARM| caesiumclt-v*-aarch64-unknown-linux-musl.tar.gz|
|Linux x86_64| caesiumclt-v*-x86_64-unknown-linux-musl.tar.gz|
|Windows x86_64| caesiumclt-v*-x86_64-pc-windows-msvc.zip|
"
```

## Adding New Build Targets

To add a new build target, add a new entry to the matrix:

```yaml
- build: your-build-name
  os: your-os  # ubuntu-latest, macos-latest, or windows-latest
  target: your-rust-target  # e.g., x86_64-unknown-linux-gnu
```

Make sure to also add the corresponding entry to the release body table.
