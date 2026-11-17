稍微调整一下表格，这个解决方案就更容易理解了：
```scheme
 1
 1   1
 1   2   1
 1   3   3   1
 1   4   6   4   1
       . . .
```
根据给定的说明，计算给定位置处单个数字的函数可以递归定义：
```scheme
(define (pascal row col)
  (cond ((or (= col 1) (= col row)) 1)
        (else (+ (pascal (- row 1)  (- col 1))
                 (pascal (- row 1) col)))))
```
