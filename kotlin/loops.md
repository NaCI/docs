# Kotlin Loops

```kotlin
/**
* LOOPS
*/
// Simple For Loops
for(i in 'b'..'h') print(i) 	//bcdefgh
for(i in 1..5) print(i) 	//12345
for(i in 5 downTo 1) print(i) 	//54321
for(i in 1..16 step 3) print(i)	//147101316

// Continue and Break
outer@ for (n in 2..100) {
     for (d in 2 until n) {
         if (n % d == 0) continue@outer
     }
     println("$n is prime\n")
 }

// Usage of WHEN statement
// We can avoid the next evolution by creating a variable and returning it directly:
fun step2(number: Int):String {
    when (number) {
        0 -> return "Zero"
        1 -> return "One"
        2 -> return "Two"
    }
    return ""
}

// And, now, we get to the cool part — we can just return the when !
fun step3(number: Int):String {
    return when (number) {
        0 -> "Zero"
        1 -> "One"
        2 -> "Two"
        else -> ""
    }
}

// REPEAT (continue and break keyword are unusable inside repeat function)
repeat(3) { // number of repeat count
     val randomDay = Random(4).nextInt()
     println(randomDay.toString())
}

// DO WHILE
do {
   val fortune = getFortuneCookie(getBirthday())
   println(fortune)
} while (!fortune.contains("Take it easy"))
```

**REFERENCES**
: <https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011>
: <https://kotlinlang.org/docs/reference/control-flow.html>
: <https://dzone.com/articles/learning-kotlin-return-when>

### [Go back to Kotlin section](../kotlin)
