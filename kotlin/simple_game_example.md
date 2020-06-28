# Kotlin Simple Game Code

```kotlin
import kotlin.math.absoluteValue

enum class Directions(val shortName: String? = null) {
    NORTH("n"),
    SOUTH("s"),
    EAST("e"),
    WEST("w"),
    START,
    END;
}

class Location (val width: Int = 4, val height: Int = 4) {

    private val map = Array(width) { arrayOfNulls<String>(height) }

    var currentLocation = Pair (0,0)

    fun updateLocation(direction: Directions) {

        when (direction) {
            Directions.NORTH -> {
                currentLocation = Pair(currentLocation.first, (currentLocation.second + 1).rem(height))
            }
            Directions.SOUTH -> {
                currentLocation = Pair(currentLocation.first, (currentLocation.second - 1).absoluteValue.rem(height))
            }
            Directions.EAST -> {
                currentLocation = Pair((currentLocation.first + 1).rem(width), currentLocation.second)
            }
            Directions.WEST -> {
                currentLocation = Pair((currentLocation.first - 1).absoluteValue.rem(width), currentLocation.second)
            }
        }
    }

    fun getDescription ():String? {
        return map[currentLocation.first.rem(width)][currentLocation.second.rem(height)]
    }

    init {
        map[0][0] = "You are at the start of your journey. [n / e]"
        map[0][1] = "The road stretches before you. It promises beauty and adventure. [ n / s / e]"
        map[0][2] = "The road still stretches before you. It rains and the water forms puddles. [ n / s / e]"
        map[0][3] = "It is getting much colder and you wish you had a wool coat. [ s / e]"

        map[1][0] = "The narrow path stretches before you. You are glad you are on foot. [ n / e /w]"
        map[1][1] = "It is getting warmer. You smell moss, and marmot scat. You are stung by a mosquito. [ n / s / e / w]"
        map[1][2] = "You decide to sit on your backside and slide down a vast snowfield. [ n / s / e /w]"
        map[1][3] = "You have climbed to the top of a snowy mountain and are enjoying the view. [ w / s / e]"

        map[2][0] = "You cross an old stone bridge. Your hear the murmuring of water. And another grumbling sound. [ n / e / w]"
        map[2][1] = "A bridge troll appears. It swings a club and demands gold. You give them all your coins. [ n / s / e  /w]"
        map[2][2] = "It is getting cooler. A dense fog rolls in. You can hear strange voices. [ n / s / e / w]"
        map[2][3] = "The foothills promise a strenuous journey. You thread your way around gigantic boulders. [ s / e / w ]"

        map[3][0] = "Your journey continues. A fox runs across the path with a chicken in its mouth.[ n / e ]"
        map[3][1] = "In the distance, you see a house. You almost bump into a farmer with a large shotgun. They pay you no heed. [ n / s / w ]"
        map[3][2] = "It is getting hot, and dry, and very, very quiet. You think of water and wish you had brought a canteen.[ n / s / w ]"
        map[3][3] = "You have reached the infinite desert. There is nothing here but sand dunes. You are bitten by a sand flea.[ s / w ] "
    }
}

class Game {
    private var path: MutableList<Directions> = mutableListOf(Directions.START)
    val map = Location()

    private val north = {
        path.add(Directions.NORTH)
        true
    }
    private val south = {
        path.add(Directions.SOUTH)
        true
    }
    private val east = {
        path.add(Directions.EAST)
        true
    }
    private val west = {
        path.add(Directions.WEST)
        true
    }
    private val end: () -> Boolean = {
        path.add(Directions.END)
        println("Game Over\n ${path.joinToString(separator = " - ")}")
        path.clear()
        false
    }

    private fun move(where: () -> Boolean): Boolean {
        return where()
    }

    fun makeMove(argument: String?): Boolean {
        /*
        if(Directions.values().toList().map { it.shortName }.contains(argument)) {}
        */
        return when(argument) {
            Directions.WEST.shortName -> {
                map.updateLocation(Directions.WEST)
                move(west)
            }
            Directions.EAST.shortName -> {
                map.updateLocation(Directions.EAST)
                move(east)
            }
            Directions.NORTH.shortName -> {
                map.updateLocation(Directions.NORTH)
                move(north)
            }
            Directions.SOUTH.shortName -> {
                map.updateLocation(Directions.SOUTH)
                move(south)
            }
            else -> move(end)
        }
    }
}

fun main(args: Array<String>) {
    val game = Game()
    while (true) {
        print("Enter a direction: n/s/e/w:")
        if(!game.makeMove(readLine())) {
            break
        } else {
            println(game.map.getDescription())
        }

    }
}
```

**REFERENCES**
: <https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011>

### [Go back to Kotlin section](../kotlin)
