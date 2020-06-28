# Coroutines in Android Project

## Difference between callback pattern

Callbacks are a great pattern, however they have a few drawbacks. Code that heavily uses callbacks can become hard to read and harder to reason about. In addition, callbacks don't allow the use of some language features, such as exceptions.

Kotlin coroutines let you convert callback-based code to sequential code. Code written sequentially is typically easier to read, and can even use language features such as exceptions

### Coroutines by another name (Same approach on different languages)

The pattern of `async` and `await` in other languages is based on coroutines. If you're familiar with this pattern, the **suspend** keyword is similar to `async`. However in Kotlin, `await()` is implicit when calling a **suspend** function.

Kotlin has a method Deferred.await() that is used to wait for the result from a coroutine started with the async builder.

## Understanding CoroutineScope

In Kotlin, all coroutines run inside a CoroutineScope. A scope controls the lifetime of coroutines through its job. When you cancel the job of a scope, it cancels all coroutines started in that scope. On Android, you can use a scope to cancel all running coroutines when, for example, the user navigates away from an Activity or Fragment. Scopes also allow you to specify a default dispatcher. A dispatcher controls which thread runs a coroutine.

## Usage of ViewModelScope

It is available in ViewModel to use Coroutines with. We can create or use that -already defined- scope.

1. viewModelScope.launch will start a coroutine in the viewModelScope. This means when the job that we passed to viewModelScope gets canceled, all coroutines in this job/scope will be cancelled. If the user left the Activity before delay returned, this coroutine will automatically be cancelled when onCleared is called upon destruction of the ViewModel.

2. Since viewModelScope has a default dispatcher of Dispatchers.Main, this coroutine will be launched in the main thread. We'll see later how to use different threads.

3. The function delay is a suspend function. This is shown in Android Studio by the  icon in the left gutter. Even though this coroutine runs on the main thread, delay won't block the thread for one second. Instead, the dispatcher will schedule the coroutine to resume in one second at the next statement.

## Coroutines with Room

All we need to do is add `suspend`keyword before the Dao function

When you do this, Room will make your query main-safe and execute it on a background thread automatically. However, it also means that you can only call this query from inside a coroutine.

And – that's all you have to do to use coroutines in Room. Pretty nifty

## Coroutines with Retrofit

To use suspend functions with Retrofit you have to do two things:

In the Service API interface:

1. Add a suspend modifier to the function

2. Remove the Call wrapper from the return type. Here we're returning String, but you could return complex json-backed type as well. If you still wanted to provide access to Retrofit's full Result, you can return `Result<String>` instead of String from the suspend function.

Retrofit will automatically make suspend functions main-safe so you can call them directly from Dispatchers.Main.

> Both Room and Retrofit make suspending functions main-safe.<br>
> It's safe to call these suspend funs from Dispatchers.Main, even though they fetch from the network and write to the database.<br>

> Both Room and Retrofit use a custom dispatcher and do not use Dispatchers.IO.<br>
> Room will run coroutines using the default query and transaction Executor that's configured.<br>
> Retrofit will create a new Call object under the hood, and call enqueue on it to send the request asynchronously.

## Testing Coroutines

The library kotlinx-coroutines-test has the runBlockingTest function that blocks while it calls suspend functions.

With that method we are able to provide correct test results.(With synchronous call of suspend function, test result will be wait till the IO operation like service call or database operation ends)

Code Sample:

```kotlin
@Test
    fun whenRefreshTitleSuccess_insertsRows() = runBlockingTest {
        val titleDao = TitleDaoFake("title")
        val subject = TitleRepository(
            MainNetworkFake("SomeNetworkResult"),
            titleDao
        )
        subject.refreshTitle() // This is suspend function - makes network request and write data to db
        Truth.assertThat(titleDao.nextInsertedOrNull()).isEqualTo("SomeNetworkResult")
    }
```

### Add timeout for operation using **withTimeout**

withTimeout { ... } is designed to cancel the ongoing operation on timeout, which is only possible if the operation in question is cancellable.

By using it we can limit network operation duration.

Sample Code:

```kotlin
withTimeout(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
}
```

It produces the following output

> I'm sleeping 0 ...<br>
> I'm sleeping 1 ...<br>
> I'm sleeping 2 ...<br>
> Exception in thread "main" kotlinx.coroutines.TimeoutCancellationException: Timed out waiting for 1300 ms

**withTimeoutOrNull** function that is similar to **withTimeout** but returns null on timeout instead of throwing an exception

```kotlin
val result = withTimeoutOrNull(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
    "Done" // will get cancelled before it produces this result
}
println("Result is $result")
```

It produces the following output

> I'm sleeping 0 ...<br>
> I'm sleeping 1 ...<br>
> I'm sleeping 2 ...<br>
> Result is null

**REFERENCES**
: [codelabs.developers.google.com/codelabs/kotlin-coroutines](codelabs.developers.google.com/codelabs/kotlin-coroutines)
: [https://kotlinlang.org/docs/reference/coroutines/cancellation-and-timeouts.html](https://kotlinlang.org/docs/reference/coroutines/cancellation-and-timeouts.html)

**< [Go back to Android section](../android)**
