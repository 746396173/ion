# Ion Kotlin Extensions - async/await

async/await allows you to write code that looks like synchronous code, but is actually run asynchronously via coroutines.

For example, if you wanted to download a list of files with async/await in Ion:

```kotlin
fun getFiles(files: Array<String>) = async {
    for (file in files) {
        Ion.with(context)
        .load(file)
        .asString()
        .await()
    }
}
```
This may look like synchronous code, but it is not. The return type of getFiles is a actually [Future](https://github.com/koush/ion#futures). The operation happens asynchronously, and only when all the files are finished downloading, will the Future's callback be called.

The code in an async block is a [suspend fun, aka a coroutine](https://kotlinlang.org/docs/reference/coroutines.html).
```kotlin
async {
    // the code in here is a suspend fun, a coroutine.
    // execution can be suspended and resumed.
}
```

Inside an async block, you can use await on various objects to wait for results.

```kotlin
fun getFiles(files: File) = async {
    System.out.println("Bob")

    Ion.with(context)
    .load(file)
    .asString()
    .await()
    
    System.out.println("Chuck")
}

System.out.println("Alice")
getFiles(file)
System.out.println("David")
```

async blocks are run asynchronously, but look synchronous. The output from this code would be:

```
Alice
Bob
David
Chuck
```

Execution was paused at the await() call, and resumed after the file was downloaded.