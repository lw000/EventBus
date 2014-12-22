EventBus
======
[![GoDoc](https://godoc.org/github.com/asaskevich/EventBus?status.svg)](https://godoc.org/github.com/asaskevich/EventBus)

Package EventBus is the little and lightweight eventbus with async compatibility for GoLang.

#### Installation
Make sure that Go is installed on your computer.
Type the following command in your terminal:

	go get github.com/asaskevich/EventBus

After it the package is ready to use.

#### Import package in your project
Add following line in your `*.go` file:
```go
import "github.com/asaskevich/EventBus"
```
If you unhappy to use long `EventBus`, you can do something like this:
```go
import (
	evbus "github.com/asaskevich/EventBus"
)
```

#### Example
```go
func calculator(a int, b int) {
	fmt.Printf("%d\n", a + b)
}

func main() {
	bus := EventBus.New();
	bus.Subscribe("main:calculator", calculator);
	bus.Publish("main:calculator", 20, 40);
	bus.Unsubscribe("main:calculator");
}
```

#### Implemented methods
* **New()**
* **Subscribe()**
* **SubscribeOnce()**
* **HasCallback()**
* **Unsubscribe()**
* **Publish()**
* **PublishAsync()**
* **WaitAsync()**

#### New()
New returns new EventBus with empty handlers.
```go
bus := EventBus.New();
```

#### Subscribe(topic string, fn interface{}) error
Subscribe to a topic. Returns error if `fn` is not a function.
```go
func Handler() { ... }
...
bus.Subscribe("topic:handler", Handler)
```

#### SubscribeOnce(topic string, fn interface{}) error
Subscribe to a topic once. Handler will be removed after executing. Returns error if `fn` is not a function.
```go
func HelloWorld() { ... }
...
bus.SubscribeOnce("topic:handler", HelloWorld)
```

#### Unsubscribe(topic string) error
Remove callback defined for a topic. Returns error if there are no callbacks subscribed to the topic.
```go
bus.Unsubscribe("topic:handler");
```

#### HasCallback(topic string) bool
Returns true if exists any callback subscribed to the topic.

#### Publish(topic string, args ...interface{})
Publish executes callback defined for a topic. Any addional argument will be tranfered to the callback.
```go
func Handler(str string) { ... }
...
bus.Subscribe("topic:handler", Handler)
...
bus.Publish("topic:handler", "Hello, World!");
```

#### PublishAsync(topic string, args ...interface{})
PublishAsync executes callback defined for a topic asynchronously. Useful for slow callbacks.
Any addional argument will be tranfered to the callback.
```go
func slowCalculator(a, b int) {
	time.Sleep(3 * time.Second)
	fmt.Printf("%d\n", a + b)
}
...
bus := EventBus.New();
bus.Subscribe("main:slow_calculator", slowCalculator);

bus.Publish("main:slow_calculator", 20, 60); // synchronous execution means wait.
fmt.Println("I got blocked waiting")

bus.PublishAsync("main:slow_calculator", 30, 70);

fmt.Println("start: do some stuff while waiting for a result")
fmt.Println("end: do some stuff while waiting for a result") 

bus.WaitAsync(); // wait for all async callbacks to complete
bus.Unsubscribe("main:slow_calculator");
```

####  WaitAsync()
WaitAsync waits for all async callbacks to complete.

#### Support
If you do have a contribution for the package feel free to put up a Pull Request or open Issue.

#### Special thanks to [contributors](https://github.com/asaskevich/EventBus/graphs/contributors)
* [Brian Downs](https://github.com/briandowns)
* [Dominik Schulz](https://github.com/gittex)
* [bennAH](https://github.com/bennAH)