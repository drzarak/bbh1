# Testing the Link Checker

This repository uses [Lychee](https://lychee.rs/) to check for broken links in markdown files. This ensures that all documentation links remain valid and accessible.

## What is Tested

The link checker validates:
- **Local file links** (e.g., links to other markdown files, directories)
- **External URLs** (e.g., https://github.com, https://example.com)
- **Relative paths** within the repository
- **Email addresses** (when `--include-mail` is used)

## How to Test Locally

### Quick Test
Run the automated test script:
```bash
./test-link-checker.sh
```

### Manual Testing

1. **Test a single file:**
   ```bash
   ./lychee --config lychee.toml README.md
   ```

2. **Test all markdown files:**
   ```bash
   ./lychee --config lychee.toml --no-progress './**/*.md'
   ```

3. **Test with verbose output:**
   ```bash
   ./lychee --config lychee.toml --verbose README.md
   ```

4. **Test without network requests (local only):**
   ```bash
   ./lychee --config lychee.toml --offline './**/*.md'
   ```

## Configuration

The link checker is configured via `lychee.toml`:

```toml
exclude = [
  "https://readme-typing-svg.herokuapp.com"
]
```

This configuration excludes certain URLs from being checked (e.g., dynamic SVG generators that may not respond reliably).

## Continuous Integration

Links are automatically checked on every pull request via GitHub Actions (`.github/workflows/link-check.yml`).

## Expected Results

- ✅ **Local links** should return `[200]` status
- 🚫 **External links** may show network errors in sandboxed environments (this is expected)
- 👻 **Excluded links** will show as `[EXCLUDED]`

## Troubleshooting

### Common Issues

1. **Network errors:** These are expected in restricted environments and don't indicate a problem with the link checker itself.

2. **File not found:** Check that the file path is correct and the file exists.

3. **Permission denied:** Ensure the lychee binary has execute permissions:
   ```bash
   chmod +x lychee
   ```

### Adding Exclusions

To exclude problematic URLs, add them to `lychee.toml`:

```toml
exclude = [
  "https://problematic-url.com",
  "https://another-url.com"
]
```