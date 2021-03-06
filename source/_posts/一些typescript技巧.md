---
title: 一些typescript技巧
date: 2019-11-19 12:17:20
tags:
	- 笔记
---



# 一些typescript技巧

用了 TS 也2年多了, 把遇到的一些 TS 的问题以及解决方案罗一下

> 有确说实 也就是文档看少了 [见引用资料](#引用资料)

多看一下 `typescript` 的源码, 以及各大流行框架的源码, 可以学到很多知识

<!--more-->

## Any

毕竟 <span style="font-size:2rem;color: #294E80"><span style="color: red">Any</span>Script</span>

没啥好说的 `any as any`



## 一些新特性

### Optional chaining

> TS 3.7.2 版本新增 [详细说明](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining)

这玩意很多语言都有了,  就是让你少写一些 `if` / `&&`

```typescript
let x = foo?.bar.baz();
↓
let x = (foo === null || foo === undefined) ?
    undefined :
    foo.bar.baz();
```

### Nullish Coalescing

> TS 3.7.2 版本新增 [详细说明](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing)

就检查默认值是否存在, 官方例子

```typescript
let x = foo ?? bar();
↓
let x = (foo !== null && foo !== undefined) ?
    foo :
    bar();
```

实际作用就是代替我们经常使用的 `||`, 避免一些不可预期的错误 比如 `0`,`""(空字符串`

```typescript
function Foo(name: string | null) {
   // this.name = this.name || "default";
    this.name = this.name ?? "default"
}
```

## 从 JSON生成Typescript定义

我一般使用的 是 这个[Json2Ts](http://json2ts.com/)

## 关于对象

### MapLike

```typescript
interface MapLike<T> {
    [index: string]: T;
}
```

比如你定义了一个 Object 但是没定义 type 想操作里面的东西, 但是提示 Key 必须是 `unique symbols`的时候



如果给定了 Object 的 keys 列表

```typescript
type usernames = "haozi" | "haozi2"

type IUser<T> = {
    [index in usernames]: T
}
```



如果给定的是 一个 `class `,  `interface`



```typescript
type A<T> = {
    [index in keyof ITest]: T;
};
```

* typeof - 获取变量的类型
* keyof - 获取类型的键
* in - 遍历键名

以上三个可以组合使用

## 类型检查

函数入参增加类型检查, 保证 `a`的读取写入安全

```typescript
class Test {
	a: string 
    
    get_prototype<T extends keyof Test>(key: T): Test[T] {
        return this[key];
    }
    set_prototype<T extends keyof Test>(key: T, value: Test[T]): this {
        (this as Test)[key] = value;
        return this;
    }
}
```

## never

这属性用到的蛮少的, 比如一个函数永远不会返回值, 比如 `renderLoop`

```typescript
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}

// Inferred return type is never
function fail() {
    return error("Something failed");
}

// Function returning never must have unreachable end point
function infiniteLoop(): never {
    while (true) {
    }
}
```



## 工具属性

没啥好说的 [文档](https://www.typescriptlang.org/docs/handbook/utility-types.html),(点击函数名也可以跳转)

* [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt),  将 `T` 里面所以的属性变为可选属性

* [`Readonly<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlyt) 将 `T` 里面所以的属性变为只读属性

* [`Record<K,T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#recordkt) 构造一个类型为`T`属性为K的类型, 类似



```typescript
interface F<K,T> {
    [index in T]: K
}
```


* [`Pick<T,K>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktk) 从 `T` 里面选择 `K`的成员 来组成一个新的属性

* [`Omit<T,K>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittk) 和 `Pick` 相反,  从 `T`里面排除`K`的成员来组成新的属性 

* [`Exclude<T,U>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#excludetu)取`T`,`U`两个元素的

* [`Extract<T,U>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttu) 取`T`,`U`的成员并集

* [`NonNullable<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#nonnullablet) 排除 `T` 中的所有 `null`, `undefine`

* [`ReturnType<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#returntypet) 由函数类型`T`的返回值类型构造一个类型。

* [`InstanceType<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#instancetypet)由构造函数类型`T`的实例类型构造一个类型。

* [`Required<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#requiredt) 将`T`成员改为 `required`

* [`ThisType<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#thistypet)这个工具不会返回一个转换后的类型。它做为上下文的`this`类型的一个标记。注意，若想使用此类型，必须启用`--noImplicitThis`。



## 不支持 Emoji 的语言是屑

```typescript
const 🌶️💦💉💧🐂🍺 true
const 👴👀⑧⭐️ false
const 🍜 main
const 🔢 int
const 🔠 char
```

## IOC 容器

// TODO

## 剩下的问题持续收集中

// TODO

## 引用资料

[1]: Typescript Documentation https://www.typescriptlang.org/docs/home.html

[2]: TypeScript Deep Dive https://basarat.gitbooks.io/typescript/