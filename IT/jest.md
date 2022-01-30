# Jest
测试框架


[适配器文档]https://jestjs.io/zh-Hans/docs/expect


```js
test('2 加 2 等于 4', () => {  expect(2 + 2).toBe(4);});
```

在上面的代码中，`expect (2 + 2)` 返回了一个"预期"的对象。 你通常不会对这些期望对象调用过多的匹配器。 在此代码中，`.toBe(4)` 是匹配器。 当 Jest 运行时，它会跟踪所有失败的匹配器，以便它可以为你打印出很好的错误消息。

`toBe`使用 `Object.is`来进行精准匹配的测试。 如果您想要检查对象的值，请使用 `toEqual` 代替：

```js
test('对象赋值', () => {  const data = {one: 1};  data['two'] = 2;  expect(data).toEqual({one: 1, two: 2});});
```

复制

`toEqual` 递归检查对象或数组的每个字段。

## 真值[​](https://jestjs.io/zh-Hans/docs/using-matchers#%E7%9C%9F%E5%80%BC "直接链接到标题")

代码中的`undefined`, `null`, and `false`有不同含义，若你在测试时不想区分他们，可以用真值判断。 Jest提供helpers供你使用。

-   `toBeNull` 只匹配 `null`
-   `toBeUndefined` 只匹配 `undefined`
-   `toBeDefined` 与 `toBeUndefined` 相反
-   `toBeTruthy` 匹配任何 `if` 语句为真
-   `toBeFalsy` 匹配任何 `if` 语句为假

```js

test('null', () => {  
const n = null;  
expect(n).toBeNull();  
expect(n).toBeDefined();  
expect(n).not.toBeUndefined();  
expect(n).not.toBeTruthy();  
expect(n).toBeFalsy();  
});  
  
test('zero', () => {  
const z = 0;  
expect(z).not.toBeNull();  
expect(z).toBeDefined();  
expect(z).not.toBeUndefined();  
expect(z).not.toBeTruthy();  
expect(z).toBeFalsy();  
});





```

你应该使用最精确匹配的匹配器来测试你期望你的代码能干什么。

## 数字[​](https://jestjs.io/zh-Hans/docs/using-matchers#%E6%95%B0%E5%AD%97 "直接链接到标题")

大多数的比较数字有等价的匹配器。

```js
test('two plus two', () => {  const value = 2 + 2;  expect(value).toBeGreaterThan(3);  expect(value).toBeGreaterThanOrEqual(3.5);  expect(value).toBeLessThan(5);  expect(value).toBeLessThanOrEqual(4.5);  // toBe and toEqual are equivalent for numbers 
expect(value).toBe(4);  expect(value).toEqual(4);});
```

复制

对于比较浮点数相等，使用 `toBeCloseTo` 而不是 `toEqual`，因为你不希望测试取决于一个小小的舍入误差。

```js
test('两个浮点数字相加', () => {  
const value = 0.1 + 0.2;  //expect(value).toBe(0.3);           这句会报错，因为浮点数有舍入误差  
expect(value).toBeCloseTo(0.3); // 这句可以运行});
});
```

复制

## 字符串[​](https://jestjs.io/zh-Hans/docs/using-matchers#%E5%AD%97%E7%AC%A6%E4%B8%B2 "直接链接到标题")

您可以检查对具有 `toMatch` 正则表达式的字符串︰

```js
test('there is no I in team', () => {  expect('team').not.toMatch(/I/);});test('but there is a "stop" in Christoph', () => {  expect('Christoph').toMatch(/stop/);});
```

复制

## Arrays and iterables[​](https://jestjs.io/zh-Hans/docs/using-matchers#arrays-and-iterables "直接链接到标题")

你可以通过 `toContain`来检查一个数组或可迭代对象是否包含某个特定项：

```js

const shoppingList = [  'diapers',  'kleenex',  'trash bags',  'paper towels',  'milk',];test('the shopping list has milk on it', () => {  expect(shoppingList).toContain('milk');  expect(new Set(shoppingList)).toContain('milk');});
```

复制

## 例外[​](https://jestjs.io/zh-Hans/docs/using-matchers#%E4%BE%8B%E5%A4%96 "直接链接到标题")

若你想测试某函数在调用时是否抛出了错误，你需要使用 `toThrow`。

```
function compileAndroidCode() {  throw new Error('you are using the wrong JDK');}test('compiling android goes as expected', () => {  expect(() => compileAndroidCode()).toThrow();  expect(() => compileAndroidCode()).toThrow(Error);  // You can also use the exact error message or a regexp  expect(() => compileAndroidCode()).toThrow('you are using the wrong JDK');  expect(() => compileAndroidCode()).toThrow(/JDK/);});
```

复制

> 注意：函数需要在expect的包装函数中调用，否则 `toThrow`断言总是会失败。