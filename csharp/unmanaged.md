# Unmamanged Code in C#

`.net` platform will automatically mamange _memeory_ for you. Using a _garbabe collector_ all objects that are no longer referenced will eventually be removed.
The reason this is higly costly is because, in order for the GC (garbage collector) to do its job, it has to _pause_ **all threads** in order to ensure that no thread changes the memory.

Imagine:

<img width="806" alt="image" src="https://github.com/stoiandan/SoftwareJournal/assets/10388612/83304ccd-d9f6-4032-b633-bee255292e46">


Here `Thread 3`, if theoretically would be allowed to run, would have no idea the `GC` is moving memory around (As GC).

Also, becuase of _race conditions_ it might be that case the a _thread_ updates a reference to an object, but the GC is not informed yet, incorectly marking the new ojbect as unreferenced, and thus eligible for garbace collection:

```C#
class Example {
    public object A;
}

Example example = new Example();
example.A = new object();

// GC starts marking phase and marks example.A as reachable
// At this point, the thread changes example.A to point to a new object
example.A = new object();

// If the GC doesn't account for this change, it might not mark the new object as reachable
// and incorrectly collect the new object, leading to crashes when accessed
```


## using statement

`using` statements are a covennient mechanism for dealing with unmanaged code, i.e., classes that implement the `IDisposable` interface. The bigest advange over calling the `Dispose` method yourself, is that even  if there's an exception thrown, the `Dispose` method is guaranteed to be be called:


```C#
    using(UnmamangedClass unmanaged = new())
    {
        ....
        throw new Exception("ups!");
    }
   // unmanaged will corectly disposed of unmanaged
```
