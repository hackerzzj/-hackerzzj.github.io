js小技巧

- `&&` 和 `||` 运算符使用短路逻辑（short-circuit logic），是否会执行第二个语句（操作数）取决于第一个操作数的结果。在需要访问某个对象的属性时，使用这个特性可以事先检测该对象是否为空：

  ```js
  var name = o && o.getName();
  ```

- 或用于缓存值（当错误值无效时）：

```js
var name = cachedName || (cachedName = getName());
```

- 类似地，JavaScript 也有一个用于条件表达式的三元操作符：

```js
var allowed = (age > 18) ? "yes" : "no";
```

