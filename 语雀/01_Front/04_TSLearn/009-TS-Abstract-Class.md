# TS-Abstract-Class

- 抽象类和其他类区别不大，只是不能用来创建对象，抽象类就是专门用来被继承的类，抽象方法必须重写。

```javascript
enum Gender{male= 0,female=1}

abstract class Person{

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


    eat():void{}





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

    eat() {
        console.log('学生睡觉了')
    }


}

let student1:Student = new Student(1,'王乃醒',Gender.male,'201501818134')

student1.eat()






```

