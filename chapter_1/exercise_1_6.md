由于new-if是一个函数，而不是特殊形式，因此根据应用序每个参数子表达式都会在执行该过程之前进行求值。这意味着，在执行该sqrt-iter函数时，无论(good-enough? guess x)的求值结果如何，(good-enough? guess x)，guess和(sqrt-iter (improve guess x) x)它们都会在执行new-if之前进行求值。

由于分支(sqrt-iter (improve guess x) x)是递归调用函数，因此该函数会陷入无限循环。在这种情况下，有趣的是，new-if将永远不会被执行。

可以通过执行以下命令进行检查：

``` Scheme
(define (new-if predicate then-clause else-clause)
  (cond (predicate then-clause)
        (else else-clause)))

(define (average x y) (/ (+ x y) 2))

(define (square x) (* x x))

(define (improve guess x) (average guess (/ x guess)))

(define (good-enough? guess x)
(< (abs (- (square guess) x)) 0.001))


(define (sqrt-iter guess x)
  (new-if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x) x)))


(sqrt-iter 1.0 4) ; 无限循环
```
