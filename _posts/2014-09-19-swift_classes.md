---
layout: post
---
Swift Classes总结
这是一份weloveswift上关于swift class的笔记。
如果你也像我一样只关注要点，在需要的时候才去看细节，那么下面这段小节肯定适合你。

* Creating a class
使用class创建类，如果属性非Optional，必须先赋予初始值，或则在在init中初始化，否则报错。
如果属性值可能为空，必须设置为Optionl。关于OPtional的使用，见本博[Optional使用](http://www.salehao.com/swift-optionalji-chu-1/)。
* Initializers
用户初始化class struct enum等
必须在初始化当前class属性后，才能调用supper class等初始化方法
初始化方法不返回值
如果初始化不能确保所有属性都有值，则会报错
* Creating custom initializers
除默认的初始化方法，可以添加需要任何初始化方法，可以在初始化方法中提供参数默认的值
* Testing for identity
class instant的比较，使用＝＝＝／！＝＝。
对class的实例是对class 对引用，不同于struct enum等是对值的copy，所以对实例的比较，实际上比较引用对象是否一个同一个对象。
* Inheritance
继承，可以借用父类所有可用的东西
* Overriding
重写父类的方法，使用override关键字
可以通过super调用父类的方法
* Base class
没有从任何class继承的class，可以说是一个干净的swift类
运行速度会比从NSObject中继承的类快很多，但会损失很对Oc运行时的特性，详见下面stackoverflow中说明
* Protocols
定义可以被代理给其他class的功能，
和方法定义类似，但不能在参数中设置默认值
没有声明@optional的方法，都是required。


下面是笔记部分。
## Creating a class
To create a new class use the class keyword followed by the class name and a pair of braces. 

	class Point {
	    var x = 0.0 // sets the default value of x to 0
	    var y = 0.0 // sets the default value of x to 0
	}
	
	// this creates a new Point instance using the default initializer
	var point = Point()
	
	point.x = 100 // sets the x property to 100
	point.y = 200 // sets the y propery to 200

## Initializers

> Initialization is the process of preparing an instance of a class, structure, or enumeration for use. This process involves setting an initial value for each stored property on that instance and performing any other setup or initialization that is required before the new instance is ready to for use.
> 
> You implement this initialization process by defining initializers, which are like special methods that can be called to create a new instance of a particular type. Unlike Objective-C initializers, Swift initializers do not return a value. Their primary role is to ensure that new instances of a type are correctly initialized before they are used for the first time.

if you do not set the default values of x and y in the above example the compiler will get 3 errors:

	class Point { // Class 'Point' has no initializers
	    var x // Type annotation missing in pattern
	    var y // Type annotation missing in pattern
	}
	
To create an initializer use the init keyword followed by 0 or more parameters needed for initialization.

	class Point { 
	    var x: Float
	    var y: Float
	
	    init(x: Float, y: Float) {
	        self.x = x
	        self.y = y
	    }
	}
	// Example usage
	var point = Point(x: 100, y: 200)
	
To set an optional type just add ? after the desired type. 
If a property is declared with an optional type it will get a default value of nil.

	class Point { 
	    var x: Float?
	    var y: Float?
	}
## Creating custom initializers
    // no bio provided
    init(firstName: String, lastName: String) {
        self.firstName = firstName
        self.lastName = lastName
    }

    // bio provided
    init(firstName: String, lastName: String, bio: String) {
        self.firstName = firstName
        self.lastName = lastName
        self.bio = bio
    }
implicit value to the bio parameter of the initializer

	init(firstName: String, lastName: String, bio: String = "I ♡ Swift!") {
        self.firstName = firstName
        self.lastName = lastName
        self.bio = bio
    }
   
##  Testing for identity
> Because classes are reference types, it is possible for multiple constants and variables to refer to the same single instance of a class behind the scenes. (The same is not true for structures and enumerations, because they are value types and are always copied when they are assigned to a constant or variable, or passed to a function.)
> 
> It can sometimes be useful to find out if two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:
> 
> Identical to (===)
> Not identical to (!==)

	for user in users {
	    if user === host {
	        // host logic here
	        println("this is the host")
	    } else {
	        // guest logic here
	        println("this is a guest")
	    }
	}
	
	
## Inheritance
A class can inherit methods, properties, and other characteristics from another class.

	class AClass {
	    func doSomething() {
	        println("Hello from AClass")
	    }
	}
	class Subclass: AClass  {
	}
	
## Overriding
To override a method write the override keyword before the method declaration:

	class AClass {
	    func doSomething() {
	        println("Hello from AClass")
	    }
	}
	
	class Subclass: AClass  {
	    override func doSomething() {
	        println("Hello from Subclass")
	    }
	}	
Use the **super** keyword to call any method from the superclass.
 
	 class Subclass: AClass  {
	    override func doSomething() {
	        super.doSomething()
	
	        println("Hello from Subclass")
	    }
	}
## Base class
A class that does not inherit from another class is called a base class.

	class User {
	    var name: String!
	    var age: Int!
	
	    init(name: String, age: Int) {
	        self.name = name
	        self.age = age
	    }
	}
**The iOS and Mac OS classes ussualy inherit from NSObject, either directly on indirectly. If you have a mixt codebase I would encourage you to subclass NSObject when creating a new class:
**
> Swift classes that are subclasses of NSObject:
> 
> * are Objective-C classes themselves
> * use objc_msgSend() for calls to (most of) their methods
> * provide Objective-C runtime metadata for (most of) their method implementations
> 
> 
> 
> Swift classes that are not subclasses of NSObject:
> 
> * are Objective-C classes, but implement only a handful of methods for NSObject compatibility
> * do not use objc_msgSend() for calls to their methods (by default)
> * do not provide Objective-C runtime metadata for their method implementations (by default)
> * 
> Subclassing NSObject in Swift gets you Objective-C runtime flexibility but also Objective-C performance. Avoiding NSObject can improve performance if you don’t need Objective-C’s flexibility.
> 
> from stackoverflow Swift native base class or NSObject

## Protocols
Protocols describe methods, properties and other requirements that are needed for a specific task.
	
	protocol MyFirstProtocol {
    // I do nothing
  
Mark a method as optional using the @optional keyword. 
Without @optional keyword that method is required. 
The swift compiler will throw an error if a class conforms to a protocol and does not implement the required methods.

	class AnotherSwiftClass: MyFirstProtocol, AnotherProtocol {
	    ...
	}

**If the class inherits from another one, make sure to put the superclass name before the protocol list.**

	class AnotherSwiftClass: AClass, MyFirstProtocol, AnotherProtocol {
	    ...
	}
**Note: Protocols use the same syntax as normal methods, but are* not allowed *to specify default values for method parameters.**

One of the most overused design pattern in iOS is delegation. 
**A class can delegate a part of it’s responsibilities to an instance of another class.This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated.**

Example, the Player class delegates the shooting logic to the weapon:

	protocol Targetable {
	    var life: Int { get set }
	    func takeDamage(damage: Int)
	}
	
	protocol Shootable {
	    func shoot(target: Targetable)
	}
	
	class Pistol: Shootable {
	    func shoot(target: Targetable) {
	        target.takeDamage(1)
	    }
	}
	
	class Shotgun: Shootable {
	    func shoot(target: Targetable) {
	        target.takeDamage(5)
	    }
	}
	
	class Enemy: Targetable {
	    var life: Int = 10
	
	    func takeDamage(damage: Int) {
	        life -= damage
	        println("enemy lost \(damage) hit points")
	
	        if life <= 0 {
	            println("enemy is dead now")
	        }
	    }
	}
	
	class Player {
	    var weapon: Shootable!
	
	    init(weapon: Shootable) {
	        self.weapon = weapon
	    }
	
	    func shoot(target: Targetable) {
	        weapon.shoot(target)
	    }
	}
	
	var terminator = Player(weapon: Pistol())
	
	var enemy = Enemy()
	
	terminator.shoot(enemy)
	//> enemy lost 1 hit points
	terminator.shoot(enemy)
	//> enemy lost 1 hit points
	terminator.shoot(enemy)
	//> enemy lost 1 hit points
	terminator.shoot(enemy)
	//> enemy lost 1 hit points
	terminator.shoot(enemy)
	//> enemy lost 1 hit points
	
	// changing weapon because the pistol is inefficient
	terminator.weapon = Shotgun()
	
	terminator.shoot(enemy)
	//> enemy lost 5 hit points
	//> enemy is dead now

