# Kotlin Generics

```kotlin
/**
* GENERIC Classes
*/
class Aquarium<T> (val waterSupply: T) // Nullable
class Aquarium<T: Any> (val waterSupply: T) // NonNull
class Aquarium<T: WaterSupply> (val waterSupply: T) // Extends from specific class (Define bounds)

open class WaterSupply(var needProcessed: Boolean) {
    // Writes outer class name
    override fun toString(): String {
        return "${this::class.simpleName}(needProcessed=$needProcessed)"
    }
}

class TapWater: WaterSupply(true) {
    fun addChemicalCleaners() {
        needProcessed = false
    }
}

class FishStoreWater: WaterSupply(false)

class LakeWater: WaterSupply(true) {
    fun filter() {
        needProcessed = false
    }
}

class Aquarium<T: WaterSupply> (val waterSupply: T) {
    fun addWater() {
        check(!waterSupply.needProcessed) { "water supply needs processed" } // throws error (IllegalStateException) if condition doesn't match

        println("adding water from $waterSupply")
    }
}

fun genericExample() {
    // val aquarium = Aquarium<TapWater>(TapWater()) 
    val aquarium = Aquarium(TapWater()) // Kotlin automatically detects class type
    aquarium.waterSupply.addChemicalCleaners()

    val lakeAquarium = Aquarium(LakeWater())
    lakeAquarium.waterSupply.filter()
    lakeAquarium.addWater() // it will be throws exception unless we didn't call filter method
}

/**
* IN and OUT keywords for Generic Classes
* For ‘in' generic, we could assign a class of super-type to class of subtype
* For 'out' generic, we could assign a class of subtype to class of super-type
*
* In Kotlin List class uses out keyword
* The out keyword says that methods in a List can only return type E and they cannot take any E types as an argument.
* This limitation allows us to make List "covariant"
*
* In Kotlin Compare class uses in keyword
* This means that all methods in Compare can have T as an argument but cannot return T type.
* This makes Compare "contravariant"
*/
interface Production<out T> {
    fun produce(): T
}

interface Consumer<in T> {
    fun consume(item: T)
}

open class Food()
open class FastFood(): Food()
class Burger(): FastFood()

class FoodStore: Production<Food> {
    override fun produce(): Food {
        println("Produce food")
        return Food()
    }
}


class FastFoodStore: Production<FastFood> {
    override fun produce(): FastFood {
        println("Produce fastFood")
        return FastFood()
    }
}


class BurgerStore: Production<Burger> {
    override fun produce(): Burger {
        println("Produce Burger")
        return Burger()
    }
}

class Everybody: Consumer<Food> {
    override fun consume(item: Food) {
        println("Eat food")
    }
}

class ModernPeople: Consumer<FastFood> {
    override fun consume(item: FastFood) {
        println("Eat FastFood")
    }
}

class American: Consumer<Burger> {
    override fun consume(item: Burger) {
        println("Eat Burger")
    }
}

val production1: Production<Food> = FoodStore()
val production2: Production<Food> = FastFoodStore() // Inorder to make this assignment, Production<out T> class must use out
val production3: Production<Food> = BurgerStore() // Inorder to make this assignment, Production<out T> class must use out

production1.produce() // Output : Produce food
production2.produce() // Output : Produce FastFood
production3.produce() // Output : Produce Burger

val consumer1: Consumer<Burger> = Everybody()
val consumer2: Consumer<Burger> = ModernPeople()
val consumer3: Consumer<Burger> = American()

consumer1.consume(Burger()) // Output : Eat food
consumer2.consume(Burger()) // Output : Eat FastFood
consumer3.consume(Burger()) // Output : Eat Burger

/**
* Generic Functions
*/
fun <T: WaterSupply> isWaterClean(aquarium: Aquarium<T>) {
    println("aquarium water is clean: ${!aquarium.waterSupply.needProcessed}")
}
isWaterClean<TapWater>(aquarium)
isWaterClean(lakeAquarium) // we dont need to define class type explicitly, as it's already shown in the argument

/**
 * Inline reified keywords
* Non reified types are only available at compile time but can't be used at runtime by your program
* If we want to use generic type as real type we need to use reified keyword (we can check if any class is my generic type class)
* All generics types are only used at compile time by Kotlin
* However at runtime, all the generic types are erased
*/

fun <R: WaterSupply> hasWaterSupplyOfType() = waterSupply is R // Gives error
inline fun <reified R: WaterSupply> hasWaterSupplyOfType() = waterSupply is R // Correct

// We can use reified types for regular functions, even extension functions
inline fun <reified T: WaterSupply> WaterSupply.isOfType(): Boolean {
    println("Type of T is ${T::class.simpleName}")
    return this is T
}
aquarium.waterSupply.isOfType<LakeWater>() // false - since aquarium is type of TapWater

// Asterisk (*) means any class no matter what generic type is
inline fun <reified R: WaterSupply> Aquarium<*>.hasWaterSupplyOfTypeExtension() = waterSupply is R
aquarium.hasWaterSupplyOfTypeExtension<FishStoreWater>() // false

```

**REFERENCES**
: [https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011](https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011)

### [Go back to Kotlin section](../kotlin)
