# Cache Headers Enhancement Plan

## Goals
1. Show cache headers alongside each request result (status code, time)
2. Improve summary to show all variations with accurate percentages
3. Remove inaccurate hit/miss ratio assumptions
4. Document that this is Pressable-specific

## Current Issues
- Cache summary assumes HIT/MISS ratios that don't account for all variations (STALE, UPDATING, etc.)
- Per-request output doesn't show cache information
- Not clearly documented as Pressable-specific

## Implementation Plan

### 1. Per-Request Cache Header Display
**File**: `request-generator.php`
**Changes**:
- Modify the display logic to include cache headers when `collect_cache_headers` is enabled
- Format: `-> 200 in 0.032s [EDGE_STALE] [BATCACHE_MISS] x-ac:3.dca_updating`
- Store cache info in result array for summary processing

### 2. Enhanced Cache Summary
**File**: `cache-analyzer.php` 
**Changes**:
- Remove hit/miss ratio assumptions
- Show percentage breakdown of ALL cache header variations
- Focus on x-ac (Edge Cache) and x-nananana (Batcache) for Pressable

### 3. Documentation Updates
**File**: `README.md`
**Changes**:
- Document that `--cache-headers` is Pressable-specific
- Explain what x-ac and x-nananana headers represent
- Add examples of cache header output

## Expected Output Format

### Per-Request
```
-> 200 in 0.032s [x-ac: 3.dca_atomic_dca STALE] [x-nananana: MISS]
-> 200 in 0.024s [x-ac: 3.dca_atomic_dca UPDATING] [x-nananana: HIT]
```

### Summary
```
📦 Pressable Cache Header Summary:
   🌐 Edge Cache (x-ac):
     3.dca_atomic_dca STALE: 45 (45.0%)
     3.dca_atomic_dca UPDATING: 35 (35.0%)
     3.dca_atomic_dca HIT: 20 (20.0%)
     
   🦇 Batcache (x-nananana):
     MISS: 60 (60.0%)
     HIT: 40 (40.0%)
     
   ⏲ Average Cache Age: 42.5 seconds (when present)
```

## Implementation Order
1. Modify request generator to capture and display cache info per request
2. Update cache analyzer to show percentage breakdown instead of ratios
3. Update README documentation
4. Test with various Pressable scenarios