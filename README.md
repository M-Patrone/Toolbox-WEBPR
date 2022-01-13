## WEBPR Toolbox Manuel Patrone
- [WEBPR Toolbox Manuel Patrone](#webpr-toolbox-manuel-patrone)
  - [Variabeln Deklaration](#variabeln-deklaration)
  - [Funktionen](#funktionen)
  - [Lambda-Kalkül](#lambda-kalkül)
    - [Id Funktion](#id-funktion)
    - [Alpha Translation](#alpha-translation)
    - [Beta Reduktion](#beta-reduktion)
    - [Eta Reduktion](#eta-reduktion)
  - [map-Methode](#map-methode)
  - [filter-Methode](#filter-methode)
  - [Reduce-Methode](#reduce-methode)
  - [Splice-Methode](#splice-methode)
  - [Pair, Product, Type](#pair-product-type)
  - [Eval](#eval)
  - [Klassen und Objekte](#klassen-und-objekte)
  - [Prototype](#prototype)
  - [Vorgehen beim Programmieren](#vorgehen-beim-programmieren)
  - [Model](#model)
  - [View](#view)
  - [Controller](#controller)
  - [Observable](#observable)
  - [Async](#async)
  - [Promise](#promise)
  - [Module](#module)
  - [Callback](#callback)

> “JavaScript: Don’t judge me by my bad parts, learn the good stuff and stick with that!”
> 
> <cite>― Eric Freeman, Head First JavaScript Programming</cite>
   
### Variabeln Deklaration 
- `let` deklariert eine Variable im Gültigkeitsbereich des lokalen Blocks
- `var` deklariert eine Variable im aktuellen Scope
- `test = '';` deklariert eine Variabel im globalen Scope
- `const` erstellt eine Konstante

``` javascript
function fVariable() {
  var varVariable = "var";
  let letVariable = "let";

  console.log(varVariable, letVariable); // var let

  {
    var varVariable2 = "var_2"
    let letVariable2 = "let_2";
    console.log(varVariable2, letVariable2); // var_2 let_2
  }

  console.log(varVariable2); // var_2
  console.log(letVariable2); // ReferenceError
}
```

### Funktionen
Funktionen sind ein wichtiger Baustein in JavaScript. Funktionen können in Javascript folgendermassen ausgedrückt werden.

```  javascript
function square(number) {
  return number * number;
}
```
Funktionen können jedoch auch in Variabeln gespeichert werden:
``` javascript
const square = function(number) { return number * number }
var x = square(4)
```
Funktionen könne auch als Lambda Funktionen implementiert werden:
``` javascript
const square = a => a*a;

//aufruf
square (4)
```
### Lambda-Kalkül
Hilfreiches Video: [![Lambda Calculus](http://img.youtube.com/vi/3VQ382QG-y4/0.jpg)](http://www.youtube.com/watch?v=3VQ382QG-y4 "Lambda Calculus")

#### Id Funktion
``` javascript
const id = x => x;
```

#### Alpha Translation
``` javascript
const id = x => x
const id = y => y
```
#### Beta Reduktion
``` javascript
( f => x => f(x) ) (id) (1)
(	   x => id(x))		(1)
(			id(1))
(x => x) (1)
1
```
#### Eta Reduktion
``` javascript
x => y => plus (x) (y)
x => 	  plus (x)
		  plus
```
### map-Methode
In dieser Woche haben wir die map() Funktion kennengelernt. Die map() Methode wendet auf jedes Array Element die bereitgestellte Funktion an. Das Resultat wird in einem neuen Array zurückgeben.
``` javascript
const plus = a => b => a + b;

const twoPlus = plus(2);

[1, 2, 3].map(x => plus(2)(x)); //output: [3, 4, 5]
[1, 2, 3].map(plus(2)); //output: [3, 4, 5]
[1, 2, 3].map(twoPlus); //output: [3, 4, 5]
```
### filter-Methode
Mit der Filter()-Methode kann ein Array gefilter werden. Die Methode wendet auf jedes Array Element die bereitgestellte Funktion an und gibt nur die Elemente zurück, welche den Test besteht. 
``` javascript 
const words = ['Anna','Mike','Annabelle','Kim','Nico','Alexander'];

words.filter(w => w.charAt(0)==='A'); //output ['Anna', 'Annabelle', 'Alexander']
```
### Reduce-Methode
Die reduce()-Methode reduziert ein Array auf einen einzigen Wert, indem es jeweils zwei Elemente (von links nach rechts) durch eine gegebene Funktion reduziert. Am besten baut man die Reduce-Methode nach folgendem Schema auf:
![Reduce Aufbau](/img/Reduce.png "Reduce Aufbau")
``` javascript 
const multiple = (previousValue, currentValue) => previousValue * currentValue;

[1, 2, 3].reduce((accu, cur) => accu * cur); //output 6
[1, 2, 3].reduce(multiple); //output 6

//man kann zudem einen Startwert angeben
[1, 2, 3].reduce(multiple, 100); //output 600
```
### Splice-Methode
Mit der Splice()-Methode kann das Array modifizieren. Man kann damit Elemente löschen oder einfügen.
```javascript
let names = ["Thomas","Manuel", "Nico"];
names.splice(1,0, "Antonia","Jennifer");
console.log(names); // ['Thomas', 'Antonia', 'Jennifer', 'Manuel', 'Nico']

names.splice(1,2);
console.log(names); // ['Thomas', 'Manuel', 'Nico']
```
### Pair, Product, Type
``` javascript
const pair = x => y => f => f(x)(y);
```
Die pair Variable nimmt eine erste Funktion `x` entgegen und gibt eine verschachtelte Funktion zurück, die `y` annimmt. Diese wiederum gibt eine verschachtelte Funktion zurück, die eine Funktion `f` als Argument hat.
``` javascript
const selectFirst = f => s => f;
const pair = x => y => f => f(x)(y);

const examplePair = pair(1)(2);

examplePair(selectFirst); //output 1
```
In dieser Woche ging es um Scripting:
### Eval
Die Funktion eval() wertet JavaScript-Code aus, der als String dargestellt ist.
``` javascript
eval('console.log("Hello World!")'); //output in der Konsole Hello World!
```
**Warnung: Ausführen von Javascript Code via eval() könnte zu grossen Sicherheitsrisiko werden!**

Gute Playlist über Javascript Kontext: https://www.youtube.com/watch?v=su-SdgebJCE&list=PLndbWGuLoHea6b3g3fY77U47RiryT2Sr5&index=1

### Klassen und Objekte
Es gibt mehrere Variante zur Erstellung von Javascript Objekten:
``` javascript
const person = {
  firstName: "John",
  eyeColor: "blue"
  fullPersonText : function() {
    return this.firstName + " " + this.lastName+" "+" Augenfarbe: "+ this.eyeColor;
  }
};
```
- Vorteil: super dynamisch
- Nachteil: keine Sicherheit

**In einer Funktionsdefinition bezieht sich `this` auf den "Eigentümer" der Funktion.**

Man kann jedoch auch mithilfe einer Funktion ein Objekt machen:
``` javascript
function car (type){
  return {
    getCarTypeText: function(){
      return "Die Marke ist: "+type;
    }
  }
};
```
Eine weitere Möglichkeit wäre die folgende:
``` javascript
class Car {
  constructor(type) {
    this.carType = type;
  }
}

const carObject = new Car('SUV');
```
Guter Artikel über `new`: https://www.tutorialsteacher.com/javascript/new-keyword-in-javascript

**Wichtig:** Bei dieser Methode benötigt es immer die Funktion `constructor`. 

```
"A JavaScript class is not an object.

It is a template for JavaScript objects."
```
Die Konstruktormethode ist eine besondere Methode:
- Sie muss den genauen Namen "constructor" haben.
- Sie wird automatisch ausgeführt, wenn ein neues Objekt erstellt wird.
- Wenn Sie keine Konstruktormethode definieren, fügt JavaScript eine leere Konstruktormethode hinzu.

### Prototype
Javascript wird oft als Prototyp-basierte Sprache beschrieben. Der Prototyp ist ein Objekt, das in JavaScript standardmäßig mit allen Funktionen und Objekten verbunden ist, wobei die Prototyp-Eigenschaft der Funktion zugänglich und änderbar ist und die Prototyp-Eigenschaft des Objekts nicht sichtbar ist. Vergleichbar ist dies mit "Object" von Java.
``` javascript
function Car() {
    this.type = 'SUV';
}

Car.prototype.age = 15;
let carObject = new Car();
console.log(carObject.age); // 15
```
### Vorgehen beim Programmieren
1. Schreiben der Tests
2. Beispiele aufschreiben (Was ist das Resultat?)
3. Verschiedene Vorgehensweisen
   1. Erkundne (**Wichtig:** Zeitlimite setzen!)
   2. Statisch anfangen
   3. Statische Elemente ersetzen (z.B. durch Variabeln)
   4. Zusammenfassen (z.B. in einer Funktion) und Refactor
   5. Release
   6. Was kann ich das nächste mal besser machen?

### Model
Dieser Teil ist für die Geschäftslogik zuständig und verwaltet den Zustand von Objekten.
### View
Ist für die Darstellung der Daten des Modells. 
### Controller
Der Controller steht zwischen Model und View und ist grob gesagt für die Datenmanipulation zuständig.
### Observable
Ein Observable hat mehrere Abonennten, wenn sich der Wert ändert, werden die Abonennten "benachrichtigt". Es wird dann eine bestimmte Funktion ausgeführt, die vorher definiert wurde (Callback).
``` javascript
const Observable = (value) => {
  const listeners = [];
  return {
    onChange: (callback) => listeners.push(callback),
    getValue: () => value,
    setValue: (val) => {
      if (value === val) return;
      value = val;
      listeners.forEach((notify) => notify(val));
    },
  };
};
``` 
Quelle: König Dierk
### Async
Await ist im Grunde ein "Zückerli" für Promises. Es lässt Ihren asynchronen Code eher wie synchronen/prozeduralen Code aussehen:
``` javascript
function returnProm() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 5000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await returnProm();
  console.log(result);
}

asyncCall();

```
### Promise
Ein `Promise` wird verwendet, um das asynchrone Ergebnis einer Operation zu verarbeiten. JavaScript ist so konzipiert, dass es nicht auf die vollständige Ausführung eines asynchronen Codeblocks wartet, bevor andere synchrone Teile des Codes ausgeführt werden können. Mit Promises können wir die Ausführung eines Codeblocks aufschieben, bis eine asynchrone Anfrage abgeschlossen ist. Auf diese Weise können andere Vorgänge ohne Unterbrechung weiterlaufen.
``` javascript
const ownPromise = new Promise((resolve, reject) => { 
    if(true) {
         setTimeout(function(){
             resolve("Promise is resolved!"); // fulfilled
        }, 300);
    } else {    
        reject('rejected!');  
    }
});

ownPromise
.then((successMsg) => {
    console.log(successMsg);
})
.catch((errorMsg) => { 
    console.error(errorMsg);
});
```
### Module
Früher wurden die einzelnen Codeblöcke einfach in separate Files hineingeschrieben und dann einzeln im HTML geladen:
``` HTML
<script src="essentials.js"></script>
<script src="index.js"></script>
```
Wieso sollte man Module verwenden:
- Organisation des Codes
- Dependencies
- Error Vermeidung
- **Wichtig:** Module werden asynchron geladen

``` javascript
//befindet sich im File essentials.js
export let findModulus = (a, b) => {return a % b;}

//import im index.js file
import {findModulus} from './essentials.js';
```
### Callback
Ein Callback ist eine Funktion, welche eine Funktion als Parameter übergeben wurde. Ein geeigneter Einsatzzweck von einem Callback wäre, bei einer asynchroner Funktion, bei denen eine Funktion auf eine andere Funktion warten muss (z. B. auf das Laden einer Datei).
``` javascript
 function showResult(res) {
  document.writeln(res);
}

function sum(num1, num2, callback) {
  let sum = num1 + num2;
  callback(sum);
}

sum(100,200, showResult);
```

