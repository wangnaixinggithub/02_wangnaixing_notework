# TS-Interface

# 1、implements And overwrite Interface Method

```javascript
enum Gender{male= 0,female=1}

interface Ai{
    doMoreThing():void
}

 class Person implements Ai{

    //1.私有属性
    private _id:number
    private _name:string
    private _sex:0|1

    //2.构造方法
    constructor(id:number,name:string,sex:0|1) {
        this._id = id
        this._name = name
        this._sex = sex
    }


    //Getter/Setter 访问器
    get sex(): 0 | 1 {
        return this._sex;
    }

    set sex(value: 0 | 1) {
        this._sex = value;
    }
    get name(): string {
        return this._name;
    }

    set name(value: string) {
        this._name = value;
    }
    set id(value: number) {
        this._id = value;
    }
    get id(): number {
        return this._id;
    }

    doMoreThing() {
        console.log('可以做很多很多我做不到的事！！！')
    }


 }

 let person1:Person = new Person(1,'王乃醒',Gender.male)

person1.doMoreThing()

```

# 2、Limit Type

- 在构建类型的时候不再和之前JS一样，可以无限制添加属性了。

```javascript
enum Gender{male= 0,female=1}

interface myInterface{
     id:number
     name:string
     sex:0|1

}


let obj:myInterface = {
    id:1,
    name:'王乃醒',
    sex:Gender.female
}

```

