# Constant Singleton by Swift
[Singleton pattern](http://www.galloway.me.uk/tutorials/singleton-classes/) is a design pattern that restricts 
the instance of a class to only one object. 

## When to use
When you wanted to share data between different parts of code, instead of having to pass the data around. For example,
global setting constant such as locale, language, currency etc.

##Implementation
Using [dispatch_once](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man3/dispatch_once_f.3.html) (Objective-C approach), if you are interested [Singletons in Objective-C](http://www.galloway.me.uk/tutorials/singleton-classes/). Like the name say, this function will execute block only once. We utilize this by put initialise code inside dispatch_once.

```swift
class Singleton {

  class var sharedInstance : Singleton {
    struct Static {
        static var onceToken : dispatch_once_t = 0
        static var instance : Const? = nil
    }
    dispatch_once(&Static.onceToken) {
        Static.instance = Const()
    }
    return Static.instance!
  }
  
}
```
###Using Example
```swift
var global_instance:Singleton = Singleton.sharedInstance
```

What happened here is when we call `Singleton.sharedInstance`, Static.instance will be initialised once and only once. Later when we try to access `Singleton.sharedInstance`, we shall get an exact same instance.  





##Example
```
- Const
  - Category1
    - key : value 
    - key : value 
    - key : value 
  - Category2
    - key : value 
    - key : value 
  - Category3
    - key : value 
    - key : value 
    - key : value 
```

```swift
class ConstSingleton {
    private var constant = Dictionary<String, Dictionary<String,String>>()
    
    func getConst(type:String, key: String) -> String? {
        if constant.isEmpty || constant[type] == nil{
            return nil
        }
        else {
            var myConst = constant[type]
            if myConst?[key] != nil {
                return myConst?[key]
            }
        }
        return nil
    }
    
    func setConst(type:String, key: String, value: String) -> Bool? {
        var success:Bool
        if constant.isEmpty || constant[type] == nil{
            var newConst = [key : value]
            if let unwrappedPreviousValue = constant.updateValue(newConst, forKey: type) {
                println("Replaced the previous value: \(unwrappedPreviousValue)")
            } else{
                println("Added a new value")
            }
        }
        else{
            if let unwrappedPreviousValue = constant[type]?.updateValue(value, forKey: key) {
                println("Replaced the previous value: \(unwrappedPreviousValue)")
            } else {
                println("Added a new value")
            }
            
        }
        return true
    }
    
    class var sharedInstance : ConstSingleton {
        struct Static {
            static var onceToken : dispatch_once_t = 0
            static var instance : ConstSingleton? = nil
        }
        dispatch_once(&Static.onceToken) {
            Static.instance = ConstSingleton()
        }
        return Static.instance!
    }
}
```

Ps. Sorry for my English : )
