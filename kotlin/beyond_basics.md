# Kotlin Beyond Basics

```kotlin
/**
* Consts
* Consts are compile time constants. T
* Their value has to be assigned during compile time, unlike vals, where it can be done at runtime.
* This means, that consts can never be assigned to a function or any class constructor, but only to a String or primitive.
*/
const val foo = complexFunctionCall()   //Not okay
val fooVal = complexFunctionCall()  //Okay

const val bar = "Hello world"

// We can't declare const in class
class MyClass {
    const val CONSTANT2 = "asd" // This will give error
}
// Instead we need to use compainon object in class or object class
class MyClass3 {
    companion object {
        const val CONSTANT2 = "123"
    }
}
// or
object MyClass2 {
    const val CONSTANT2 = "asd"
}
// Usage
println(MyClass2.CONSTANT2)
println(MyClass3.CONSTANT2)

/**
* EXTENSION FUNCTIONS
* It will allow to add functions to an existing class, without having access to its source code
* They will not actually modify the class they extend
* By defining an extention you do not insert new members into the class
*/

fun String.hasSpaces(): Boolean {
    val found: Char? = this.find { it == ' ' }
    return found != null
}
// Alternative way to represent same function
fun String.hasSpaces(): Boolean = find { it == ' ' } != null

println("My text with space".hasSpaces())

// Note: "extenstions.kt" is good file to keep all extension functions

open class AquariumPlant(var color: String, private val size: Int)
class GreenLeafyPlant(size: Int): AquariumPlant("Green", size)

fun AquariumPlant.isRed() = color.equals("Red", ignoreCase = true)
// Extension functions can't access to their extended class's private variables
fun AquariumPlant.isBig() = size < 20 // It will give error since size is private variable

fun AquariumPlant.print() = println("AquariumPlant")
fun GreenLeafyPlant.print() = println("GreenLeafyPlant")

val plant = GreenLeafyPlant(20)
println(plant.isRed()) // false
plant.print() // Output : GreenLeafyPlant

val aquariumPlant: AquariumPlant = plant
aquariumPlant.print() // Output : AquariumPlant
// Normally it should print GreenLeafyPlant but it prints AquariumPlant
// because extension functions resolved statically and at compile time (not on runtime)

// Extension properties
val AquariumPlant.isGreen: Boolean
    get() = color.equals("Green", ignoreCase = true)

val plant = GreenLeafyPlant(20)
println(plant.isGreen)

// Nullable Example
fun AquariumPlant?.pull() {
    this?.apply {
        println("removing $this")
    }
}
var nullPlant: AquariumPlant? = null
    nullPlant.pull() // it will not throw exception

/**
* Extention Function as parameter
*/
// By HashMap<K, V>.() defining as that we make our build function as extention function for hashmap
fun <K, V> buildMap(build: HashMap<K, V>.() -> Unit): Map<K, V> {
    val map = HashMap<K, V>()
    map.build()
    return map
}

// Another Example
fun <T> T.myApply(f: T.() -> Unit): T {
    this.f()
    return this
}

fun createString(): String {
    return StringBuilder().myApply {
        append("Numbers: ")
        for (i in 1..10) {
            append(i)
        }
    }.toString()
}

fun createMap(): Map<Int, String> {
    return hashMapOf<Int, String>().myApply {
        put(0, "0")
        for (i in 1..10) {
            put(i, "$i")
        }
    }
}

/**
* Reflections
*/
class Plant {
    fun trim() {}
    fun fertilize() {}
}

fun reflections() {
    val classObj: KClass<Plant> = Plant::class
    for (method: KFunction<*> in classObj.declaredMemberFunctions){
        println("${classObj.simpleName} has ${method.name} method")
    }
}

/**
* APPLY, RUN, LET
*/ 

// Apply return the object itself
// Apply can be very useful for calling functions on a newly created object
data class Bird(var name: String)
val bird = Bird("marti").apply { name = "sahin" }

// Difference between run and apply is
// Run returns the result of executing the lambda
// While Apply returns the object after the lambda has been applied
val bird2 = Bird("leylek")
println(bird.run { name.toUpperCase() }) // Leylek
println(bird.apply { name = name.toUpperCase() }) // Bird(name=Leylek)

// Let returns copy of changed object
// Let is particularly useful for chaining manipulations together
val result = bird.let { it.name.capitalize() } // Sahin
        .let { "$it bird" } // Sahin bird
        .let { it.length } // 10 - converts variable to integer
        .let { it.plus(30) } // 40
println(result) // Output: 40


/**
* Operator overloading
* Kotlin allows us to provide implementations for a predefined set of operators on our types. 
* These operators have fixed symbolic representation (like + or *) and fixed precedence.
*/
data class MyDate(val year: Int, val month: Int, val dayOfMonth: Int)

// Supported intervals that might be added to dates:
enum class TimeInterval { DAY, WEEK, YEAR }

fun MyDate.addTimeIntervals(timeInterval: TimeInterval, amount: Int): MyDate {
    val c = Calendar.getInstance()
    c.set(year + if (timeInterval == TimeInterval.YEAR) amount else 0, month, dayOfMonth)
    var timeInMillis = c.timeInMillis
    val millisecondsInADay = 24 * 60 * 60 * 1000L
    timeInMillis += amount * when (timeInterval) {
        TimeInterval.DAY -> millisecondsInADay
        TimeInterval.WEEK -> 7 * millisecondsInADay
        TimeInterval.YEAR -> 0L
    }
    val result = Calendar.getInstance()
    result.timeInMillis = timeInMillis
    return MyDate(result.get(Calendar.YEAR), result.get(Calendar.MONTH), result.get(Calendar.DATE))
}

operator fun MyDate.plus(timeInterval: TimeInterval) =
        addTimeIntervals(timeInterval, 1)

class RepeatedTimeInterval(val timeInterval: TimeInterval, val number: Int)

operator fun TimeInterval.times(number: Int) =
        RepeatedTimeInterval(this, number)

operator fun MyDate.plus(timeIntervals: RepeatedTimeInterval) =
        addTimeIntervals(timeIntervals.timeInterval, timeIntervals.number)

fun task1(today: MyDate): MyDate {
    return today + YEAR + WEEK
}

fun task2(today: MyDate): MyDate {
    return today + YEAR * 2 + WEEK * 3 + DAY * 5
}

/**
* Invoke
* Objects with an invoke() method can be invoked as a function.
* You can add an invoke extension for any class, but it's better not to overuse it
*/ 
class Invokable {
    var numberOfInvocations: Int = 0
        private set // this variable can not be set outside of the class

    operator fun invoke(): Invokable {
        numberOfInvocations++
        return this
    }
}

fun invokeTriple(invokable: Invokable) = invokable()()()

println("Method invoked ${invokeTriple(Invokable()).numberOfInvocations} times") // Output : Method invoked 3 times

/**
* Sequences
* LAZY = fetch when needed
* EAGER = fetch immediately
* The first important thing to know is that all intermediate operations 
* (the functions that return a new sequence) are not executed until a terminal operation has been called. 
* Sequences are evaluated lazily to avoid examining all of the input data when it's not necessary.
* filter, map, distinct and sorted are intermediate
* toList and others are terminal
* asSequence() is used to change list to sequence
*/
// This will print nothing unless calling a terminal operation 
sequenceOf("A", "B", "C")
    .filter {
        println("filter: $it")
        true
    }

// But now it will be print the values
sequenceOf("A", "B", "C")
    .filter {
        println("filter: $it")
        true
    }
    .forEach {
        println("forEach: $it")
    }
// Output : 
// filter:  A
// forEach: A
// filter:  B
// forEach: B
// filter:  C
// forEach: C

// In the example below,
// c is never processed due to the early exit characteristic of sequences. 
// The terminal function any stops further processing of elements as soon as the given predicate function yields true. 
// This can greatly improve the performance of your application when working with large input collections.
val result = sequenceOf("a", "b", "c")
    .map {
        println("map: $it")
        it.toUpperCase()
    }
    .any {
        println("any: $it")
        it.startsWith("B")
    }

println(result)
// Output : 
// map: A
// any: A
// map: B
// any: B
// true

/**
* Collections vs. Sequences
* Another huge benefit of sequences over collections is lazy evaluation. 
* Since collection operations are not lazily evaluated you see huge performance increases when using sequences in certain scenarios.
*/
fun measure(block: () -> Unit) {
    val nanoTime = measureNanoTime(block)
    val millis = TimeUnit.NANOSECONDS.toMillis(nanoTime)
    print("$millis ms")
}

val list = generateSequence(1) { it + 1 }
    .take(50_000_000)
    .toList()

measure {
    list
        .filter { it % 3 == 0 }
        .average()
}
// 8644 ms

val sequence = generateSequence(1) { it + 1 }
    .take(50_000_000)

measure {
    sequence
        .filter { it % 3 == 0 }
        .average()
}
// 822 ms

/**
* Generic Functions
*/
// Implementation of standart library's partition() method
fun <T, V: MutableCollection<T>> Collection<T>.partitionTo(list1: V,list2: V, predicate: (T) -> Boolean): Pair<V, V> {
    for(item in this) {
        if(predicate(item)) {
            list1.add(item)
        } else {
            list2.add(item)
        }
    }
    return Pair(list1, list2)
}

val (words, lines) = listOf("a", "a b", "c", "d e").partitionTo(ArrayList(), ArrayList()) { s -> !s.contains(" ") }
val (letters, other) = setOf('a', '%', 'r', '}').partitionTo(HashSet(), HashSet()) { c -> c in 'a'..'z' || c in 'A'..'Z' }
```

**REFERENCES**
: [https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011](https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011)
: [https://winterbe.com/posts/2018/07/23/kotlin-sequence-tutorial/](https://winterbe.com/posts/2018/07/23/kotlin-sequence-tutorial/)

### [Go back to Kotlin section](../kotlin)
