---
title: volatile和内存屏障
updated: 2022-09-12T09:48:51
created: 2022-09-06T14:56:22
---

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 40%" />
<col style="width: 27%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr class="odd">
<td></td>
<td>原子性</td>
<td>可见性</td>
<td>一致性（重排序后的有序性）</td>
</tr>
<tr class="even">
<td>底层</td>
<td><ol type="1">
<li><p>CAS</p></li>
<li><p>Java 只保证基本数据类型<strong>？</strong></p></li>
</ol></td>
<td>volatile</td>
<td>volatile（例如单例DCL）</td>
</tr>
<tr class="odd">
<td>语法层</td>
<td></td>
<td>synchronize、锁</td>
<td></td>
</tr>
</tbody>
</table>

**volatile实现原理**
在 JVM 底层，volatile 是采用“内存屏障”来实现的。
效果：
1.  保证可见性，但不保证原子性
2.  禁止指令重排序（编译器重排序、处理器重排序）
Volatile规则：对volatile变量的写操作，happen-before 后续的读操作。（先读后写）
加入volatile 关键字时，会多出一个 lock 前缀指令。lock 前缀指令，其实就相当于一个内存屏障。内存屏障是一组处理指令，用来实现对内存操作的顺序限制。volatile 的底层就是通过内存屏障来实现的。

重排序条件：1.满足as-if-serial；2.不能存在数据依赖
public class RecordExample2 {
int a = 0;
boolean flag = false;

/\*\*
\* A线程执行
\*/
public void writer() {
a = 1; // 1
flag = true; // 2
}

/\*\*
\* B线程执行
\*/
public void read(){
if (flag) { // 3
int i = a + a; // 4
}
}

}
1、2可以被重排序。如果是volatile boolean，则不能重排序。
因为对于volatile修饰的flag变量，在后续中（thread b中）涉及到读取，因此对于flag所有写操作之前的指令，不能被重排序到写操作后面。（volatile读要保证所有的写都完毕！）
3、4存在控制依赖，不能被重排序。但是！！处理器为了进一步优化，可能**猜测执行**，来克服控制依赖对并行的影响程度
- 猜测执行：无论if是否可以被判断，先执行if的内容。当if可以被判断后，如果为真则直接返回结果。

操作 3 和操作 4 之间也可以重排序，虽然他们之间存在一个控制依赖的关系，只有操作 3 成立操作 4 才会执行。当代码中存在控制依赖性时，会影响指令序列的执行的并行度，所以编译器和处理器会采用猜测执行来克服控制依赖对并行度的影响。假如操作 3 和操作 4 重排序了，操作 4 先执行，则先会把计算结果临时保存到重排序缓冲中，当操作 3 为真时，才会将计算结果写入变量 i 中。
