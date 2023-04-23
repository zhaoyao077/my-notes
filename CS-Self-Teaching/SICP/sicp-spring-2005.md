# Structure And Interpretation Of Computer Programs

> This lecture notes is about SICP in Spring 2005 by Hal Abelson and Gerald Jay Sussman.

## Lec1A

> 2023/4/23

### ~~Computer Science~~

- Don't confuse what you learn with what tools you use.
  - Computer Science is a bad name as a major.
- PROCESS ("what")
  - **A process** is a series of related tasks or methods that together turn inputs into outputs.
- PROCEDURE ("how")
  - **A procedure** is a prescribed way of undertaking a process or part of a process.
- LISP
  - easy to pick up rules
  - not easy to become a master programmer
- 构建软件系统是理想化的，构建的产品受限于我们的想象力.

### Techniques for controlling complexity 控制复杂性的技巧

#### Black-box abstraction 黑盒抽象

- 比如求平方根(SQRT)的过程要用到求函数固定点(FIXED-POINT)的算法，就可以直接当作黑盒使用.
- 包含：
  - Primitive Objects
    - Primitive Procedures
    - Primitive Data
  - Means of Combination
    - Procedure Composition
    - Construction of Compound Data
  - Means of Abstraction
    - Procedure Definition
    - Simple Data Abstraction
  - Capturing Common Patterns
    - High-order Procedures
    - Data as Procedures

#### Conventional Interfaces 通用接口

- 比如加法运算可以用于numbers、vectors等data.
- 包含：
  - Generic Operations
  - Large-scale Structure and Modularity
  - Object-Oriented Programming (OOP)
  - Operations on Aggregates

#### Metalinguistic Abstraction 元语言抽象

- 比如λ表达式、用lisp解释lisp(APPLY-EVAL)
- 包含：
  - Interpretation
  - Example-Logic Programming
  - Register Machines

### LISP (Scheme)

- lisp是大小写不敏感的

- Primitive Elements

- Means of Combination

- Means of Abstraction

- Examples

  ```scheme
  ; prefix notation 前缀表达式
  (+ 3 (* 5 6) 8 2)
  ```

  ```scheme
  ; define a
  (define A (* 5 5))
  A -> 25
  (A) -> ERROR
  
  ; define d
  (define (D) (* 5 5))
  D -> COMPOUND PROCEDURE
  (D) -> 25
  
  ; define square的两种等价方式
  (define (SQUARE x) (* x x))
  (define SQUARE (lambda (x) (* x x) ))
  
  (SQUARE 10) -> 100
  
  ; define average of x, y
  (define (average x y)
    (/ (+ x y) 2))
  
  ; define mean square of x, y
  (define (mean-square x y)
    (average (square x)
             (square y)))
  
  (mean-square 4 5) -> 20.5
  ```
  
  ```scheme
  ; use cond to define abs
  (define (abs x)
    (cond ((< x 0) (- x))
          (x)))
  
  (abs -30) -> 30
  ```
  
- SQRT 求平方根

  - make a guess G
  - improve the guess by averaging G and X/G
  - keep improving the guess until it is good enough
  - use 1 as an initial guess

  ```scheme
  (define (SQRT x) (TRY 1 x))
  
  (define (TRY guess x)
    (if (GOOD-ENOUGH? guess x)
        guess
        (TRY (IMPROVE guess x) x))) ; recursive definition 递归定义
  
  (define (IMPROVE guess x)
    (average guess (/ x guess)))
  
  (define (GOOD-ENOUGH? guess x)
    (< (abs (- (SQUARE guess) x))
    .001))
  ```

  

  

