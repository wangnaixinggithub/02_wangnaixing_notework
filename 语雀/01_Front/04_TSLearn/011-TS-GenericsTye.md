# TS-GenericsTye

- 方法形参是泛型

```javascript
function fun01<T>(a:T){
    console.log(a)
    console.log('方法形参为泛型的方法')
}

this.fun01('str')
this.fun01(1)
this.fun01(true)

```

- 方法返回值是泛型

```typescript
function fun01<T>(a:T):T{
    return a
}

console.log(this.fun01('str')); //str
console.log(this.fun01(1)); // 1
console.log(this.fun01(true)); //true

```

- 类中使用泛型

```javascript
class MyClss<T> {

    constructor(name:T) {
       this._name = name
    }


    //1.泛型属性
    private _name:T

    get name(): T {
        return this._name;
    }
    set name(value: T) {
        this._name = value;
    }

    //2.泛型方法
    fun01<T>(a:T):T{
        return a
    }

}

let myclass1:MyClss<string> = new MyClss<string>('王乃醒')

let myclass2:MyClss<number> = new MyClss<number>(1)

console.log(myclass1.fun01('str')); //str
console.log(myclass2.fun01('str')); //str


```

