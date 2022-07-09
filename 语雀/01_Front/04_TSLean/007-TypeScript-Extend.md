# TypeScript-Extend

- 继承父类的属性和方法，构造方法使用super()，指代调用父类的构造方法。

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

class Student extends Person{

    constructor(id:number,name:string,sex:0|1,studentNumber:string) {
        super(id,name,sex);
        this._studentNumber = studentNumber
    }

    private _studentNumber!: string

    get studentNumber(): string {
        return this._studentNumber;
    }

    set studentNumber(value: string) {
        this._studentNumber = value;
    }

}

let student1:Student = new Student(1,'王乃醒',Gender.male,'201501818134')

console.log(student1.id); //1
console.log(student1.name); // '王乃醒'
console.log(student1.sex); // 0
console.log(student1.studentNumber); // '201501818134'




```

