函数接口:Predicate
===

作用:断言

所在包:java.util.function    

1.Predicate<T>    

断言方法:

            boolean test(T t);    

默认实现的方法:

*1.&&*

    对应方法:    
            default Predicate<T> and(Predicate<? super T> other) {
                Objects.requireNonNull(other);
                return (t) -> test(t) && other.test(t);
            }    

*2.或*

    对应方法:
            default Predicate<T> or(Predicate<? super T> other) {
                Objects.requireNonNull(other);
                return (t) -> test(t) || other.test(t);
            }    
            
*3.!*

    对应方法:
            default Predicate<T> negate() {
                return (t) -> !test(t);
            }    
            
*4.equals*

    静态方法:
            static <T> Predicate<T> isEqual(Object targetRef) {
                return (null == targetRef)
                    ? Objects::isNull
                    : object -> targetRef.equals(object);
            }

示例:

        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(0,1,2,3,4,5,6,7,8,9);
            /* && */
            Predicate<Integer> p1 = i -> i > 5;
            Predicate<Integer> p2 = i -> i < 8;
            List<Integer> result = list.stream()
                    .filter(p1.and(p2)).collect(Collectors.toList());
            System.out.println("result:" + result.toString());
            /* result : [6, 7] */
            
            /* || */
            Predicate<Integer> p3 = i -> i > 8;
            Predicate<Integer> p4 = i -> i < 2;
            List<Integer> result1 = list.stream()
                    .filter(p3.or(p4)).collect(Collectors.toList());
            System.out.println("result1:" + result1.toString());
            /* result1 : [0, 1, 9] */
            
            /* ! */
            Predicate<Integer> p5 = i -> i != 2;
            List<Integer> result2 = list.stream()
                    .filter(p5.negate()).collect(Collectors.toList());
            System.out.println("result2:" + result2.toString());
            /* result2 : [2] */
            
            /* equals 调用传入对象的equals 方法*/
            List<Integer> result3 = list.stream()
                    .filter(Predicate.isEqual(5)).collect(Collectors.toList());
            System.out.println("result3:" + result3.toString());
            /* result3 : [5] */
        }

1.1 IntPredicate| DoublePredicate| LongPredicate
---

 ==独立接口==; Predicate<T>的Int、Double、Long类型入参扩展

        public static void main(String[] args) {
            
            /* IntPredicate */
            IntPredicate p1 = x -> x > 6;
            IntPredicate p2 = x -> x < 10;
            boolean test = p1.and(p2).test(7);
            System.out.println(test);
            
            /* DoublePredicate */
            DoublePredicate p3 = x -> x > 0;
            DoublePredicate p4 = x -> x != 0;
            boolean test2 = p3.or(p4).test(0);
            System.out.println(test2);
            
            /* LongPredicate */
            LongPredicate p5 = x -> x > 0;
            boolean test3 = p5.negate().test(5L);
            System.out.println(test3);
            
            /* LongPredicate 可以接收 Int 类型值,  同基础类型*/
            int i = Integer.MAX_VALUE;
            boolean test4 = p5.test(i);
            System.out.println(test4);
            
            /* 
            * ps.自己产生的错误想法:
            * filter 入参为 Predicate<? super T> predicate
            *    是不能使用IntPredicate、DoublePredicate、LongPredicate的
            *    因为和Predicate<T> 没有继承关系，是独立的接口
            */
            List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
            list.stream().filter(p1).collect(Collectors.toList());
        }
        
2.BiPredicate<T, U>    
---

==独立接口==; Predicate<T>的二元扩展    

            BiPredicate<String, LocalDate> biP1 = (x, y) -> {
                String temp = "abc";
                LocalDate date = LocalDate.of(2020, 1, 9);
                boolean test5 = Predicate.isEqual(temp).test(x);
                Predicate<LocalDate> datePredicate = time -> time.isBefore(date);
                boolean test6 = datePredicate.test(y);
                if(test5 && test6) {
                    return true;
                }
                return false;
            };
            boolean test7 = biP1.test("abc", LocalDate.of(2020, 1, 8));
            System.out.println(test7);