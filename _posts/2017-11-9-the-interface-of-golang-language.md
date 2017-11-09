#### Go不是典型的[OO语言](https://baike.so.com/doc/6146835-6360018.html)，它在语法上不支持类和继承的概念。但是Go语言引入了一种新类型—Interface，它在效果上实现了类似于C++的“多态“。Go语言官网对于Interface类型的描述是：
>An interface type specifies a method set called its interface. A variable of interface type can store a value of any type with a method set that is any superset of the interface. Such a type is said to implement the interface. The value of an uninitialized variable of interface type is nil.
>一个 interface 类型定义了一个 方法集 作为其 接口。 interface 类型的变量可以保存含有属于这个 interface 类型方法集超集的任何类型的值，这时我们就说这个类型 实现 了这个 接口。未被初始化的 interface 类型变量的零值为 nil。

---

``` golang
package main

import (
    "fmt"
    "math"
)

type Abser interface {
    Abs() float64
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

func main() {
    var a Abser
    f := MyFloat(-math.Sqrt2)
    a = f     // a MyFloat implements Abser
    fmt.Println(a.Abs())
}
```
####从语法上看，Interface定义了一个或一组[method](http://blog.csdn.net/uudou/article/details/52412831)，这些method只有函数签名，没有具体的实现代码。若某个数据类型实现了Interface中定义的那些被称为"method"的函数，则称这些数据类型实现了interface。上面的代码中，第8-10行是通过type语法声明了一个名为Abser的interface类型（Go中约定的interface类型名通常取其内部声明的method名的er形式）。第12-19行通过type语法声明了MyFloat类型且为该类型定义了名为Abs()的method。根据上面的解释，Abs()是interface类型Abser定义的方法，而MyFloat实现了该方法，所以，MyFloat实现了Abser接口。

---
####Interface类型的更通用定义可归纳如下：
```golang
type Namer interface {
    Method1(param_list) return_type
    Method2(param_list) return_type
    ...
}
```
#### 用type语法声明了一个名为Namer的interface类型,但Namer不是个具体的变量，此时内存中还没有它对应的对象。
#### 在Go语言中，一个类只需要实现了接口要求的所有函数，这个类就实现了该接口，即非侵入式接口（与java式的侵入式接口相反）。[许式伟](https://baike.so.com/doc/7212629-7437327.html)在《go语言编程》中谈到了非侵入式接口的好处：
>其一， Go语言的标准库，再也不需要绘制类库的继承树图。你一定见过不少C++、 Java、 C# 类库的继承树图。这里给个Java继承树图：  http://docs.oracle.com/javase/1.4.2/docs/api/overview-tree.html  在Go中，类的继承树并无意义，你只需要知道这个类实现了哪些方法，每个方法是啥含义就足够了。 
其二，实现类的时候，只需要关心自己应该提供哪些方法，不用再纠结接口需要拆得多细才 合理。接口由使用方按需定义，而不用事前规划。 
其三，不用为了实现一个接口而导入一个包，因为多引用一个外部的包，就意味着更多的耦 合。接口由使用方按自身需求来定义，使用方无需关心是否有其他模块定义过类似的接口。

---
#### golang不支持完整的面向对象思想，没有继承，多态依赖接口实现。golang只能模拟继承，其本质是组合，但是golang语言提供了一些语法糖使其看起来达到了继承的效果。golang的设计理念大道至简，传统的继承概念在golang中已经显得不是那么必要，golang通过接口去实现多态。interface类型不包含任何数据成员。interface类型进行转换的时候，默认返回的是临时对象，所以如果需要对原始对象进行修改，需要获取原始对象的指针。下面是一个稍微复杂点的例子。
```golang
type IPizzaCooker interface {
    Prepare(*Pizza)
    Bake(*Pizza)
    Cut(*Pizza)
}
 
func cookOnePizza(ipc IPizzaCooker) *Pizza {
    p := new(Pizza)
    ipc.Prepare(p)
    ipc.Bake(p)
    ipc.Cut(p)
    return p
}
 
type PizzaDefaultCooker struct {
}
 
func (this *PizzaDefaultCooker) CookOnePizza() *Pizza {
    return cookOnePizza(this)
}
func (this *PizzaDefaultCooker) Prepare(*Pizza) {
    //....default prepare pizza
}
func (this *PizzaDefaultCooker) Bake(*Pizza) {
    //....default bake pizza
}
func (this *PizzaDefaultCooker) Cut(*Pizza) {
    //....default cut pizza
}
 
type MyPizzaCooker struct {
    PizzaDefaultCooker
}
 
func (this *MyPizzaCooker) CookOnePizza() *Pizza {
    return cookOnePizza(this)
}
func (this *MyPizzaCooker) Bake(*Pizza) {
    //....bake pizza use my style
}
 
func main() {
    var cooker MyPizzaCooker
    p := cooker.CookOnePizza()
    //....
}
```

---
关于interface的一篇文章：http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go