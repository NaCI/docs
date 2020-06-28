# Kotlin Basics

```kotlin
/**
* ANY type holds any type of object.
* The root of the Kotlin class hierarchy. Every Kotlin class has Any as a superclass.
*/

/**
* Almost everything in Kotlin is an expression with a value, but there a few exceptions.
* WHILE loops and FOR loops are not expression with a value
*/

/**
  * ————————————————————————————————————————
  * NULL Safety - Safe Calls
  * safe call operator, written ?.
  * ————————————————————————————————————————
  */
// init nullable item list with values
val listWithNulls: List<String?> = listOf("Kotlin", null, "Java", null, null, "C#")
  // iterate over all items in the list
  for(item in listWithNulls) {
    // check if item is null or not
    // let => provides to implement function for object
    // print values if item is not null
     item?.let { println(it+"\n") }
  }

// another safe call example
bob?.department?.head?.name

// If either `person` or `person.department` is null, the function is not called:
person?.department?.head = managersPool.getManager()

/**
  * ————————————————————————————————————————
  * Elvis Operator
  * ————————————————————————————————————————
  */
// When we have a nullable reference b, we can say "if b is not null, use it, otherwise use some non-null value":
val l: Int = if (b != null) b.length else -1
// Along with the complete if-expression, this can be expressed with the Elvis operator, written ?::
val l = b?.length ?: -1

// Note that, since throw and return are expressions in Kotlin, they can also be used on the right hand side of the elvis operator. 
// This can be very handy, for example, for checking function arguments:
fun foo(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("name expected")
    // ...
}

/**
  * ————————————————————————————————————————
  * !! Operator
  * The third option is for NPE-lovers: 
  * the not-null assertion operator (!!) converts any value to a non-null type and 
  * throws an exception if the value is null. We can write b!!, and this will return a non-null value of b (e.g., a String in our example) or throw an NPE if b is null:
  * ————————————————————————————————————————
  */
val l = b!!.length

/**
  * ————————————————————————————————————————
  * Safe Casts
  * Regular casts may result into a ClassCastException if the object is not of the target type. 
  * Another option is to use safe casts that return null if the attempt was not successful:
  * ————————————————————————————————————————
  */
val aInt: Int? = a as? Int

/**
  * ————————————————————————————————————————
  * Collections of Nullable Type
  * If you have a collection of elements of a nullable type and want to filter non-null elements, 
  * you can do so by using filterNotNull
  * ————————————————————————————————————————
  */
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()

/**
  * ————————————————————————————————————————
  * String Format
  * ————————————————————————————————————————
  */
val numberOfItems = 13.4
"I have $numberOfItems item in my backpack"
val numberOfThings = 12
"I have ${numberOfItems + numberOfThings} item in my backpack"


/**
  * ————————————————————————————————————————
  * Using Range with if-else
  * ————————————————————————————————————————
  */
val fish = 50
 if(fish in 1..100) println("good")
val character = "b"
 if(character in "a".."z") println("good")

/**
  * ————————————————————————————————————————
  * Switch - case alternate
  * ————————————————————————————————————————
  */
val numberOfThings = 12
when(numberOfThings) {
    0 -> println("No item")
    20 -> println("20 item")
    13,14 -> println("It is 13 or 14")
    in 10..15 -> println("Between 10 and 15")
    !in 0..10 -> println("Outside of the range 0 to 10")
    else -> println("Perfect")
}

/**
* Triple Quoted String
*/
val herSpeech = "'Oh c'mon'"
val myText = """    And -the man says that : "Am i -the only one here!".${"\n"} Then she replied $herSpeech """
println(myText.trim()) 
// Output : 
// And -the man says that : "Am i -the only one here!".
//  Then she replied 'Oh c'mon'

/**
* String Patterns
*/
fun getMonthPattern() = """\d{2}\.\d{2}\.\d{4}"""
"13.02.1992".matches(getMonthPattern().toRegex()) // true

val month = "(JAN|FEB|MAR|APR|MAY|JUN|JUL|AUG|SEP|OCT|NOV|DEC)"
fun getPattern(): String = """\d{2} $month \d{4}"""
"13 JAN 1992".matches(getPattern().toRegex()) // true

/**
* Nothing type
* Nothing type can be used as a return type for a function that always throws an exception. 
* When you call such a function, the compiler uses the information that it throws an exception.
*/
fun failWithWrongAge(age: Int?): Nothing { // If that was not return nothing nullable age param will give an error
    throw IllegalArgumentException("Wrong age: $age")
}

fun checkAge(age: Int?) {
    if (age == null || age !in 0..150) failWithWrongAge(age)
    println("Congrats! Next year you'll be ${age + 1}.")
}
checkAge(10)

/**
* Import Rename
* as NewName
*/
import kotlin.random.Random as KRandom
import java.util.Random as JRandom

/**
* Kotlin Idioms
* https://kotlinlang.org/docs/reference/idioms.html
*/

/**
* Kotlin Standard Library
* https://kotlinlang.org/api/latest/jvm/stdlib/
*/
```

**REFERENCES**
: [https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011](https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011)
: [https://kotlinlang.org/docs/reference/control-flow.html](https://kotlinlang.org/docs/reference/control-flow.html)
: [https://bezkoder.com/kotlin-list-mutable-list/](https://bezkoder.com/kotlin-list-mutable-list/)
: [https://dzone.com/articles/learning-kotlin-return-when](https://dzone.com/articles/learning-kotlin-return-when)

### [Go back to Kotlin section](../kotlin)
