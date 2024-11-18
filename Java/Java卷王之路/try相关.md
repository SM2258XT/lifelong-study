---
title: try相关
updated: 2022-11-23T16:30:39
created: 2022-11-23T16:24:54
---

try {
return br.readLine();
} finally {
if (br != null) br.close();
}

如果try块和finally块中的方法，都抛出异常，那么try块中的异常会被抑制（suppress），只会抛出finally中的异常，而把try块的异常完全忽略。
如果我们用catch语句去获得try块的异常，也没有什么影响，catch块虽然能获取到try块的异常，但是对函数运行结束抛出的异常并没有什么影响。
每个实现了AutoCloseable（或者Closeable）的对象都是资源。

**try-with-resources自动调用close()**
try (BufferedReader br = new BufferedReader(new FileReader(path))) {
return br.readLine();
}
也可以有catch和finally块，不过只会在所声明的资源关闭之后才会运行
