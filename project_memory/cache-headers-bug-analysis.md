# Cache Headers Bug Analysis

## Problem Description
When `--cache-headers` flag is set, the `--burst` functionality breaks:
- Either runs only 1 burst then stops
- Or runs with bursts of 1 until completed

## Root Cause Analysis

### Current Flow
1. Request generator accumulates cache headers in `$this->cache_headers` array
2. After each burst, we call `$request_generator->get_cache_headers()`
3. This returns ALL accumulated headers since the beginning, not just from the current burst
4. The counts keep growing across bursts

### Suspected Issue Location
**File**: `microchaos/core/commands.php`
**Lines**: 649-656

```php
// Process cache headers
if ($collect_cache_headers) {
    $cache_headers = $request_generator->get_cache_headers();
    foreach ($cache_headers as $header => $values) {
        foreach ($values as $value => $count) {
            $cache_analyzer->collect_headers([$header => $value]);
        }
    }
}
```

### The Problem
The `$count` variable in the nested foreach loop is the accumulated count from ALL previous bursts, not just the current burst. This might be:
1. Confusing the burst logic
2. Causing variable name collision with the main `$count` parameter
3. Processing takes longer as data accumulates

## Potential Solutions

### Solution 1: Reset cache headers after each burst
Add a method to reset cache headers in the request generator after processing:
```php
public function reset_cache_headers() {
    $this->cache_headers = [];
}
```

Then call it after processing in commands.php:
```php
if ($collect_cache_headers) {
    $cache_headers = $request_generator->get_cache_headers();
    // ... process headers ...
    $request_generator->reset_cache_headers();
}
```

### Solution 2: Get and clear in one operation
Add a method that returns headers and clears them:
```php
public function get_and_clear_cache_headers() {
    $headers = $this->cache_headers;
    $this->cache_headers = [];
    return $headers;
}
```

### Solution 3: Track per-burst headers
Modify the request generator to track headers per burst and return only the latest burst's headers.

## Implemented Solution

### Fix Applied (Two-part fix):

1. **Variable Name Collision Fix**
   - Changed `$count` to `$header_count` in cache header processing loops (line 652, 962)
   - Changed `$count` to `$url_count` in async URL group loops (line 622, 947)
   - This prevents shadowing the main `$count` parameter

2. **Cache Header Accumulation Fix**
   - Added `reset_cache_headers()` method to MicroChaos_Request_Generator
   - Called after processing headers in each burst
   - Prevents headers from accumulating across bursts

### Files Modified:
- `microchaos/core/commands.php`: Fixed variable names, added reset call
- `microchaos/core/request-generator.php`: Added reset_cache_headers() method

## Testing Plan
1. Test with `--cache-headers` and various `--burst` values
2. Verify burst count is respected
3. Verify cache header reporting is still accurate
4. Test with both count-based and duration-based tests