函数接口:Predicate < T >
===

作用:断言

所在包:java.util.function

断言方法:

            boolean test(T t);    

默认实现的方法:

*1.&&*

    对应方法:    
            default Predicate<T> and(Predicate<? super T> other) {
                Objects.requireNonNull(other);
                return (t) -> test(t) && other.test(t);
            }    

*2.‘||’*

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
            
*4.==*

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
            
            /* == */
            List<Integer> result3 = list.stream()
                    .filter(Predicate.isEqual(5)).collect(Collectors.toList());
            System.out.println("result3:" + result3.toString());
            /* result3 : [5] */
        }
