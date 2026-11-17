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
### 迭代版本的 pascal 函数
前面给出的递归版本 pascal 函数的效率并不好，首先，因为它使用的是递归计算方式，所以它的递归深度受限于解释器所允许的最大递归深度。
其次，递归版本 pascal 函数计算值根据的是公式 $\binom{row}{col} = \binom{row-1}{col-1} + \binom{row-1}{col}$ ，这个公式带有重复计算，并且复杂度也很高，因此，前面给出的 pascal 函数只能用于 row 和 col 都非常小的情况。

要改进这一状况，可以使用帕斯卡三角形的另一个公式来计算帕斯卡三角形的元素： $\binom{row}{col} = \frac{row!}{col! \cdot (row-col)!}$ ，其中row!表示row的阶乘。

阶乘可以利用书本 34 页的 factorial 函数(迭代版)来计算：
```scheme
;;; iter-factorial.scm

(define (factorial n)
    (fact-iter 1 1 n))

(define (fact-iter product counter max-count)
    (if (> counter max-count)
        product
        (fact-iter (* counter product)
                   (+ counter 1)
                   max-count)))
```

然后，使用 factorial 和给定的公式，给出迭代版本的 pascal 定义：
```scheme
;;; 12-iter-pascal.scm

(load "iter-factorial.scm")

(define (pascal row col)
    (/ (factorial row)
       (* (factorial col)
          (factorial (- row col)))))
```
迭代版的 pascal 函数消除了递归版 pascal 的重复计算，并且它是一个线性复杂度的算法，因此，迭代版的 pascal 比起递归版的 pascal 要快不少，并且计算的值的大小不受递归栈深度的控制：
```scheme
1 ]=> (load "12-iter-pascal.scm")

;Loading "12-iter-pascal.scm"...
;  Loading "iter-factorial.scm"... done
;... done
;Value: pascal

1 ]=> (pascal 4 0)

;Value: 1

1 ]=> (pascal 4 4)

;Value: 1

1 ]=> (pascal 4 2)

;Value: 6

1 ]=> (pascal 1024 256)

;Value: 346269144346889986395924831854035885865562696970381965629886488418277363006094610102022787857042680079787023567910689787903778413121916299434613646920250770005333708478791829291433521372230388837458830111329620101516314992938333258379799593534242588
```
最后一个测试尝试了对 (pascal 1024 256) 进行求值，计算结果是一个蛮大的数，如果使用递归版的 pascal 函数的话，这种计算就要非常非常久了，但是迭代版本还是可以很快地算出来。
