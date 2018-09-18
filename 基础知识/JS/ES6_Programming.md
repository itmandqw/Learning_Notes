# try-catch-finally

执行流程   首先执行try块中的代码 如果抛出异常会由catch去捕获并执行  如果没有发生异常  catch去捕获会被忽略掉  但是不管有没有异常最后都会执行finally

```javascript
try {
   throw "test";
} catch (err) {
    console.log(err);    // test(返回的是try中throw的错误)
} finally {
    console.log('finally');
}
```

***

# Promise

