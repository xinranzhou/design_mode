## 继承VS组合
在面向对象编程中，有一条非常经典的设计原则，那就是：组合优于继承，多用组合少用继承。为什么不推荐使用继承？组合相比继承有哪些优势？如何判断该用组合还是继承？

### 为什么不推荐使用继承

继承是面向对象编程的四大特性之一，用来表述类与类之间is-a的关系，可以解决代码复用问题。但是如果继承层级过深，过于复杂，也会影响到代码到可读性与可维护性。
![avatar](https://static001.geekbang.org/resource/image/3f/c6/3f99fa541e7ec7656a1dd35cc4f28bc6.jpg)
定义一个鸟类抽象类，抽象出一个会飞的鸟类和不会飞的鸟类，再抽象是否能下蛋的类，此时有三层继承关系，在具体实现细节中如果某个实现方法异常或者父类方法需要重新实现则需要一层一层往上查找，并且父类抽象方法变化所有子类都需要做相应的变化。

总之，继承最大的问题就在于：继承层次过深、继承关系过于复杂会影响到代码的可读性和可维护性。这也是为什么我们不推荐使用继承

### 组合相比继承有哪些优势

实际上，可以使用组合，接口，委托三个技术手段一块来实现继承能力。

```

下面是以TpyeScript举例
基于抽象类继承
```
abstract class Flyable {
    abstract fly(): any
}

abstract class EggTweetlayable {
    constructor(parameters: any) {

    }
    fly() {
        console.log('fly')
    }
    abstract layEgg(): any
    abstract tweet(): any
}


// 鸵鸟类
class Ostrich extends EggTweetlayable {
    constructor(arg: any) {
        super(arg)
    }
    public layEgg(): void {
        console.log('layEgg') // 实现layEgg
    }

    public tweet(): void {
        console.log('tweet') // 实现tweet
    }
}
const ostrich = new Ostrich('')

ostrich.layEgg()
ostrich.fly()
```

利用组合实现

```
// 定义接口
interface Flyable {
    fly: () => {}
}
interface Egglayable {
    layEgg: () => void
}
interface Tweetable {
    tweet: () => void
}

class Egglayability implements Egglayable {
    layEgg() {
        console.log('layEgg')
    }
}

class Tewwtability implements Tweetable {
    tweet() {
        console.log('tweet')
    }
}
// 鸵鸟类
class Ostrich implements Egglayable, Tweetable {
    private eggAbility = new Egglayability() // 组合
    private tweetAbility = new Tewwtability() // 组合
    constructor() {
    }

    public layEgg():void {
        this.eggAbility.layEgg() // 委托
    }

    public tweet():void {
        this.tweetAbility.tweet() // 委托
    }
}
```
通过组合方式，鸵鸟子类也拥有类能下蛋与会叫的方法。同样实现了继承的能力

继承主要有三个作用：表示 is-a 关系，支持多态特性，代码复用。而这三个作用都可以通过其他技术手段来达成。比如 is-a 关系，我们可以通过组合和接口的 has-a 关系来替代；多态特性我们可以利用接口来实现；代码复用我们可以通过组合和委托来实现。所以，从理论上讲，通过组合、接口、委托三个技术手段，我们完全可以替换掉继承，在项目中不用或者少用继承关系，特别是一些复杂的继承关系

### 如何判断该用组合还是继承
尽管我们鼓励多用组合少用继承，但组合也并不是完美的，继承也并非一无是处。在实际的项目开发中，我们还是要根据具体的情况，来选择该用继承还是组合。如果类之间的继承结构稳定，层次比较浅，关系不复杂，我们就可以大胆地使用继承。反之，我们就尽量使用组合来替代继承。除此之外，还有一些设计模式、特殊的应用场景，会固定使用继承或者组合
