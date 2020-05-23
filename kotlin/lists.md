[Go to Kotlin section](../kotlin)

```kotlin
/**
  * ————————————————————————————————————————
  * List - MutableList
  * List is read-only (immutable), you cannot add or update items in the original list.
  * MutableList inherites List and supports read/write access, you can add, update or remove items.
  * List & MutableList are ordered collections in which the insertion order of the items is maintained.
  * List & MutableList allows duplicates and null values
  * ————————————————————————————————————————
  */
// Initialization
val myList: List<String> = listOf("ali", "veli");
val myList = mutableListOf("ali", "ahmet", "mehmet")
val myList = (0..1000).toMutableList()
val anyList = listOf("test", 2019, null)
val anyList = listOfNotNull("test", 2019, null)

val intList = List(5) { i -> i }
//intList = [0, 1, 2, 3, 4]

val intListReversed = MutableList(5) { i -> 4 - i }
//intListReversed = [4, 3, 2, 1, 0]

// Remove item
myList.removeAt(0)
//res12: kotlin.String = ahmet

// Add Item to List
// List doesn’t accept us to add items. So we can only do this with MutableList.

// Add item to list using add() method.
myList.add("newItem");

// Insert item into the list at the specified index. // starts at 0
myList.add(1, "otherItem")

// Add item to list using plus (+) operator.
myList += "kotlin"

// Add whole list to antoher list with 'addAll' method
val anotherList = listOf("item1", "item2")
myList.addAll(2, anotherList) // Add new list to specific index

// Combine multiple lists
val combinedlist1 = list1.plus(list2).plus(list3)
val combinedlist2 = list1 + list2 + list3

// Access items from List
myList.isNullOrEmpty()
myList[2]
myList.get(2)
myList.getOrElse(6, { -1 })  // -1
myList.getOrNull(6)          // null

// Get sublist - These methods dont change original list, instead they create another list
myList.subList(2, 5)              // [2, 3, 4]
myList.slice(2..5)                // [2, 3, 4, 5]

// Take until from list
val nums = listOf(0, 1, 5, 9, 4, 2, 6, 7, 8)
nums.take(3)                    // [0, 1, 5]
nums.takeWhile { it < 5 }       // [0, 1]
nums.takeLast(3)                // [6, 7, 8]
nums.takeLastWhile { it != 4 }   // [2, 6, 7, 8]

// Drop (Opposite of take)
val nums = listOf(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
// Other methods are also same (dropWhile, dropLast, dropLastWhile)
nums.drop(3)                    // [3, 4, 5, 6, 7, 8]

// Update/Replace item in List
val list = mutableListOf<Any?>(0,1,2,3)
list.set(3, "three")           // [0, 1, 2, three]
list[0] = "zero"               // [zero, 1, 2, three]
list.replaceAll { "$it item" } // [0 item, 1 item, 2 item, 3 item]
list.replaceAll { e -> e.toString().capitalize() + ".com" }
// [0 item.com, 1 item.com, 2 item.com, 3 item.com]
list.fill(null)                // [null, null, null, null]

// Remove Item With Condition
myList.removeIf { item -> item % 2 == 0 }
myList.removeIf { item in (0..100).toList() }
anyList.removeIf { item -> item is Int }

// Predicate creation
val condition: (String) -> Boolean = { text -> text.startsWith("P") }
val condition2: (String) -> Boolean = { it.startsWith("P") }
myList.removeIf(condition)

// Iterate over List
val numbers = listOf(0, 1, 2, 3, 4, 5)
// forEach() method.
numbers.forEach { i -> print(">$i ") }

// Simple for loop
for (number in numbers) {
	print(">$number ")
}

// ListIterator and a while loop.
val listIterator = numbers.listIterator()
while (listIterator.hasNext()) {
	val i = listIterator.next()
	print(">$i ")
}

var prevItem = list.getOrElse(0, {0});
list.forEach { item ->
    println("prevItem : $prevItem - item : $item\n")
    if(item < prevItem) {list.remove(item)}
    prevItem = item
}

// Delete items in loop
val list = mutableListOf(0,1,4,87, 33, 45, 48, 51, 23, 20, 10, 15, 13, 17, 100, 1000, 70)
var prevItem = list.getOrElse(0, {0});
 val listIterator = list.listIterator()
 while(listIterator.hasNext()) {
     val index = listIterator.nextIndex()
     val item = listIterator.next()
     println("prevItem : $prevItem - item : $item\n")
     if(item < prevItem) {
         println("item deleted : $item\n")
         listIterator.remove()
     }
     prevItem = item
 }

// Alternate of remove items for simple cases
list.removeIf { it % 10 == 0 }

// For loop with item index
val startIndex = 1
for (i in startIndex until numbers.size) {
	print(">${numbers[i]} ")
}

val list = mutableListOf(0,1,4,87, 33, 45, 48, 51, 23, 20, 10, 15, 13, 17, 100, 1000, 70)
for((index, item) in list.withIndex()) {
     print("Index : $index - Item : $item\n")
 }

// Reverse List
// Get a new List with revered order using reversed() method.
val numbers = mutableListOf(0, 1, 2, 3)
val reversedNumbers = numbers.reversed()
// reversedNumbers: [3, 2, 1, 0]
numbers[3] = 5
// numbers:         [0, 1, 2, 5]
// reversedNumbers: [3, 2, 1, 0] - this doesnt change

// Get a reversed read-only list of the original List as a reference using asReversed() method. 
// If we update item in original list, the reversed list will be changed too.
val numbers = mutableListOf(0, 1, 2, 3)
val refReversedNumbers = numbers.asReversed()
// refReversedNumbers: [3, 2, 1, 0]
numbers[1] = 8
// numbers:            [0, 8, 2, 3]
// refReversedNumbers: [3, 2, 8, 0] - this changes too

// Filter List
data class Animal (val name:String)
val animals = listOf(Animal("Lion"), Animal("Elephant"), Animal("Bug"))
// animals = [Animal(name=Lion), Animal(name=Elephant), Animal(name=Bug)]
val animalsWithN = animals.filter { it.name.contains("n") }
// animalsWithN = [Animal(name=Lion), Animal(name=Elephant)]

// Check items in list
val list =
	listOf("bezkoder", 2019, "kotlin", "tutorial", "bezkoder.com", 2019)

list.contains("bezkoder")                          // true
list.contains("zkoder")                            // false

"bezkoder" in list                                 // true

list.containsAll(listOf("bezkoder", "tutorial"))   // true
list.containsAll(listOf("zkoder", "tutorial"))     // false

list.indexOf(2019)       // 1
list.lastIndexOf(2019)   // 5

list.indexOf(2020)       // -1
list.lastIndexOf(2020)   // -1

list.indexOfFirst { e -> e.toString().startsWith("bez") }     // 0
list.indexOfFirst { e -> e.toString().startsWith("zkoder") }  // -1

list.indexOfLast { e -> e.toString().endsWith("9") }          // 5
list.indexOfLast { e -> e.toString().endsWith(".net") }       // -1

list.find { e -> e.toString().startsWith("bez") }             // bezkoder
list.find { e -> e.toString().startsWith("zkoder") }          // null

list.findLast { e -> e.toString().endsWith("9") }             // 2019
list.findLast { e -> e.toString().endsWith(".net") }          // null

// Sort List
// Sort() method to is used for sort a Mutable List in-place, and sortDescending() for descending order.
nums.sort()
nums.sortDescending();

// Sorted() and sortedDescending() don’t change the original List. Instead, they return another sorted List.
val sortedNums = nums.sorted()
val sortedNumsDescending = nums.sortedDescending()

// Sort by custom filter
data class MyDate (val month:Int, val day:Int)
val myDates = mutableListOf(
	MyDate(4, 3),
	MyDate(5, 16),
	MyDate(1, 29)
)
myDates.sortBy { it.month }
myDates.sortByDescending { it.month } // descending version

// Sorted by is equal to sorted -with filter. 
// don’t change the original List. Instead, they return another sorted List.
val sortedDates = myDates.sortedBy { it.month }

// Sort with complex custom filters
val myDates = mutableListOf(
	MyDate(8, 19),
	MyDate(5, 16),
	MyDate(1, 29),
	MyDate(5, 10),
	MyDate(8, 3)
)
myDates.sortWith(compareBy { it.month })

// Another sort example
val monthComparator = compareBy<MyDate> { it.month }
val dayThenMonthComparator = monthComparator.thenBy { it.day }
myDates.sortWith(dayThenMonthComparator)

// A complex example
class MyDateComparator {
     companion object : Comparator<MyDate> {
         override fun compare(o1: MyDate, o2: MyDate): Int = when {
             o1.month != o2.month -> o1.month - o2.month
             o1.day != o2.day -> o1.day - o2.day
             else -> 0
         }
     }
 }
myDates.sortWith(MyDateComparator)

// SOME USEFUL LIST METHODS
// joinToString
println(numbers.joinToString(prefix = "<", postfix = ">", separator = "•")) // <1•2•3•4•5•6>
val chars = charArrayOf('a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q')
println(chars.joinToString(limit = 5, truncated = "...!") { it.toUpperCase().toString() }) // A, B, C, D, E, ...!

val max = nums.max()
val min = nums.min()
val sum = nums.sum()
val avg = nums.average()

val cars = listOf(Car("Mazda", 6300), Car("Toyota", 12400),
        Car("Skoda", 5670), Car("Mercedes", 18600))
val car = cars.maxBy { car -> car.price }

// Kotlin List map
// The mapping operation returns a modified list by applying a transform function on each element of the list.
val nums2 = nums.map { e -> e * 2 } // every item multipled by 2 in the nums list as new list

// Kotlin List reduction
// Reduction is a terminal operation that aggregates list values into a single value. 
// The reduce() method applies a function against an accumulator and each element 
// (from left to right) to reduce it to a single value.
val nums = listOf(4, 5, 3, 2, 1, 7, 6, 8, 9)
val sum = nums.reduce { total, next -> total + next }
println(sum)
// reduceRight is the same logic but iterate on reversed list 

// Kotlin List group by
// The groupBy() method groups elements of the original list by the key returned by the given selector function, 
// applied to each element. It returns a map where each group key is associated with a list of corresponding elements.
val words = listOf("as", "pen", "cup", "doll", "my", "dog", "spectacles")
val res = nums.groupBy { if (it % 2 == 0) "even" else "odd" }
println(res)
// Result : {odd=[1, 3, 5, 7], even=[2, 4, 6, 8]}
val res2 = words.groupBy { it.length }
println(res2)
// Result : {2=[as, my], 3=[pen, cup, dog], 4=[doll], 10=[spectacles]}

// Kotlin List any
// The any() method returns true if at least one element matches the given predicate function.
val nums = listOf(4, 5, 3, 2, -1, 7, 6, 8, 9)
val r = nums.any { e -> e > 10 } // r is false

// Kotlin List all
// The all() returns true if all elements satisfy the given predicate function.
val nums = listOf(4, 5, 3, 2, -1, 7, 6, 8, 9)
val r = nums.all { e -> e > 0 } // r is false

// Some examples
val spices = listOf("curry", "pepper", "cayenne", "ginger", "red curry", "green curry", "red pepper" )
val newSpices = spices.withIndex().filter { (index,item) -> index < 3 && item.contains('c') }.map { it.value }
val newSpices2 = spices.take(3).filter { item -> item.contains('c') }
println(newSpices)

/**
* Pairs (key-value)
*/
val equipment = "fishnet" to "catching" // Simple Pair definition
val equipment = "fishnet" to 12 to "bruce" to "lee" to true // Pair chain
equipment.first.first.second // Output: bruce
equipment.first // Output: (((fishnet, 12), bruce), lee)
val equipment = ("fishnet" to 12) to ("bruce" to "lee") // We can use paranthesis for pairs
equipment.second.first // Output: bruce

// We can use pairs to return multiple variables on functions. 
// And we can use them as destructure the pair into variables
fun giveMeATool(): Pair<String, String> {
    return ("fishnet" to "cactching fish")
}
val(tool, use) = giveMeATool()
println("Tool $tool usage is $use") // Output: Tool fishnet usage is cactching fish

// Pair could be convert to list with toList() method
equipment.toList() // Output : [fishnet, catching]

/**
* Triple
* is just like pair but with three variables
*/
val (a, b, c) = Triple(2, "x", listOf(null))

/**
* MAPS
* Map is list of pairs
*/
val cures: MutableMap<String, String> = mutableMapOf("white spots" to "Ich", "red sores" to "hole diseases")
cures.putIfAbsent("white spots", "whites"); // It will not be added since the key already exist in map
cures.getOrDefault("purple spots", "Nope") // Output: Nope

println(cures.getOrPut("purple spots") {
        println("new value added for purple spots")
        val myVal = "Purples"
        myVal
})
// Output : 
// new value added for purple spots
// Purples

/**
* SET
* Set<T> stores unique elements; their order is generally undefined. 
* null elements are unique as well: a Set can contain only one null. 
* Two sets are equal if they have the same size, and for each element of a set there is an equal element in the other set.
* The default implementation of Set – LinkedHashSet – preserves the order of elements insertion
*/
val numbers = setOf(1, 2, 3, 4)
println("Number of elements: ${numbers.size}") // 4
if (numbers.contains(1)) println("1 is in the set") // true

val numbersBackwards = setOf(4, 3, 2, 1)
println("The sets are equal: ${numbers == numbersBackwards}") // true (order is not important)

// Example of SET and MAP
val allBooks = setOf("Macbeth", "Romeo and Juliet", "Hamlet", "A Midsummer Night's Dream")
val library = mapOf("Shakespeare" to allBooks)
println(library) // Output : {Shakespeare=[Macbeth, Romeo and Juliet, Hamlet, A Midsummer Night's Dream]}
println(library["Shakespeare"]?.elementAt(2)) // Output : Hamlet

// Return true if all customers are from a given city
fun Shop.checkAllCustomersAreFrom(city: City): Boolean =
        customers.all { it.city == city }

// Return true if there is at least one customer from a given city
fun Shop.hasCustomerFrom(city: City): Boolean =
        customers.any { it.city == city }

// Return the number of customers from a given city
fun Shop.countCustomersFrom(city: City): Int =
        customers.count { it.city == city }

// Return a customer who lives in a given city, or null if there is none
fun Shop.findCustomerFrom(city: City): Customer? =
        customers.find { it.city == city }

/**
* "FlatMap" example
*/
data class Shop(val name: String, val customers: List<Customer>)

data class Customer(val name: String, val city: City, val orders: List<Order>) {
    override fun toString() = "$name from ${city.name}"
}

data class Order(val products: List<Product>, val isDelivered: Boolean)

data class Product(val name: String, val price: Double) {
    override fun toString() = "'$name' for $price"
}

data class City(val name: String) {
    override fun toString() = name
}
// Return the most expensive product that has been ordered by the given customer
fun getMostExpensiveProductBy(customer: Customer): Product? =
        customer.orders
                .flatMap{
                    it.products
                }.maxBy{
                    it.price
                }
// Same method, different representation
fun getMostExpensiveProductBy(customer: Customer): Product? =
        customer.orders
                .flatMap(Order::products)
                .maxBy(Product::price)

// Another example
// Return all products the given customer has ordered
fun Customer.getOrderedProducts(): List<Product> =
        orders.flatMap(Order::products)

// Return all products that were ordered by at least one customer
fun Shop.getOrderedProducts(): Set<Product> =
        // customers.flatMap(Customer::orders).flatMap(Order::products).toSet()
	// Another representation
	customers.flatMap(Customer::getOrderedProducts).toSet()

/**
* associateBy
* Returns a Map containing the elements from the given sequence indexed by the key
* returned from keySelector function applied to each element.
*/
data class Person(val firstName: String, val lastName: String) {
    override fun toString(): String = "$firstName $lastName"
}
val scientists = listOf(Person("Grace", "Hopper"), Person("Jacob", "Bernoulli"), Person("Johann", "Bernoulli"))
val byLastName = scientists.associateBy { it.lastName }
// Jacob Bernoulli does not occur in the map because only the last pair with the same key gets added
println(byLastName) // {Hopper=Grace Hopper, Bernoulli=Johann Bernoulli}

val byLastName2 = scientists.associateBy({ it.lastName }, { it.firstName })
// Jacob Bernoulli does not occur in the map because only the last pair with the same key gets added
println(byLastName2) // {Hopper=Grace, Bernoulli=Johann}

/**
* Partition
*/
val numbers = listOf("one", "two", "three", "four")
val (match, rest) = numbers.partition { it.length > 3 }

// Return customers who have more undelivered orders than delivered
fun Shop.getCustomersWithMoreUndeliveredOrders(): Set<Customer> = customers.filter {
    val (delivered, undelivered) = it.orders.partition { order -> order.isDelivered }
    undelivered.size > delivered.size
}.toSet()

/**
* FOLD
* This function helps to accumulate value starting with initial value, 
* then apply operation from left to right to current accumulator value and each element.
* foldRight() -> right to letf
*/
println(listOf(1, 2, 3, 4, 5).fold(1) { mul, item -> mul * item }) // (Initial)1 * 1*2*3*4*5 => 120
println(listOf(1, 2, 3, 4, 5).fold(3) { mul, item -> mul * item }) // 3 * 1*2*3*4*5 => 360
println(listOf(71, 79, 67, 68, 64).fold("Tadaaa:") { char, item -> char + item.toChar() }) // Output: Tadaaa:GOCD@

// Another example for fold
fun Customer.getOrderedProducts(): List<Product> =
        orders.flatMap(Order::products)

/**
* Intersect
* Intersect gets same values on two lists
*/
// Return the set of products that were ordered by all customers
fun Shop.getProductsOrderedByAll(): Set<Product> {
    val allOrderedProducts = customers.flatMap { it.getOrderedProducts() }.toSet()
    // Gets allOrderedProducst as initial and crop its value by the customer order values
    return customers.fold(allOrderedProducts, { orderedByAll, customer ->
        orderedByAll.intersect(customer.getOrderedProducts())
    })
}

fun main(args: Array<String>) {
    val item1 = Product("Köfte", 12.3)
    val item2 = Product("Pattes", 22.3)
    val item3 = Product("Dolma", 32.3)
    val item4 = Product("Antrikot", 2.3)
    val item5 = Product("Bezelye", 12.7)
    val customer1 = Customer("Ali",City("Istanbul"), listOf(Order(listOf(item1, item3), true), Order(listOf(item1, item5), false)))
    val customer2 = Customer("Ayse",City("Ankara"), listOf(Order(listOf(item1, item2, item3), true)))
    val customer3 = Customer("Veli",City("Bolu"), listOf(Order(listOf(item5), true), Order(listOf(item1, item5), false)))
    val shop = Shop("lokanta", listOf(customer1, customer2, customer3))
    println(shop.getProductsOrderedByAll()) // Output: ['Köfte' for 12.3]
}

// Count the amount of times a product was ordered.
// Note that a customer may order the same product several times.
fun Shop.getNumberOfTimesProductWasOrdered(product: Product): Int {
    return customers.flatMap(Customer::getOrderedProducts).count { it == product }
    // Long way
    // return customers.flatMap(Customer::getOrderedProducts).groupBy { it.name }.getValue(product.name).size
}

/**
* distinct()
* Returns a sequence containing only distinct elements from the given sequence.
*/
val list = listOf('a', 'A', 'b', 'B', 'A', 'a')
// just like distinct we can use toSet() method either
println(list.distinct()) // [a, A, b, B]
println(list.distinctBy { it.toUpperCase() }) // [a, b]
```

### REFERENCES
* https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011
* https://kotlinlang.org/docs/reference/collections-overview.html
* https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-triple/
* https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/associate-by.html
* https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/join-to-string.html
* https://bezkoder.com/kotlin-list-mutable-list/
* https://bezkoder.com/kotlin-fold/
* https://dzone.com/articles/learning-kotlin-return-when
* http://zetcode.com/kotlin/lists/
