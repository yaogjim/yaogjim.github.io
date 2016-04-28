---
layout: post
---
swift optional基础－1
## 概览
在swift中声明变量时，默认是非optional的，也就是定义时必须赋予非nil的值，否则就会出现编译时错误。
	比如

	var message: String = "Swift is awesome!" // OK
	message = nil // compile-time error
	
如果你无法在定义变量时确定该变量是否可空（nil），你必须使用optional，来告诉swift编译器，这个变量可能为nil，之后你可以为这个变量赋值，也可以设置为nil。

	class Messenger {
	    var message1: String = "Swift is awesome!" // OK
	    var message2: String? // OK
	}
而在objc中定义变量时可以设置初始值也可以不设置，之后可以继续随意修改，比如；

	NSString *message = @"Objective-C will never die!";
	message = nil;	
-

	class Messenger {
	    NSString *message1 = @"Objective will never die!";
	    NSString *message2;
	}
	
## 为什么要使用optional
在objc中直接对nil对象操作不会有编译错误，而出现运行时错误。比如：

	NSString *stockCode = [self findStockCode:@"Facebook"]; // nil is returned
	NSString *text = @"Stock Code - ";
	NSString *message = [text stringByAppendingString:stockCode]; // runtime error
	NSLog(@"%@", message);
如果stockCode为nil，那么就出现运行时错误。
而在swift中，如果对optional操作（即可能为nil的对象）直接报编译错误。如：

	var stockCode:String? = findStockCode("Facebook")
	let text = "Stock Code - "
	let message = text + stockCode  // compile-time error
	println(message)
这里swift强制了nil检查，提高了代码的质量。
## Unwrapping optional
swift中的optional本质上一个enum类型，所以如果直接使用optional，就是出现编译错误。<br>
**“value of optional type String? is not unwrapped“**<br>
使用前必须先使用！来unwrap，将值从optional中解出来；而unwrap之前，必须先判断该optional是否为nil，对可能为nil对optional做！操作会导致 运行时错误。
比如

	var stockCode:String? = findStockCode("Facebook")
	let text = "Stock Code - "
	let message = text + stockCode!  // runtime error
如果stockCode为nil的话，会产生运行时错误，**fatal error: Can’t unwrap Optional.None**，但能通过编译。
## optional 绑定
相对于强制Unwrapping optional，使用optional绑定是一种更简单更推荐的Unwrapping方法。

	var stockCode:String? = findStockCode("Facebook")
	let text = "Stock Code - "
	if let tempStockCode = stockCode {
	    let message = text + tempStockCode
	    println(message)
	}
直接将optional值在if语句赋给常量，如果不为nil则if语句为真。
上面的语句可以简化为：（因为findStockCode返回的是一个optional）

	let text = "Stock Code - "
	if let tempStockCode = findStockCode("Facebook") {
	    let message = text + tempStockCode
	    println(message)
	}
## optional 链
optional链,通过?.将多个optional操作链接在一起。
比如

	class Stock {
	    var code: String? 
	    var price: Double? 
	}
	
	func findStockCode(company: String) -> Stock? {
	    if (company == "Apple") {
	        let aapl: Stock = Stock()
	        aapl.code = "AAPL"
	        aapl.price = 90.32
	
	        return aapl
	            
	    } 	        
	    return nil
	}
findStockCode返回一个optional，stock对象的两个属性也是optional。

	if let stock = findStockCode("Apple") {
	    if let sharePrice = stock.price {
	        let totalCost = sharePrice * 100
	        println(totalCost)
	    }
	}
使用optional链，可以将上述代码简化

	if let sharePrice=findStockCode("Apple")?.price{
		let totalCost = sharePrice * 100
	        println(totalCost)
	}