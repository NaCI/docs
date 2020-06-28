# Kotlin Functions

```kotlin
/**
* FUNCTIONS
* Functions are declared using the fun keyword. 
* Functions that are defined inside a class or object are called member functions. 
* Values are returned using the return keyword.
* Unlike other languages in kotlin we dont need to define type for no return value functions
* Even no return type is specified, return type will be Unit
* Function parameters are provided between the "()". They always have the pattern name:Type
*/

/**
* IT
* A shorthand of a single argument lambda is to use the keyword ‘it'. 
* This value represents any lone that argument we pass to the lambda function
*/

fun hello(name: String, age: Int) {
  println("Hello $name with age $age")
}
// Funtions returns String
fun getHelloText(name: String): String {
    return "Hello $name";
}

// If a function body contains (returns) only a single expression, you reduce it to a single-line function.
fun getHelloText(name: String): String = "hello $name" 
// Interestingly we dont need to specify return type for single-line function
fun getHelloText(name: String) = "hello $name" // This will also work
fun getHelloText(name: String) { return "hello $name" } // This will NOT work. it will be error for not specifying return type 

/**
 * Main method to play around with
 */
fun main(args: Array<String>) {
    println("${getHelloText("naci").capitalize()} Hi, I am a simple string")
}

// Single Line Function Example with default arguments and Elvis operator
fun dayOfWeek(dayOfWeek: Int? = null): String = when(dayOfWeek ?: Calendar.getInstance().get(Calendar.DAY_OF_WEEK)) {
        1 -> "Sunday"
        2 -> "Monday"
        3 -> "Tuesday"
        4 -> "Wednesday"
        5 -> "Thursday"
        6 -> "Friday"
        7 -> "Saturday"
        else -> "Friday"
}

// Everything in Kotlin expression. We can use such statements like below
val isUnit = println("This is an expression")
println(isUnit)
// This is an expression
// kotlin.Unit

val temperature = 10
val isHot = temperature > 50
val message = "You are ${ if(isHot) "fried" else "safe"} fish" // Ternary operator for kotlin // we can simply use if-else block
println(message) // You are safe fish

val timeOfTheDay = args[0].toInt();
println(when {
	timeOfTheDay < 12 -> "Good morning, Kotlin"
	else -> "Good night, Kotlin"
})

fun getFortuneCookie(): String {
    val listOfFortunes = listOf(
        "You will have a great day!",
        "Things will go well for you today.",
        "Enjoy a wonderful day of success.",
        "Be humble and all will turn out well.",
        "Today is a good day for exercising restraint.",
        "Take it easy and enjoy life!",
        "Treasure your friends because they are your greatest fortune."
    )
    print("Enter your birthday: ")
    val birthday = readLine()?.toIntOrNull() ?: 1
    return listOfFortunes[birthday%listOfFortunes.size]
}

// LET method provides to do operations after function call // like do-while but not in loop (do-do) :)
getFortuneCookie().let { for (i in 1..3) println(it) } // this calls the method once and prints the result 3 times

// or call it in for loop several times
var fortune: String
for (i in 1..10) {
  fortune = getFortuneCookie()
  println("Your fortune is: $fortune")
  if(fortune.contains("Take it easy")) break
}

// Default Arguments for Functions
fun shouldChangeTheWater(
    day: String,
    temperature: Int = 22,
    isDirty: Boolean = false): Boolean {
    return false;
}

// Multiple usages of the default argument methods
fun main(args: Array<String>) {
    shouldChangeTheWater("Monday")
    shouldChangeTheWater("Monday", isDirty = true)
    shouldChangeTheWater("Monday", 23)
    shouldChangeTheWater(isDirty = true, day = "Monday", temperature = 12)
}

fun canAddFish(tankSize: Double, currentFish: List<Int>, fishSize: Int = 2, hasDecorations: Boolean = true): Boolean {
    return (tankSize * if (hasDecorations) 0.8 else 1.0) >= (currentFish.sum() + fishSize)
}

// Functions can be called as default arguments
fun calculateTemperatureAsCelsius(temperatureAsFahrenheit: Int) = ((temperatureAsFahrenheit - 32)*5)/9
fun shouldChangeWater(day: String, temperature: Int = calculateTemperatureAsCelsius(20)) {
    //...
}

// LAMBDA Function (Anonymous or nameless function)
{println("Heelo")} ()
val swim = {println("swim\n")} // Create and assign function to a variable
swim() // Output : swim
swim // Output : res2: () -> kotlin.Unit = () -> kotlin.Unit

// Use function arguments for lambdas
var dirty = 20
val waterFilter = { dirty: Int -> dirty / 2 }
waterFilter(dirty) // Output : 10

// Define argument and return type
val waterFilter: (Int) -> Int = { dirty -> dirty / 2 }
waterFilter(dirty) // Output : 10

// If the code returns no value we use the type Unit:
var link: (String) -> Unit = {s:String -> println(s) } // just "s" is ok too (without :String)
link("test")

// Lambda function with multiple arguments
val waterFilter = {dirty: Int, divider: Int -> dirty / divider}
waterFilter(10, 4) // Output : 2 
// Different presentation for same function
val waterFilter: (Int, Int) -> Int = { dirty, divider -> dirty / divider }
waterFilter(10, 4) // Output : 2

// HIGHER ORDER FUNCTION
// It is a function which takes a function as parameter
// List Filter and repeat functions are some example of higher-order functions from standard library

// Example
fun updateDirty(dirty: Int, operation: (Int) -> Int): Int {
    return operation(dirty)
}

// Different calls example for high order functions
var dirty = 20
val waterFilter: (Int) -> Int = { dirty -> dirty / 2 }
fun feedFish(dirty: Int) = dirty + 10

dirty = updateDirty(dirty, waterFilter) // use lambda function as parameter of another function
dirty = updateDirty(dirty, ::feedFish) // use name function as parameter of another function
dirty = updateDirty(dirty, { dirty -> dirty + 50 }) // create function as parameter of another function
dirty = updateDirty(dirty) { dirty -> dirty + 50 } // create function as parameter - short definition

val random1 = Random.nextInt(0, 100); // Output is same for every random1 call
// random1 has a value assigned at compile time, and the value never changes when the variable is accessed.
 val random2 = {Random.nextInt(0, 100)}; // // Output changes for every random2() call
// random2 has a lambda assigned at compile time, 
// and the lambda is executed every time the variable is referenced, returning a different value.

val rollDice: (Int) -> Int = {
    print("$it sided dice is rolling.. ")
    if(it == 0) 0
    else Random.nextInt(1, it) + 1
}
fun gamePlay(diceSide: Int, operation: (Int) -> Int) {
    println("The dice is : ${operation(diceSide)}")
}
gamePlay(4, rollDice)

// EXTENTION FUNCTION EXAMPLE
fun List<Int>.divisibleBy(operation: (Int) -> Int): List<Int> {
    var filteredList = mutableListOf<Int>()
    for(item in this) {
        if(operation(item) == 0) {
            filteredList.add(item)
        }
    }
    return filteredList
}
val numbers = listOf<Int>(1,2,3,4,5,6,7,8,9,0)
val divisibleNumbers = numbers.divisibleBy {
    it.rem(3)
}
println(divisibleNumbers) // Output : [3, 6, 9, 0]

// Different represantation for defining extention functions
val isEven: (Int) -> Boolean = { it.rem(2) == 0 }
val isOdd: Int.() -> Boolean = { this.rem(2) == 0 }
```

**REFERENCES**
: [https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011](https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011)

### [Go back to Kotlin section](../kotlin)
