# ES6

## 1.ES6的模块化语法

####    默认导出语法export default {默认导出的成员}

  例子：

​    let n1 = 10

​    let n2 = 20

​    function show() {}

   export default {

   n1,

   show

  }

####   默认导入语法import m1 from ‘模块标识符’

  例子：

  import m1 from './01.js'

####    按需导出语法： export 按需导出的成员

例子

 export let s1 = 'aaa'

 export let s2 = 'ccc'

 export function say() {}

#### 按需导入语法;import { s1, s2, say } from ‘模块标识符’

例子

import { s1, s2, say } from './03-按需导入.js'

console.log(s1)

console.log(s2)

console.log(say);

## 2.使用Promise解决回调地域问题

promise.then()、promise.catch()

import thenFs from 'then-fs'

thenFs.readFile('./files/11.txt', 'utf8').then((r1) => {

​    console.log(r1);

​    return thenFs.readFile('./files/2.txt', 'utf8')

  })

  .then((r2) => {

​    console.log(r2);

​    return thenFs.readFile('./files/3.txt', 'utf8')

  })

  .then((r3) => { console.log(r3); })

  .catch((err) => {

​    console.log(err.message);

  })

## 3.使用async/await简化Promise的调用

​     方法中用到await，则方法需要被async修饰

import thenFs from 'then-fs'

//函数里面有await 这个方法就要被async关键词修饰

//在asycn方法中 第一个await之前的代码会同步执行 await之后的代码会异步执行

console.log('a');

async function getAllFiles() {

  console.log('b');

  const r1 = await thenFs.readFile('./files/1.txt', 'utf8')

  console.log(r1);

  const r2 = await thenFs.readFile('./files/2.txt', 'utf8')

  console.log(r2);

  const r3 = await thenFs.readFile('./files/3.txt', 'utf8')

  console.log(r3);

  console.log('d');

}

getAllFiles()

console.log('c');

// 输出结果：

// a

// b

// c

// 111

// 222

// 333

// d

## 4.Evenloop

## 5.宏任务和微任务的执行顺序

​    在执行下一个宏任务之前，先检查是否有待执行的微任务

 



  