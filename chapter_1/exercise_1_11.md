递归方案：
```scheme
(define (f-recursive n)
  (if (< n 3)
      n
      (+ (f-recursive (- n 1))
         (* 2 (f-recursive (- n 2)))
         (* 3 (f-recursive (- n 3))))))
```
迭代方案：
```scheme
(define (f-iterative n)
  (define (f-loop a b c count)
    (if (= n count)
        a  ; Final result of the computation
        (f-loop (+ a (* 2 b) (* 3 c))  ; Compute f(n)
                a
                b 
                (+ 1 count))))
  (if (< n 3)
      n
      (f-loop 2 1 0 2)))
```
