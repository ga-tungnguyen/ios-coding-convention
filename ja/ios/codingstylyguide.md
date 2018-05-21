# ## GA Coding Guidelines

### Swift coding guidelines



このドキュメントは、以下に挙げる目標を達成できる方法を促進するために試しとして作成されたものです。

1. より厳密で、プログラムが誤解する可能性が少ないこと
2. 意図が明確であること
3. 上長さが排除されていること
4. 美学についての議論が少ないこと

---------------

#### Naming

Use the Swift naming conventions described in the API Design Guidelines. Some key takeaways include:

- striving for clarity at the call site
	- Preferred:  

		```
		var greeting = "Hello world!"
		``` 
	-  Not preferred:

		```
		var string = "Hello world"
		```
- prioritizing clarity over brevity
- using camel case (not snake case)
	- Preferred:  

		```
		var profileImageView: UIImageView!
		``` 
	-  Not preferred:

		```
		var profileimageview: UIImageView!
		```
- using uppercase for types (and protocols), lowercase for everything else
	- e.g: `HTTP, URL, POST, GET`
	
- including all needed words while omitting needless words
	- e.g: 

	```
	public mutating func removeElement(_ member: Element) -> Element?
	allViews.removeElement(cancelButton) 
	```
	In this case, the word `Element` adds nothing salient at the call site. 	This API would be better:
	 
	```
	public mutating func remove(_ member: Element) -> Element?
	allViews.removeElement(cancelButton) 
	```
- using names based on roles, not types
	- Not preferred:

	```
	class ProductionLine {
 		func restock(from widgetFactory: WidgetFactory)
	}
	```
	- Preferred: 
	
	```
	class ProductionLine {
  		func restock(from supplier: WidgetFactory)
	} 
	```
- sometimes compensating for weak type information
- beginning factory methods with make
	- e.g. `x.makeIterator() `.
- naming methods for their side effects
	- Those without side-effects should read as noun phrases, e.g. 		`x.distance(to: y), i.successor()`.

	- Those with side-effects should read as imperative verb phrases, e.g., 		`print(x), x.sort(), x.append(y)`. 
- verb methods follow the -ed, -ing rule for the non-mutating version
	- e.g: `z = x.sorted() , z = x.appending(y)` 
- protocols that describe what something is should read as nouns
	- e.g. `Collection`
- protocols that describe a capability should end in -able or -ible
- using terms that don't surprise experts or confuse beginners
- generally avoiding abbreviations
- using precedent for names
- giving the same base name to methods that share the same meaning
- labeling closure and tuple parameters 
	- Not preferred:

		` return (lastname, firstName)` 

	- Preferred: 

		` return (lastName: lastname, firstName: firstName)` 
		
### Delegate

When creating custom delegate methods, an unnamed first parameter should be the delegate source.

-	Not Preferred:

```
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

- 	Preferred:

```
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

### Use Type Inferred Context
Use compiler inferred context to write shorter, clear code. 

- Not Preferred:

```
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

- Preferred:

```
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

### Generics
Generic type parameters should be descriptive, upper camel case names. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as T, U, or V.

- Not Preferred:

```
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

- Preferred:

```
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

### Language
Use US English spelling to match Apple's API.

- Not Preferred:

```
let colour = "red"
```

- Preferred:

```
let color = "red"
```

### Code Organization
Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a // MARK: - comment to keep things well-organized.

### Protocol Conformance
In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.


- Not Preferred:

```
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

- Preferred:

```
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```
### Unused Code
Unused (dead) code, including Xcode template code and placeholder comments should be removed.

- Not Preferred:

```
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```

- Preferred:

```
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```
