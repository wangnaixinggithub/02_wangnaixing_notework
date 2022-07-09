# TypeScript-Create-Object

- 创建对象

```typescript
enum Gender{male= 0,female=1}

class Person{

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



}


//1.使用构造函数创建Person实例person1
let person1:Person = new Person(1,'王乃醒',Gender.male)

//2.通过Setter方法，修改person1的私有属性

person1.id = 2
person1.name =  '王大帅哥'
person1.sex = Gender.female

//3.通过Getter方法，访问person1的私有属性
const id1 = person1.id;
const name1 = person1.name;
const sex1 = person1.sex

console.log(id1)  // 2
console.log(name1) // 王大帅哥
console.log(sex1) // 1





```

