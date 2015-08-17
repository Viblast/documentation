
## Getting Debug Information from Viblast Player

Viblast Player collects information about its operations. This information can greatly help in debugging problems. To get it just call:

```javascript
var debugInfo = Viblast.printDebugInfo();
```

`printDebugInfo()` returns a preformatted string that can be sent to the Viblast Team preferably with additional information describing the issue. It's safe to call the `printDebugInfo()` function in any environment even in environments that don't support Viblast Player.

Viblast Player collects data only about its own operations and `printDebugInfo()` does NOT contain any private information.

