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

## Platform Understanding
- **Pressable limitations documented**: Not bugs, but platform security measures
- **Burst flag clarification**: Controls serial requests, not concurrent (works perfectly)
- **Typical usage pattern**: Duration-based testing with high bursts (50-500+) for capacity planning
- **Key metrics**: Requests/second + resource trends + Grafana monitoring data