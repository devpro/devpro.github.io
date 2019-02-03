# C# language

## Use cases

### Examples

#### Synchronous call to an asynchronous method

```csharp
// if CallServiceAsync returns Task (void)
var task = Task.Run(() => CallServiceAsync(url, action));
task.Wait();
```
