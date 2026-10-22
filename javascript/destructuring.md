# JS：解构赋值的几个实用写法

## 对象解构：重命名 + 默认值

```js
const res = { data: { list: [] }, code: 200 };

const { data: payload = {}, code: statusCode = 500 } = res;
console.log(payload, statusCode);
```

接口字段名和本地变量名不一致时，重命名比 `const x = res.a_b_c` 清爽。

## 函数参数直接解构

```js
function createUser({ name, age = 18, role = "user" } = {}) {
  console.log(name, age, role);
}

createUser({ name: "a" });  // a 18 user
createUser();               // undefined 18 user，不报错
```

注意最后的 `= {}`：不传参时不至于解构 undefined 报错。

## 交换变量、取剩余

```js
let a = 1, b = 2;
[a, b] = [b, a];

const { password, ...safe } = user;  // 剔掉敏感字段再传
```

## 嵌套别太深

解构超过两层，可读性反而下降，不如老老实实分步写。
一行能看懂的才叫简洁。
