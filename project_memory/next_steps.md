# Next Steps

## Bug Fixes (Priority)
1. Fix `--progressive` mode
   - Issue: Not accurately determining breaking point or recommended capacity
   - Likely cause: Test environment restrictions or threshold logic

2. Fix `--cache-headers` breaking `--burst`
   - Issue: Flag interaction causing burst functionality to fail
   - Need to investigate flag parsing/processing order

3. Fix `--concurrency-mode=async`
   - Issue: Not handling concurrent requests properly
   - Breaks burst flag functionality
   - Check curl_multi_exec implementation

## Investigation Needed
- Review how flags are processed and potential conflicts
- Examine progressive testing logic and thresholds
- Check async concurrency implementation with curl_multi

## Potential Improvements
- Enhanced error handling and reporting
- Better flag validation to prevent conflicts
- More robust progressive testing algorithm