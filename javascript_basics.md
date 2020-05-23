[Go to homepage](https://naci.github.io)

```javascript
Javascript Temelleri

————————————————————————————————————————
In JavaScript, there are seven fundamental data types
“ Number (4, 23.42)
“ String
“ Boolean
“ Null
“ Undefined
“ Symbol
“ Object
————————————————————————————————————————
String Interpolation
var ali = 22
var veli = 'asd'
console.log(`${ali-3} ve ${veli}`)
————————————————————————————————————————
Variables
var myName = 'Arya'; // Eski versiyon (block scoped)

let yourName; // ES6 ile gelmiş. Üstüne aynı değişken tekrar oluşturulamaz. Default değeri ‘undefined’ olur. (function scoped)
// Variables declared with let can be reassigned.

const theirName = ‘Vulululu’; //  ES6 ile gelmiş. Yeni değer set edilemez. Constant.
// elements in an array declared with const remain mutable
————————————————————————————————————————
TypeOf
Değişkenlerin tipini göstermek için kullanılır.

Check if variable type is string (use === for type comparison)
let ali = “CodeIgniter”;
if((typeof ali) === "string") {console.log("yes");}

//fonksiyon ataması yapılmış bir değişkenin tipi ‘function’ olarak dönüyor.

// == ile sorgulama yapılırsa tip kontrolü yapılmıyor (Örnek : if(‘1’==1) true döner)
// === ise tipi de sorgular
————————————————————————————————————————
Truthy and Falsy
check if a variable exists

let myVariable = ‘I Exist’;
if(myVariable) {
   console.log(myVariable)
} else {
   console.log('The variable does not exist.')
}

The list of falsy values includes:
* 0
* Empty strings like "" or ''
* null which represent when there is no value at all
* undefined which represent when a declared variable lacks a value
* NaN, or Not a Number
————————————————————————————————————————
Short-Circuit Evaluation
Koşul kontrolünde debugger en soldan okumaya başlar. En soldaki değer geçerliyse diğerlerine bakmaz. Bundan dolayı aşağıdaki gibi bir kullanım yapılabilir : 

var person = {
  name: 'Jack',
  age: 34
}
let job = person.job || 'unemployed';
console.log(job);
// 'unemployed'
————————————————————————————————————————
Ternary Operator
let isCorrect = true;
isCorrect ? console.log('Correct!') : console.log('Incorrect!');

let favoritePhrase = 'Love That!';
favoritePhrase === 'Love That!' ? console.log("I love that!") : console.log("I don't love that!");

let groceryItem = 'papaya';
————————————————————————————————————————
The Switch Keyword

let groceryItem = 'papaya';
switch (groceryItem) {
  case 'tomato':
  case 'lime':
    console.log('Tomatoes and Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}
————————————————————————————————————————
Functions

//Function creation example
function getReminder(value1, value2 = 5) {
  console.log(`Water ${value1} the ${value2} plants.`);
}
getReminder(‘ali’); getReminder(‘ali’, 3);
// Fonksiyonların parametlerinin default değerleri ‘undefined’ değeridir.

// Return example
function rectangleArea(width, height) {
  if (width < 0 || height < 0) {
    return 'You need positive integers to calculate area!';
  }
  return width * height;
}
————————————————————————————————————————
Function Expressions

const functionName = function(parameter) {
  if(parameter){
    return true;
  } else {
    return false;
  }
}
————————————————————————————————————————
Arrow Functions

const functionName = (parameter) => {
   console.log(parameter);
}

//Other writing types
const functionName = () => {};
const functionName = paramOne => {}; // accepts only single parameter
const functionName = (paramOne, paramTwo) => {};

// Two same functions
const sumNumbers = number => number + number; // accepts only single line
const sumNumbers = number => {
     const sum = number + number;
     return sum;
}
————————————————————————————————————————
Arrays

let shoppingList = [“fruit”, 4, [”salad”, ”beacon”, ”blurred”], true]; 
console.log(shoppingList[2][2])

//Array’in boyutu dışında kalan elemanlar ‘undefined’ olarak dönerler.

//Add item to last index
shoppingList[shoppingList.length] = “New Item”;
shoppingList.push(“Newer Item”, “Other Item”);

//Add Item Specific Index
//The splice() method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.
const insertIndex = 1;
const insertItem = ‘Hululu’;
shoppingList.splice(insertIndex, 0, insertItem);
shoppingList.splice(insertIndex, 2, insertItem); // belirtilen index’ten sonraki iki item’ı silip yerine yeni item’ı ekler

//Remove last item
shoppingList.pop();

//Remove first item and return removed item
shoppingList.shift();

//The unshift() method adds one or more elements to the beginning of an array and returns the new length of the array
console.log(shoppingList.unshift(4, 5));

//The slice() method returns a shallow copy of a portion of an array into a new array object selected from begin to end (end not included)
console.log(shoppingList.slice(1));
console.log(shoppingList.slice(1, 3));

//The map() method creates a new array with the results of calling a provided function on every element in the calling array.
const map1 = shoppingList.map(x => x + “ alınacak”);

//All Methods of Arrays
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join

————————————————————————————————————————
Variable Equalization

// All variables set by reference (pass-by-reference) so when one of the variable from equalisation changes the other one changes too

const famousSayings = ['Fortune favors the brave.', 'A joke is a very serious thing.', 'Where there is love there is life.'];
let listItem = famousSayings;
console.log(listItem);
listItem[2] = 'New Value';
console.log(listItem);
console.log(famousSayings);

https://developer.mozilla.org/tr/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

————————————————————————————————————————
Functions as Data

const myFunc = newFunc = (variable1) => {
 console.log(variable1);
} 
myFunc(‘aliVeli’);

// Fonksiyonun orijinal ismi için aşağıdaki çağrı kullanılır
console.log(myFunc.name);
————————————————————————————————————————
Function Inside Function

const someFunc = aotherFunc => { let i = 'a'; let j = aotherFunc; console.log(i+j); }
const otherFunc = (afunc, param)=> {let d = afunc(param); return d;}
const someOtherFunc = (param)=> param+'b'+'c';
someFunc(otherFunc(someOtherFunc,'veli'));

//Başka örnek
const addTwo = num => num + 2;
const checkConsistentOutput = (function1, param1) => {
  let firstTry = function1(param1);
  let secondTry = function1(param1);
  if(firstTry === secondTry) {
    return firstTry;
  } else {
    return 'This function returned inconsistent results';
  }
}

console.log(checkConsistentOutput(addTwo, 5))
————————————————————————————————————————
Array Functions

ForEach
const fruits = ['mango', 'papaya', 'pineapple', 'apple'];

// Separate Function Based usage
function printGrocery(element, index, allArray) {
  console.log(element+' - '+index);
}

fruits.forEach(printGrocery);

// Arrow Function based usage
fruits.forEach((fruit,index) => console.log(fruit+' - '+index));

 Map
// map fonksiyonunun foreach fonksiyonundan farkı modifiye edilmiş array’in kopyasını döner
const numbers = [1, 2, 3, 4, 5]; 

const bigNumbers = numbers.map(number => {
  return number * 10;
});
console.log(numbers); // Output: [1, 2, 3, 4, 5]
console.log(bigNumbers); // Output: [10, 20, 30, 40, 50]

// Alternatif kullanım
const divideBigNumbers = (number)=> {
  return number/100;
}
const smallNumbers = bigNumbers.map(divideBigNumbers);
console.log(smallNumbers); // Output: [1, 2, 3, 4, 5]
console.log(bigNumbers); // Output: [100, 200, 300, 400, 500]

 Filter
// Dizinin içinden belirli elemanları filtrelemeyi sağlar. map gibi yeni bir kopya array oluşturur.

const randomNumbers = [375, 200, 3.14, 7, 13, 852];

const smallNumbers = randomNumbers.filter(number => (
  number < 250
))
// alternatif yazım 
const smallNumbers = randomNumbers.filter(number => {
  return number < 250;
})

console.log(randomNumbers); // output : [ 375, 200, 3.14, 7, 13, 852 ]
console.log(smallNumbers); // output : [ 200, 3.14, 7, 13 ]

————————————————————————————————————————
Objects

let spaceship = {
  'Fuel Type' : 'Turbo Fuel',
  homePlanet : 'Earth',
  color: 'silver',
  'Secret Mission' : 'Discover life outside of Earth.'
};
console.log(spaceship['Fuel Type’]);
console.log(spaceship.homePlanet);

// Delete property from object
delete spaceship.color;

Objects Functions
let retreatMessage = 'We no longer wish to conquer your planet. It is full of dogs, which we do not care for.';

// To use “this” keyword in object function -> we must use old function definition
let alienShip = {
  name:'Alien23',
  retreat: function (name=this.name) {
    console.log(name+'\n'+retreatMessage);
  },
  takeOff() {
    console.log('Spim... Borp... Glix... Blastoff!');
  }
}

alienShip.retreat();
alienShip.retreat('Alien7');
alienShip.takeOff();

Loop Through Objects
for(let objField in alienShip) {
  console.log(`${objField} - ${alienShip[objField]}`);
}

Usage of “This” keyword in Objects
const robot = {
  model: '1E78V2',
  energyLevel: 100,
  provideInfo() {
    return `I am ${this.model} and my current energy level is ${this.energyLevel}`;
  }
};
console.log(robot.provideInfo());

// But we can’t use “this” in object arrow function
//Arrow functions inherently bind, or tie, an already defined this value to the function itself that is NOT the calling object.
const goat = {
  dietType: 'herbivore',
  diet: () => {
    console.log(this.dietType);
  }
}; // it will not work. “This” value return as “undefined”

Object Privacy
// One common convention is to place an underscore _ before the name of a property to mean that the property should not be altered.
const robot = {
  _model: '1E78V2',
  _energyLevel: 100,
}; // it is still changeable (mutable)

Getter
const robot = {
  _model: '1E78V2',
  _energyLevel: 100,
  get energyLevel() {
    if(typeof this._energyLevel === 'number') {
      return `My current energy level is ${this._energyLevel}`;
    } else {
      return 'System malfunction: cannot retrieve energy level'
    }
  }
};

console.log(robot.energyLevel);
robot._energyLevel = 'aliveli';
console.log(robot.energyLevel);

Setter
const robot = {
  _model: '1E78V2',
  _energyLevel: 100,
  _numOfSensors: 15,
  get numOfSensors(){
    if(typeof this._numOfSensors === 'number'){
      return this._numOfSensors;
    } else {
      return 'Sensors are currently down.'
    }
  },
  set numOfSensors(num) {
    if(typeof num === 'number' && num >= 0) {
  		this._numOfSensors=num;
       } else {
         console.log('Pass in a number that is greater than or equal to 0');
       }
  }
};

robot.numOfSensors = 100;
console.log(robot.numOfSensors);
robot.numOfSensors = 'asd';
console.log(robot.numOfSensors);

Factory
//uses to create objects with exact properties, like factories
const robotFactory = (model, mobile) => {
  return {
    model: model,
    mobile: mobile,
    beep: function(){
      console.log(`${this.model} Beep Boop`);
    }
  }
};

const tinCan = robotFactory('P-500', true);
tinCan.beep()

Property Value Shorthand
// Create factory with less code
const robotFactory = (model, mobile) => {
  return {
    model,     // here is the trick
    mobile,
    beep: function(){
      console.log(`${this.model} Beep Boop`);
    }
  }
};

Destructured Assignment
const robot = {
  model: '1E78V2',
  energyLevel: 100,
  design: {
      color: ‘blue’
  }
};

const {model} = robot;   // destructured assignment
const {color} = robot.design;

Object Assign   
const newRobot = Object.assign( {laserBlaster: true, voiceRecognition: true}, robot)

console.log(robot); // robot object doesn’t change
console.log(newRobot);  // newRobot = robot + new properties

————————————————————————————————————————
Object Pass By Reference

Let spaceship = {
  homePlanet : 'Earth',
  color : 'silver'
};

// We can change object fields by passing object as parameter
let paintIt = obj => {
  obj.color = 'glorious gold'
};

paintIt(spaceship);
console.log(spaceship.color) // prints ‘glorious gold’

// We can’t change whole object (reassign) by passing object as parameter
let tryReassignment = obj => {
  obj = {
    identified : false, 
    'transport type' : 'flying'
  }
  console.log(obj); // Prints {'identified': false, 'transport type': 'flying'}
};

tryReassignment(spaceship); // The attempt at reassignment does not work.
console.log(spaceship); // Still returns {homePlanet : 'Earth', color : 'red'};

————————————————————————————————————————
Inheritance

class HospitalEmployee {
  constructor(name) {
    this._name = name;
    this._remainingVacationDays = 20;
  }
  
  get name() {
    return this._name;
  }
  
  get remainingVacationDays() {
    return this._remainingVacationDays;
  }
  
  takeVacationDays(daysOff) {
    this._remainingVacationDays -= daysOff;
  }
}

class Nurse extends HospitalEmployee {
  constructor(name, certifications) {
    super(name);
    this._certifications = certifications;
  }
}

const nurseOlynyk = new Nurse('Olynyk', ['Trauma', 'Pediatrics']);

console.log(nurseOlynyk);  
// OUTPUT : Nurse {
  _name: 'Olynyk',
  _remainingVacationDays: 20,
  _certifications: [ 'Trauma', 'Pediatrics' ] }

// We cannot call static methods on an instance.

————————————————————————————————————————
Özel Gösterim
return (
    <View style={styles.container}>
        {
            currencyCode === 'TRY' || 'EUR' ?
                <>
                    <Text style={styles.priceTextStyle}>{price}</Text>
                    <Text style={styles.currencyTextStyle}>{currencyCode}</Text>
                </> : currencyCode === 'USD' || 'CHF' &&
                <>
                    <Text style={styles.currencyTextStyle}>{currencyCode}</Text>
                    <Text style={styles.priceTextStyle}>{price}</Text>
                </>
        }

    </View>
)
```

