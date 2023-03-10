---
layout: post
title: Invoke Repeating and Invoke should you use it
date: 2022-05-06 20:29
category: [unity]
tags: [unity,programming]
---

There comes a time when you either need to run a block of code after a certain time, or you need to run a block repeatedly at a predetermined time. The question is how would you do this, and Unity has a few ways you can do this, one of which is using `Invoke()` and/or using `InvokeRepeating()` to do the job.

### Using Invoke and InvokeRepeating

When you would like to run a block of code after a certain delay, there is nothing more easier than to use `Invoke`, in the following example the `Execute` method is executed or runs with a 2 seconds delay after the scene is started.

```csharp
public class InvokeTest : MonoBehaviour
{  
    private void Start()
    {
        Invoke("Execute", 2f);
    }

    private void Execute()
    {
        Debug.Log("Method is executed");
    }  
}
```

To run a method that runs periodically, then we can use `InvokeRepeating`, which is simialr to `Invoke`. Only this time, there is a third parameter that determines the time intervals that a block of code will run.

```csharp
public class InvokeTest : MonoBehaviour
{  
    private void Start(){
        InvokeRepeating("Execute", 2f, 1f);
    }

    private void Execute(){
        Debug.Log("Method is executed");
    }  
}
```

As you can see in the above code, the Execute method will run with 2 seconds delay after the scene has loaded and it will run once a second periodically.

Using an `Invoke()` or `InvokeRepeating()` is easier than using a coroutine. Where using a `Coroutine` is more flexible. However you cannot pass a parameter to an invoked method but you can do this to a coroutine.

Another thing which we have to mention is coroutines are more performance-friendly than the `Invoke()`. For basic games, it does not matter much but if you have several objects which do the same thing, you should consider using `Coroutine` instead of `Invoke()`.

The last difference between `Invoke()` and a `Coroutine`, is the execution condition after the deactivation of the object. `Invoke()` and `InvokeRepeating()` do not stop after the game object is deactivated. If you don't take care of them correctly, which is when the `GameObject` is disabled or destroyed, you would need to also do a `CancelInvoke()` to make sure that the `InvokeRepeating()` doesn't keep running.

We can see in the code example below what we would need to do.

```csharp
public class InvokeTest : MonoBehaviour
{  
    private void Execute(){
        Debug.Log("Method is executed");
    }

    private void OnEnable(){
        InvokeRepeating("Execute", 2f, 1f);
    }

    private void OnDisable()
    {
        CancelInvoke();
    }
}
```

Now we could also do this inside an `OnDestroy()`, however if you were to then only disable the `GameObject`, the code block would still continue to run. You may have also noticed that the code also has the `InvokeRepeating()` moved to the `OnEnable()` section, so that if the `GameObject` was re-enabled, the `InvokeRepeating()` would also be set back up again.

### In Conclusion

The reason to use them is up to you and your use case, as long as you are aware of their limitations, then you would be able to identify if they are not working as you intended them to be used.

