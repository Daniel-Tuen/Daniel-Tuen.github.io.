java.util.function 包概览    
===

1.一元函数:    
---

    Predicate<T> : 断言; 传入T类型参数, 返回boolean类型值    

    Consumer<T> : 消费传入的T类型参数, 无返回值    

    Supplier<T> : get()返回T 类型值    

    Function<T, R> : 消费传入的T类型参数, 返回R 类型值    

    UnaryOperator<T> : Function<T, R>的入参和返回值都为T类型的扩展    

2.二元函数:    
---

    BiConsumer<T, U> : 消费传入的T类型、U类型参数, 无返回值    

    BiPredicate<T, U> : 传入T类型、U类型参数, 返回boolean类型值    

    BiFunction<T, U, R> : 消费传入的T类型、U类型参数, 返回R 类型值    

    BinaryOperator<T> : BiFunction<T, U, R>的入参和返回值都为T类型的扩展

3.扩展函数: 基于基础类型常用扩展(图)    
---
    
![img](/assets/20200109-function.png)