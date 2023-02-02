| 序号 | 内容                     | 链接地址                                                 |
| ---- | ------------------------ | -------------------------------------------------------- |
| 1    | Java8新特性-Lambda表达式 | https://thinkwon.blog.csdn.net/article/details/113764085 |
| 2    | Java8新特性-Optional     | https://thinkwon.blog.csdn.net/article/details/113791796 |
| 3    | Java8新特性-Stream       | https://thinkwon.blog.csdn.net/article/details/113798096 |
| 4    | Java8新特性-Base64       | https://thinkwon.blog.csdn.net/article/details/113798575 |
| 5    | Java8新特性-日期时间API  | https://thinkwon.blog.csdn.net/article/details/111087199 |

### Stream

#### skip

> Stream<T> skip(long n) 

**参数**：传递要跳过的前导元素数,跳过前n个元素。
**返回**：该方法返回一个跳过元素的新流。
**异常**：如果我们传递负数，它将抛出IllegalArgumentException。

1. skip方法用于从一开始跳过给定数量元素的源流创建新流。
2. skip方法对于有序并行管道，特别是对于n个数较多的情况，代价较高。
3. skip方法是顺序流管道的廉价操作。
4. 如果要跳过的元素数n等于或大于流中的元素数，则输出将为空流。
5. skip是有状态的中间操作(`stateful intermediate operation`)。