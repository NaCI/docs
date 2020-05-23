[Go to Kotlin section](../kotlin)

```kotlin
// AN COMPLETE EXAMPLE
val Book.weight: Double
    get() = pages.times(1.5)

fun Book.tornPages(tornPages: Int): Int {
    pages = if (tornPages < pages) pages.minus(tornPages) else 0
    return pages
}

class Book(val title: String, val author: String, val year: String, var pages: Int) {

    companion object {
        const val MAX_NUMBER_OF_BOOKS_PER_USER = 30
    }

    fun printUrl() {
        println("${Constants.BASE_URL}/$title.html")
    }

    fun canBorrow(borrowedBookCount: Int): Boolean {
        return borrowedBookCount < MAX_NUMBER_OF_BOOKS_PER_USER
    }

    fun getTitleAndAuthor(): Pair<String, String> {
        return title to author
    }

    fun getTitleAndAuthorAndYear(): Triple<String, String, String> {
        return Triple(title, author, year)
    }

    override fun toString(): String {
        return "Book(title='$title', author='$author', year='$year', pages=$pages)"
    }
}

class Puppy {
    fun playWithBook(book:Book) {
        val tornedPageCount = Random.nextInt(0, book.pages) + 1
        book.tornPages(tornedPageCount).apply {
            println(
                "Sorry but puppy eat $tornedPageCount pages from the ${book.title}\n" +
                        if (this > 0) "Only $this pages left!!" else "Nothing left.."
            )
        }
    }
}

val myBook = Book("Satranc", "Stefan Zweig", "1997", 120)
//val(title, author) = myBook.getTitleAndAuthor()
val(title, author, year) = myBook.getTitleAndAuthorAndYear()
println("Here is your book $title written by $author in $year.")

val puppy = Puppy()
while(myBook.weight > 0) {
      puppy.playWithBook(myBook)
}
```
