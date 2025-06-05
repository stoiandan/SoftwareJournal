This example is inspired from [a Microsoft post](https://devblogs.microsoft.com/dotnet/understanding-the-whys-whats-and-whens-of-valuetask/).

Imagine a simple `Task` returning method:


```C#
public async Task WriteAsync(byte value)
{
    if (_bufferedCount == _buffer.Length)
    {
        await FlushAsync();
    }
    _buffer[_bufferedCount++] = value;
}
```
the method definition says it's returning a `Task`, but that's nowhere to be seen. In essence, if the task goes via the most common case — that is, skips the if; since, most commonly when interacting with a buffer, it's only __full__ once, and for the rest of the calls it's __not full__ –  runtime will return for as Task object that is allready completed.

Now, since this case can be common, meaning lots of times — not just for this method, but any other alike methods we might write – we would like to just return a task that is completed, we could create such a pre-deifned task, and re-use that all the time! Aka, cache it – just us a caching techinque.

And dotnet's APIs actually expose that pre-defined task object for us, it's called `Task.CompletedTask` from `System.Threading.Tasks`.

In fact there are a bunch of other, common case `Task`s that dotnet just caches, including a `Task.FromResult(true)` and `Task.FromResult(false)` for when a funciton's return type is `bool` – this is also why I think (and also jugding the acrticle's __jargon__) that it's the run-time, rather than the compiler that needs to do the adjustment.

However, not everything can be cached, if you return a `Task<int>` there'd be millions of values to cache and that just won't work.

This is where `TaskValue<T>` comes in place. As opposed to `Task` it's not a __class__, but a __struct__, this means it's allocated on the stack, rather than the heap – why not have `Task` just be a struct? Because it can be used in more complicated scenarions; like multiple places awaiting the same task – and inside it, it can store one of two values – a __discriminated-union__, like an `enum` –– either a synchronous/direct result, i.e. a `T`, for when it returns directly – and again, since it's allocated on stack, it's efficient – or a `Task<T>` for when you have to await.