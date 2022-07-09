# TS-Define-literal

- 定义字面量，比如 a只能被赋值 0 或者 1

```typescript
enum Gender {
    'male'= 0,
    'female'= 1
}

let a: 0|1 = Gender.male

let b: 0|1 = Gender.female

console.log(a)
console.log(b)

```

