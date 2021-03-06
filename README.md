


Typescript Summary
==================

First question goes down to is whats Typescript (TS) ?

Introduced by Microsoft, TS is basically an extension of Javascript. Its whole purpose is to let JS programmers write better and managed code. 

By managed code, what I mean is - since JS is not a compiled lanaguage like C#/Java, programmers dont get to know about the errors in their code at the compile time. Also in JS, you can change the data type of your variable anytime - this makes JS an open ground to make mistakes. 

TS does the work of compiling (transpiling) the code and providing a better code writing environment to the programmer. 

*Writing environment*? Yes TS code will eventually be transpiled to JS when you want to run it on a browser.


 Look at the following example:

![Writing environment](http://i.imgur.com/M5LNx8v.png)

The left is the TS code, and the right side is the transpiled JS code.

You will see that we have made an abstract class Encryption and a normal class NormalEncrytion in the example.

There is no difference in the generated JS code for the two classes. You will notice that there is not restriction made for keeping the class abstact in the transpiled JS.
But the TS compiler will not let you instantiate the abstract class Encryption as abstract classes are meant to be (see the error near the red underline).

So the point I am trying to make is - Many of the features will not be available in the generated JS code; but still the TS compiler will let you *write better and error free code*.

So advantages of TS:

  - **Type checking** - Gives you ability to define type for a variable and check for errors
  - **Syntatic sugar** - Just define a class by using TS' keyword. The compiler will take care of everything.
  - **OOPS language goodies** - TS brings the goodness of Interfaces, Abstract class, Generics, etc to JS

----------


### Basic Types
TS has these eight basic data types -

![Types](https://s18.postimg.org/wl3etj589/tstypes.png)

 1- **Number** - ```public amount : number = 123;```

2- **String** - ```let color: string = "blue";```

Strings also support template syntax -
```
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.
```

 3- **Boolean** - ```let isDone: boolean = false;```

 4- **Array** -

 ``` let list: number[] = [1, 2, 3];```

```
// Array can be defined like this
let list: Array<number> = [1, 2, 3];
```

5- **Tuple** - *Basically an array which can have different data types*
```
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```
6- **Enum** - *A way to assign numeric values to a set of values*
``` enum Status { Processing, Accepted, Rejected} ```

7- **Any** - *This type works exactly like the normal variables in JS. TS compiler doesnt perform its type checking on these ones.*

```
let notSure: any = 4;
notSure = "maybe a string instead";
```
8- **Void** -
For functions, it tells that the function doesnt return anything.
```
function warnUser(): void {
    alert("This is my warning message");
}
```

For variables, a void type variable can only be null or undefined.

``` let unusable: void = undefined; ```

### Union of Types

There are situations in which a function can accept a parameter which can be of different type of data type.
Using Union, you can set a restriction for a function to accept a parameter of given range of data types.

```
function alertGender(gender: boolean | string){
 if (typeof gender === "boolean") {
   alert("You are " + (gender ? "male" : "female" ));
    }
    if (typeof gender === "string") {
   alert("You are " + gender);
    }
}
// the parameter gender could have been set as any.
// But that leaves scope for error, as in you wont be able
// to get the strict type checking of TS

```

----------


----------


Interfaces
-------------

Interfaces are like a contract. A class/function which implements an interface should have the properties defined in the interface.

```
interface  IFlyable{
    heightLimit : number;
    numberEngines? : number; //optional property
    fly() : void;
}

class Vehicle implements IFlyable{
    public name: string;
    public heightLimit : number;
    fly() : void{};
}
// Vehicle needs to have properties specified in the IFlyable
// Compiler will throw an error if the implementing class doesnt have all the properties
// Properties suffixed with ? are optional
```

### Interface for functions

```
interface SearchFunc {
    (source: string, subString: string): boolean;
}

// This is an interface for function that do search operation.
// It sets a guideline for functions to take two string inputs and return a bool


let Search : SearchFunc;

Search = function(a,b):boolean { // no need to reiterate that a and b are string
    return true;
}
```

### Using Interface to restrict input parameter to a function

```
interface Square{
    side : number;
}


function calculateArea(sq : Square){
   return (side * side);

    // The input parameter sq implements the Interface Square
    // This puts a condition that the input parameter
    // needs to have the property side
}


calculateArea({side:20}); // valid
calculateArea({side:'20'}); // error - This is one common problem when working on JS

calculateArea({side:1, color: 'red' }) // error - This is because of extra property
// TS treats this as error

// If your object will have extra properties, then pass like this -
calculateArea({ side: 1, color: 'red' } as Square);


```

> **Note:**

> - Properties suffixed with ? are optional to implement in the class.
> - Interface can inherit from other Interfaces, Classes


----------


----------


Classes
-------------

TS classes have public, private and protected members. Members are public by default.
Classes can also be defined as Abstract classes and they can have Static properties.
Derived classes need to call the ```super``` function to execute the constructor of the base class.

#### Key points ####

**Abstract classes** - These are meant to be inherited, but cannot be instantiated.
**Static properties** - These properties describe the class not the instances of it. See the property population in the Class kitchensink code.


### TS Class Kitchensink code ###

Lets see the example below:

We want to model classes for Employees.

Now every employee will have a gender, name, etc. These properties are bound to be there in every human. **As a thumb rule, keep the inherent properties in the abstract class**

Now properties like Gender, Name are public in nature.
Properties which are private to oneself like Salary should be kept as private so that they are not accessible publically.




```
// Put all inherent properties in the abstract clss

abstract class Employee{

    static headCount : number; // make this property static to count the population. The property population doesnt belong to the instance, it rather belongs to the class itself

    public name: string; // default public
    gender : number;

    putAttendance() : void{
        console.log("punch in card");
        // this function can be put into abstract clss because every employee will put attendance
    }

    abstract work() : void;
    // every employee has their own role and work - so its better to make it abstract and not put any implementation fr ths function

    private salary : number; // private because its private to the employee and it should not be accessible outside
}


class DeveloperEmployee extends Employee{

    constructor(company : string){
        Employee.headCount++;
        super();
    }

    work(){
        console.log('develop IT systems'); // had to implement this function
    }

}

class ManagerEmployee extends Employee{

    constructor(company : string){
        Employee.headCount++;
        super();
    }

    work(){
        console.log('managing other employees'); // had to implement this function
    }

}

Employee.headCount = 0;
let dev = new DeveloperEmployee('EY');
let manager = new ManagerEmployee('EY');

console.log(Employee.headCount);
```


----------


----------


Functions
-------------

TS functions check the input parameters and give error on compile time if there is any mismatch.

There are also Default and Optional parameters for functions.

```
function buildName(firstName: string, lastName = "Smith", middleName? : string) {
}

```


###Rest Parameters

Rest parameters are treated as a boundless number of optional parameters. When passing arguments for a rest parameter, you can use as many as you want; you can even pass none. The compiler will build an array of the arguments passed in with the name given after the ellipsis (...), allowing you to use it in your function.

```

function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;

```



# Namespaces

Namespaces are used to avoid naming collisions in the global scope. There can be situations where your code uses libraries LibraryX and LibraryY. It is possible that these libraries have classes with same name. It can make the code really messy.

Thats why its a good practice to structurize the code using Namespaces.

A Namespace can ```export``` only the properties which are required, and can keep the inner implementations within itself.

A namespace can be splitted across different files.

```
namespace Encryption {

const bit_mode = 64;

    export class AES {
        encrypt(s: string) {
            return s;
        }
        decrypt(s: string) {
            return s;
        }
    }

    export class DES {
      encrypt(s: string) {
            return s;
        }
        decrypt(s: string) {
            return s;
        }
    }
}

```

# Decorators

Decorators are a way to perform extra activities like logging, manipulating , restricting whenever a class is declared, or a function is run

For example -

```
@sealed
class ABC {}

function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

// the function sealed will run when the class is declared, and seal the class' constructor and prototype

```

More on Decorators coming soon....

