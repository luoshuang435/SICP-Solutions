使用练习 1.7 中针对非常大的数和非常小的数的相同改进方法：

```scheme
(define (cube x) (* x x x))

(define (good-enough? previous-guess guess)
  (< (abs (/ (- guess previous-guess) guess)) 0.00000000001))

(define (cube-root-iter guess x)
  (if (good-enough? (improve guess x) guess)
      guess
      (cube-root-iter (improve guess x) x)))

(define (improve guess x)
  (/ (+ (/ x (* guess guess)) (* 2 guess)) 3))


(define (cube-root x)
  (cube-root-iter 1.0 x))
```

返回：
```scheme
(cube-root 12345) -> 23.111618749807374
(cube 23.111618749807374) -> 12345.000000000167
```
