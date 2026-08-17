# Fix Template Test Sync Issue - Issue #10

## Problem
The test in `test/init.test.js` hardcodes a `vars` object (lines 92-102) instead of using the one `writeSchedulerFiles()` builds. This creates a gap: template changes can pass the test but break real code.

## Solution
1. Extract the `vars` object into a new `schedulerVars(job)` function in `src/schedulers.js`
2. Have `writeSchedulerFiles()` call `schedulerVars()` instead of building inline
3. Update the test to use `schedulerVars()` instead of the hardcoded map

## Files to Touch
- `src/schedulers.js`: Add `schedulerVars()` function, update `writeSchedulerFiles()`
- `test/init.test.js`: Replace hardcoded vars with `schedulerVars()`

## Test Command
```bash
npm test
```

## Acceptance Criteria
- [ ] `schedulerVars(job)` exported, `writeSchedulerFiles()` is its only caller
- [ ] Test uses `schedulerVars()` instead of hardcoded map
- [ ] Repro from issue #10 now makes `npm test` **fail** (step 3, no step 2 edit)
- [ ] `npm test` green without mutations, existing assertions still pass

## Scope
Touch only `src/schedulers.js` and `test/init.test.js`. Don't call `writeSchedulerFiles()` in tests.