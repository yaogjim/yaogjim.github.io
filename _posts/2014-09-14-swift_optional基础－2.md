---
layout: post
---
swift Optional基础2
## 1、不确定是否nil的都作Optional
在swift中，无论变量、常量还是类是属性，如果不是Optional的话，都必须指定初始值（默认值），且不能为nil；另外，除非是Optional，否则无法**再**被设置为nil。

	class Person {
	  var image : UIImage?
	  var firstName = String()
	  var lastName = String() 
	}
	
如上，我们定义一个Person类，在创建对象时提供firstName和lastName，而image可以保持空，在用户保存头像以后再设置。<br>
1.如果var image : UIImage后不加？，则必须提供初始值，否则xcode报Persion无法正常init，因为在init中所有非optional属性必须有值。
2.同时如果不加？，image属性也不能被设置为nil，必须在创建person对象时提供图片，这显然有违逻辑。

## 2、从objc库中返回的对象一定要check nil
因为objc中object可以为nil，所以在swift中调用objc库返回的对象都是Optional。如果在swift中忘记了nil－check，很不幸，你会得到一个大大的runtime error。

	let formatter = NSDateFormatter()
	var now:NSDate=NSDate()
	now = formatter.dateFromString("not_valid")
第3行会报value of Optional type unwarpped,即formatter.dateFromString("not_valid")返回的是Optional，赋给now前必须先unwrape.

	let formatter = NSDateFormatter()
	let now = formatter.dateFromString(“not_valid”)
	let soon = now.dateByAddingTimeInterval(5.0) 
还是第三行，报错：“NSDate？”没有dateByAddingTimeInterval方法。为什么？应该这时的now是个optional，没有被隐性的wrapped。

	let formatter = NSDateFormatter()
	let now = formatter.dateFromString("not_valid")
	let soon = now?.dateByAddingTimeInterval(5.0)
则编译通过。

## 3、enum类型 Optional
之前说过Optional实际上就是一个enum类型，查看optional的头文件可以看到

	enum Optional<T> : Reflectable, NilLiteralConvertible {
	    case None
	    case Some(T)
	    init()
	    init(_ some: T)
	
	    /// Haskell's fmap, which was mis-named
	    func map<U>(f: (T) -> U) -> U?
	    func getMirror() -> MirrorType
	    static func convertFromNilLiteral() -> T?
	}
对定义一个Optional，

	let optionalString:String?
和

	let optionalString:Optional<String>
是一样的。

所以当我们在swift中操作Optional时，相当于我们在操作一个可能为nil也可能为某个值的容器；我们使用！从Optional中强制unwrap一个nil时，相当于从这个容器中取出本来就不存在的值，于是出现：**fatal error: Can’t unwrap Optional.None**也就不奇怪了。