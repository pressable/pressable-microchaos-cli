# Design Decisions

## Architecture
- **Modular design**: Separate components for maintainability
- **Single-file distribution**: Simplified deployment to mu-plugins
- **WP-CLI integration**: Native WordPress tooling

## Implementation Notes
- Using loopback requests for internal testing
- Cache analysis via HTTP headers rather than deep instrumentation
- Resource monitoring through PHP's memory/CPU functions

## Bug Fix Approach

### PHP 8.1+ Deprecation Fix (Line 4848) - COMPLETED
- **Issue**: Implicit float to int conversion deprecated in sleep()
- **Fix**: Explicitly cast $random_delay to int: `sleep((int)$random_delay)`
- **Files**: microchaos/core/commands.php lines 687, 1034
- **Impact**: Prevents deprecation warnings on PHP 8.1+

### Cache Headers Bug Fix - COMPLETED
- **Issue**: Variable name collision (`$count` shadowing main parameter) broke burst logic
- **Root cause**: Cache header processing loops used `$count` which conflicted with request count
- **Fix**: Renamed loop variables to `$header_count` and `$url_count`
- **Additional fix**: Added `reset_cache_headers()` to prevent accumulation across bursts

### Cache Headers Enhancement - COMPLETED
- **Per-request display**: Shows cache headers with each request result
- **Accurate summary**: Percentage breakdown of all cache states (no hit/miss assumptions)
- **Pressable-specific**: Clearly documented as Pressable-only feature
- **Enhanced reporting**: Focus on x-ac (Edge Cache) and x-nananana (Batcache)

## Platform Understanding
- **Pressable limitations documented**: Not bugs, but platform security measures
- **Burst flag clarification**: Controls serial requests, not concurrent (works perfectly)
- **Typical usage pattern**: Duration-based testing with high bursts (50-500+) for capacity planning
- **Key metrics**: Requests/second + resource trends + Grafana monitoring data
- **Cache analysis**: Provides detailed Pressable cache behavior insights