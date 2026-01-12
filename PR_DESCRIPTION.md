## Summary

Ensure `docc preview` exits cleanly after handling termination signals.

Previously, when the preview server received SIGTERM/SIGINT, it would stop the server but the signal handler didn't explicitly exit the process. This could leave the process in an undefined state depending on where execution was when the signal was delivered.

Adding `exit(0)` after successfully stopping servers ensures the process terminates cleanly.

## Changes

- Add `exit(0)` after stopping preview servers in the signal handler

## Test plan

1. Run `docc preview` on a documentation bundle
2. Press Ctrl+C to send SIGINT
3. Verify the process exits with status 0 and doesn't leave orphaned threads

## Related

This is part of a broader fix for orphaned preview processes. The companion PR in swift-docc-plugin adds monitoring to detect when parent processes die unexpectedly.
