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
前面给出的递归版本 pascal 函数的效率并不好，首先，因为它使用的是递归计算方式，所以它的递归深度受限于解释器所允许的最大递归深度。
其次，递归版本 pascal 函数计算值根据的是公式$\binom{row}{col} = \binom{row-1}{col-1} + \binom{row-1}{col}$，这个公式带有重复计算，并且复杂度也很高，因此，前面给出的 pascal 函数只能用于 row 和 col 都非常小的情况。

要改进这一状况，可以使用帕斯卡三角形的另一个公式来计算帕斯卡三角形的元素：$\binom{row}{col} = \frac{row!}{col! \cdot (row-col)!}$，其中row!表示row的阶乘(factorial)。
