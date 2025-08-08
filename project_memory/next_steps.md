# Next Steps

## Bug Fixes (Priority)
1. Fix `--cache-headers` breaking `--burst`
   - Issue: Flag interaction causing burst functionality to fail
   - Need to investigate flag parsing/processing order

## Platform Limitations (Not Bugs)  
- `--progressive` mode: Limited by Pressable's ~10 request rate limit on loopback
- `--concurrency-mode=async`: Cannot bypass platform rate limits  
- These are documented in README as platform limitations, not bugs
- **Burst flag works as intended**: Controls serial requests before pause (not concurrent)

## Completed
- ✅ Fixed PHP 8.1+ deprecation warning (float to int conversion)
- ✅ Documented Pressable platform limitations in README
- ✅ Corrected understanding of burst flag usage patterns

## Investigation Needed
- Review how `--cache-headers` flag affects burst processing
- Check for flag parsing conflicts

## Potential Improvements
- Enhanced error handling and reporting
- Better flag validation to prevent conflicts
- Platform detection to warn users about limitations