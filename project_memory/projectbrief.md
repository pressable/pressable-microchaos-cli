# MicroChaos CLI Project Brief

## Purpose
Internal WordPress load testing tool via WP-CLI for staging environments where external tools are blocked. Simulates traffic at scale with cache analysis, resource monitoring, and progressive testing capabilities.

## Architecture
- **Modular components** in `microchaos/core/`
- **Build system** compiles to single `dist/microchaos-cli.php`
- **WP-CLI integration** for command-line interface
- **Key components**:
  - commands.php: WP-CLI command handling
  - request-generator.php: HTTP request management
  - cache-analyzer.php: Cache header analysis
  - resource-monitor.php: System resource tracking
  - reporting-engine.php: Results collection
  - parallel-test.php: Parallel testing functionality
  - thresholds.php: Performance thresholds
  - integration-logger.php: External monitoring

## Current Bugs (v2.0.0)
1. **`--progressive` mode broken**: Not accurately determining breaking point/capacity
2. **`--cache-headers` breaks `--burst`**: Flag interaction issue
3. **`--concurrency-mode=async` broken**: Not handling concurrent requests properly, breaks burst

## Testing Capabilities
- Standard load testing with various endpoints
- Parallel testing with JSON test plans
- Progressive testing to find capacity limits
- Resource trend analysis for leak detection
- Cache behavior analysis
- Baseline comparisons
- External monitoring integration

## Build Process
```bash
node build.js  # Compiles modular components to dist/microchaos-cli.php
```

## Code Style
- 4-space indentation
- Classes: PascalCase with underscores (MicroChaos_Commands)
- Methods: snake_case (fire_request)
- Constants: UPPERCASE (MICROCHAOS_VERSION)
- PHPDoc comments for documentation