


## 学习这本书可以参考的资料

首先说句："google 好样的，同样的关键字【the little schemer 中文】，google第1页就能搜出下面的资料，百度第1页搜的都是啥，都是啥！！！"

别人写好的**翻译版本**。
> 
http://www.cnblogs.com/Z-X-L/tag/scheme/

有人把上面的 **翻译版本** 上传到了 github.io 网站上面。
>   
http://uternet.github.io/TLS/

把书上的“问答”转化为了“现场对话”的形式,转化成了**在线学习**了？！
>  
http://vivid-cn.chengyichao.info/

《The Little Schemer》**学习笔记** - "五法十诫"
>  
http://personball.com/fp/2013/01/15/notes-about-the-little-schemer/

先看原书，看不下去的地方，去看翻译版本，再然后看 在线学习 网站，试着更加深入的了解。


## 概要

第8章 	讲解把函数当作参数         
第9章 	讲解Y组合子，以及匿名函数中如何定义递归的，在没有名字的情况下。	【还没看懂 Y组合子，智商不够了么】             
第10章 	讲解 实现一个 scheme 解释器。           




##　preface


recursive		递归


## 第1章 Toys

**list, atom, S-expression** 

讲了 list 和 atom 的区别

list 是由 () 括起来的。

atom 不是。 atom 左右两边可以是 字符，数字，特殊符号，但是就是不能两边正好是()

S-expression: list 和 atom 都属于 S-expression。

---



**car, cdr, cons** 

CONS, CAR, CDR，三个有爱的小伙伴。在Lisp/Scheme语言中，它们（某种程度上）构成了数据结构的基础。

┏━━┯━━━┓  
┃X │ Y┃  
┗━━┷━━━┛  

这个“盒子”由左右两部分组成，这样的结构也被称为点对（pair）。以 P 代表上面的结构，其左半部分是元素X，右半部分是元素Y。

car 的作用是取出一个点对的左半部分，当其作用在 P 上就会得到元素X。无独有偶，cdr （念作could-er）的作用是取出一个点对的右半部分。而cons的作用便是将两个元素组装成pair。于是便有如下的关系：

	(car P) = X

	(cdr P) = Y

	(cons X Y) = P

**You cannot ask for the car of an atom.** 但是可以对 list 提问。
就是 list 可以有car, 但是 atom 是没有的。
同样的：**You cannot ask for the car of the empty list.** 



###  **The Law of Car**
：The primitive car is defined only for non-empty lists. 

###  **The Law of Cdr**
：The primitive cdr is defined only for non-empty lists. The cdr of any non­-empty list is always another list.  

这样理解 cdr:就是把列表里 第1个元素砍掉。

a = peanut
l = (butter and jelly) 

cons a l = (peanut butter and jelly) 

cons adds an atom to the front of a list. 

其实可以这样 理解 cons: 就是把第1个 元素插入到 第2元素【必须是列表】中第1个位置。

这样看来 好像 cdr 和  cons 很像 逆向的操作。 一个砍掉列表的第1个元素，一个插入1个元素到列表头。

###  **The Law of Cons**
：The primitive cons takes two arguments. 

The second argument to cons must be a list. The result is a list. 

###  **The Law of Null? 
： The primitive null? is de­fined only for lists.** 

atom?

eq?		可以用来比较相同的元素atom,但是不能是数字。


###  **The Law of Eq?**：
The primitive eq? takes two ar­guments. Each must be a non­-numeric atom. 

### 五法总结

规则1：car 只对非空的列表有定义。

规则2：cdr 也只对非空的列表有定义，并且它总是返回一个列表。

规则3：cons 需要两个参数。第二个参数必须是列表，返回值也一定是列表。

规则4：Null?  只能对 lists 使用

规则5：Eq? 只接受2个非数字的元素。


## 第2章 Do It, Do It Again, and Again, and Again ...                                                      

[P30/211]

lat?

让我们来自己定义 一个函数 lat?

这个函数的功能：这是我们的描述
“lat? 查找列表中的每一个 S-expression表达式，看每一个 S-expression表达式是否是atom原子，直到没有 S-expression了。如果从头至尾没有遇到一个list表，那么返回#t，否则返回#f——false”

	(define  lat ?
		(lambda  (l) 
			(cond 
				(( null ?  l)  #t) 
				(( atom ?  (car  l))  (lat ?  (cdr  l))) 
				(else  #f )))) 

(cond  	 ...  )  asks questions; 
(lambda  ...  )  creates a function ; and 
(define  ...  )  gives it a name. 

(null? l) 查询参数l是否是null。如果是，则是真；否则为假，cond执行下一个查询。
else查询是什么意思？
else查询是否为真。

else是真吗？
是的，else总是真的。

**or ...)是做什么的**
(or ...)查询两个问题，每次一个。如果第一个是真那么停止并回答真。否则查询第二个问题并以第二个问题的回答作为答案。

( or  . . .  ) asks two questions, one at a time. If the first one is true it stops and answers true. Otherwise it asks the second question and answers with whatever the second question answers. 

**member? 函数**

下面是函数member?的定义

	(define member?
	  (lambda (a lat)
	    (cond
	      ((null? lat) #f)
	      (else (or (eq? (car lat) a)
	                (member? a (cdr lat)))))))

这章 学会了 2个 **递归函数** 的定义。

### 第一戒 
Always ask null? as the first question in expressing any function. 




## 第3章 3. Cons the Magnificent                              

[P48/211]

### 第二戒 
Use cons to build lists.    
使用cons构建list表  

**rember 函数**  

	(define rember
	    (lambda (a lat)
	        (cond
	       		((null? lat) '())
	       		((eq? (car lat) a) (cdr lat))
	       		(else (cons (car lat)
	                  (rember a (cdr lat)))))))

函数rember依次检查 lat 中的每一个原子是否与and相同，如果car lat与and并不相同，我们将它保存下来，稍后再cons到最终的值上面去。如果rember在lat中找到了and原子，则丢弃它。返回lat的剩余部分，然后再将先前暂存的原子cons到最终结果上去。


**firsts 函数**  
“firsts函数输入一个lists表参数，null空表，或者只包含非空表。它抽取list表中每一个成员的第一个 S-expression表达式构建新的list表。”

试着自己写一个

	(define firsts
	    (lambda (l)
	        (cond
	       		((null? l) (quote ()))
	       		((atom?  (car l))  '())
	       		(else (cons  (car (car l)) (firsts  (cdr l)))))))

 
### 第三戒 
When building a list, describe the first typical ele­ment, and then cons it onto the natural recursion.    
当构建list表时，描述第一个典型元素，然后把它cons到自然递归中去。


(else (cons (car (car l)) (firsts (cdr l)) ))
            ~~~~~~~~~~~~  ^^^^^^^^^^^^^^^^
              典型元素      自然递归


**insertR 函数**  

	( insertR new old lat ) 
	where 
		new is e 
		old is d 
	and 
		lat is  (a  b  c  d  f  g  d  h) 

结果就是：
	(a  b  c  d  e  f  g  d  h).

e 插入到 d 的后面了。
试着自己写一个

	(define  insertR 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons old (cons new (cdr lat))))
			       		(else (cons (car lat) (insertR new old (cdr lat))))
							)))))

4+5 = 9 个尾括号... 这个只能靠 编译器 帮助 来 减少错误了。

**insertL 函数**  
Hint: insertL inserts the atom new to the left of the first occurrence of the atom old in lat.

	(define  insertL 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons new lat))
			       		(else (cons (car lat) (insertL new old (cdr lat))))
							)))))

**subst  函数**  
Hint: (subst new old lat) replaces the first occurrence of old in the lat with new

	(define  subst 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons new (cdr lat)))
			       		(else (cons (car lat) (subst new old (cdr lat))))
							)))))

**subst2  函数** 
Hint: (subst2 new o1 o2 lat) replaces either the first occurrence of o1 or the first occurrence of o2 by new.  
同时有2个 old 元素, 哪个先出现, 哪个就被替换.

	(define  subst2 
		(lambda  (new  o1 o2  lat ) 
			(cond
				((null? lat) (quote()))
				(else (cond 
			       		((or (eq? (car lat) o1) (eq? (car lat) o2)) (cons new (cdr lat)))
			       		(else (cons (car lat) (subst2 new o1 o2 (cdr lat))))
								)))))
**multirember  函数** 

	(define multirember
	    (lambda (a lat)
	        (cond
	       		((null? lat) (quote()) )
					(else (cond 
		       		((eq? (car lat) a) (multirember a (cdr lat)))
		       		(else (cons (car lat) (multirember a (cdr lat))))
							)))))

**multiinsertR  函数** 

	(define  multiinsertR 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons old (cons new (multiinsertR new old (cdr lat)) )))
			       		(else (cons (car lat) (multiinsertR new old (cdr lat))))
							)))))


**multiinsertL 函数**  

	(define  multiinsertL 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons new (cons old (multiinsertL new old (cdr lat))) ))
			       		(else (cons (car lat) (multiinsertL new old (cdr lat))))
							)))))


### 第四戒

Always change at least one argument while recurring. It must be changed  to be closer to  termination.  The changing 
argument must be tested in the termination condition: when using cdr, test termination with null?. 
递归时至少要有一个参数变化，并且向终止条件方向变化。变化的参数必须有终止测试条件：当时用cdr时，用null?测试终止。

**multisubst 函数** 

	(define  multisubst 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else
					(cond 
			       		((eq? (car lat) old) (cons new (multisubst new old (cdr lat)) ))
			       		(else (cons (car lat) (multisubst new old (cdr lat))))
							))))) 

这个可以简化的,不用写出来2个  (multisubst new old (cdr lat))

好吧,写了个简化的,也不知道行不行.书上没有这个答案.
	(define  multisubst 
		(lambda  (new  old  lat ) 
			(cond
				((null? lat) (quote()))
				(else (cons 
							(cond 
								((eq? (car lat) old)  new) 
								(else (car lat))
							)
							(multisubst new old (cdr lat)) 
				))
	))) 


## 第4章 4. Numbers Games  

[P74/211]

本书不考虑 非负数,所有的 数学 运算如果 产生 负数 就给予 No answer. 这个结果

Try to  write the function +  

	(define +  1   
		(lambda  (  n  m)   
			(cond   
				((zero?  m)  n)   
				(else  (add1  (+ n  (sub1  m)))))))   

If zero?   is  like  null?   [第1诫律 说第一询问 先 询问 是不是 null,在数字 运算中 就先 询问是不是 zero ]
is  add1  like  cons

Try to  write the  function  -Hint :  Use  sub1 

	(define - 1 
		(lambda  ( n  m) 
			(cond 
				((zero?  m)  n) 
				(else  (sub1  ( - n  (sub1  m))))))) 

            
### tuple 元祖

一堆数字的组合 就是 元祖.

是 tup 的:
(2  11  3  79  47  6) 
() 


不是 tup 的:
(1  2  8  apple 4  3)   list
(3 (7  4)  13 9 ) 





### 第1戒(第1次修订)

When recurring on a list of atoms, lat , ask two questions about it: (null? lat ) and else. 
When recurring on a number, n, ask two questions about it:  (zero? n) and else. 

当递归atom原子，lat时，两个查询：(null? lat) 和 else
当递归数n时,两个查询：(zero? n) 和else


cons 是做什么的?   

	构建list表

addtup 是做什么的?

	从tup的所有数成员构建一个总和数。

addtup 的终止条件

	((null? tup) 0)

**addtup 的定义**

	(define addtup
	  	(lambda (tup)
	    	(cond
	      		((null? tup) 0)
	      		(else (+ (car tup) (addtup (cdr tup)))) 
			)))


### 第4戒(第1次修订)

Always change at least one argument while recurring. It must be changed to be closer to termination. The changing argument must be tested in the termination condition: when using cdr, test termination with null?  and when using sub1, test termination with zero?. 

递归时至少要有一个参数变化，并且向终止条件方向变化。变化的参数必须有终止测试条件：
当用cdr时，用null? 做终止测试条件
当用sub1，用zero? 做终止测试条件


函数X[不就是乘法么]

	(define x
	 	(lambda n m
	   		(cond
	     		((zero? m) 0)
	     		(else (+ n (x n (sub1 m)))))))

### 第5戒

When building a value with + , always use 0 for the value of the terminating line, for adding 0 does not change the value of an addition. 
When building a value with x,  always use 1 for the value of the terminating line, for multiplying by 1 does not change the value of a multiplication. When building a value with cons, always consider () for the value of the terminating line. 

当用+构建一个值时，使用0作为终止值，因为0不改变加法的值。
当用x构建一个值时，使用1作为终止值，因为1不改变乘法的值。
当用cons构建list表，终止条件是()


**tup+ 的定义**

tup1是(3 6 9 11 4)，tup2是(8 5 2 0 7)，问(tup+ tup1 tup2)是什么?

	(11 11 11 11 11)

(tup+ tup1 tup2)做什么?

	把所有tup1和tup2的元素一一对应加，tup的长度相同。

tup+ 有什么特殊之处?

	每次查找两个tup的元素，即每次递归都是处理两个元素。

写出函数tup+ , 只接受 2个 相同长度的 参数

	(define tup+
	  (lambda (tup1 tup2)
	    (cond
	      ((and (null? tup1) (null? tup2))
	       (quote ()))
	      (else
	        (cons (+ (car tup1) (car tup2))
	           (tup+
	             (cdr tup1) (cdr tup2)))))))

另一个版本的tup+函数，与上面的版本不同之处在于可以接受两个长度不同的tup作为参数。

		(define tup+
		  (lambda (tup1 tup2)
		    (cond
		     ((null? tup1) tup2)
		     ((null? tup2) tup1)
		     (else
		      (cons (+ (car tup1) (car tup2))
		            (tup+
		             (cdr tup1) (cdr tup2)))))))

大于号 的定义

	(define >
	 (lambda (n m)
	   (cond
	     ((zero? n) #f)
	     ((zero? m) #t)
	     (else (> (sub1 n) (sub1) m)))))

小于号 的定义 

	(define <
	 (lambda (n m)
	   (cond
	     ((zero? m) #f)
	     ((zero? n) #t)
	     (else (< (sub1 n) (sub1) m)))))

= 的定义, 下面这定义,我没看懂啊.

     (define =
	  (lambda (n m)
	    (cond
	      ((zero? m) (zero? n))
	      ((zero? n) #f)
	      (else (= (sub1 n) (sub1) m)))))   

用函数<和函数>重写函数=

	(define =
	 (lambda (n m)
	   (cond
	     ((> n m) #f)
	     ((< n m) #f)
	     (else #t))))

就是说我们有两个函数来比较atom原子是否有相等对么?

	对。用 = 查询数，用 eq? 查询其它的。

写出函数^ 

	(define ^
	 (lambda (n m)
	   (cond
	     ((zero? m) 1)
	     (else (x n (^ n (sub1 m)))))))

除法的定义

	(define ÷
	 (lambda (n m)
	   (cond
	     ((< n m) 0)
	       (else (add1 (÷ (- n m) m))))))

**length 定义**  
(length lat) 的值是什么? 确定 lat 中 元素的个数.

	(define length
	 	(lambda (lat)
			(cond
				((null? lat) 0)
				(else (add1 (length (cdr lat))))
			)))

**pick 定义**  
(pick n lat) 是什么, 返回 lat 中第 n 个元素.

	(define pick
	 	(lambda (n lat)
			(cond
				((zero? (sub1 n)) (car lat))
				(else (pick (sub1 n) (cdr lat)))
			)))

**rempick 定义**  
(rempick n lat) 是什么，移除 lat 中第 n 个元素.

	(define rempick
	 	(lambda (n lat)
			(cond
				((zero? (sub1 n)) (cdr lat))
				(else cons((car lat) (rempick (sub1 n) (cdr lat))))
			)))

**number? 定义** 

(number? a)

你能写出 number?函数吗?

	不能。number? 如同add1, sub1, zero?, car, cdr, cons, null?, eq?, 以及atom?, 都是元函数

现在使用number?函数写一个no-nums，它删除一个lat中的所有出现的数。

**no-nums 定义** 

	(define no-nums
	 	(lambda (lat)
			(cond
				((null? lat) (quote ()))
				(else (cond
					((number? (car lat)) (no-nums? (cdr lat)))
					(else (cons (car lat) (no-nums? (cdr lat))))
				)))))


现在写一个函数 all-nums，它从一个lat中抽取出所有的数构成一个tup。

**all-nums 定义** 

	(define all-nums
	 	(lambda (lat)
			(cond
				((null? lat) (quote ()))
				(else (cond
					((number? (car lat)) (cons (car lat) (all-nums (cdr lat))))
					(else (all-nums (cdr lat)))
				)))))

写一个函数eqan?，当两个参数a1和a2是一样的原子时为真。

**eqan? 定义** 

这个待写.

---  

现在写一个函数occur描述一个lat中出现原子a的次数

**occur 定义** 

	(define occur
	 	(lambda (a lat)
			(cond
				((null? lat) 0)
				(else (cond
					( (eq? (car lat) a) (add1 (occur a (cdr lat)))) 
					(else (occur a (cdr lat)))
				)))))

写一个函数one?仅当n为1时(one? n)为#t真，否则为#f假

**one? 定义** 
	
	(define one?
	 	(lambda (n)
			(cond
				((zero? n) #f)
				(else (zero? (sub1 n))) )))

现在重写rempick，它删去 lat 中的第 n 个atom原子。你可以使用你自己的one?函数。

**rempick 定义**  
(rempick n lat) 是什么，移除 lat 中第 n 个元素.

	(define rempick
	 	(lambda (n lat)
			(cond
				((one? n)) (cdr lat))
				(else cons((car lat) (rempick (sub1 n) (cdr lat))))
			)))

## 第5章 5. *Oh My Gawd* : It's Full of Stars                                           

[P96/211]

**rember 函数**  

	(define rember
	    (lambda (a lat)
	        (cond
	       		((null? lat) '())
	       		((eq? (car lat) a) (cdr lat))
	       		(else (cons (car lat)
	                  (rember a (cdr lat)))))))

**rember* 函数**  

	(define rember*
	  (lambda (a l)
	    (cond
			((null? l) (quote()))
			( (atom? (car l))
				( cond
					((eq? (car l) a) (rember* a (cdr l)))
				(else (cons (car l) (rember* a (cdr l)))) ))
			(else (cons (rember* a (car l)) (rember* a (cdr l)))) )))



**insertR* 函数**  

	(define insertR*
	  (lambda (new old l)
	    (cond
			((null? l) (quote()))
			( (atom? (car l))
				( cond
					((eq? (car l) old)  (cons (cons old new) (insertR* new old (cdr l))))
				(else (cons (car l) (insertR* new old  (cdr l)))) ))
			(else (cons (insertR* new old (car l)) (insertR* new old (cdr l)))) )))



### 第一戒 最终版  

When recurring on a list of atoms, lat, ask two questions about it:  (null?  lat) and else. 
When recurring on a number, n, ask two questions about it:  (zero?  n)  and else. 
When recurring on a list of S-expressions, l, ask three question about it:  (null? l),  (atom? (car l)), and else. 

当在一个由原子构成的列表(lat)上递归时，问两个问题：(null? lat) 还有 else.
当在一个数字上递归时，问两个问题：(zero? n) 还有 else.
当在一个由S-表达式构成的列表（复合列表）上递归时，问三个问题：(null? l), (atom? (car l)),还有 else.


### 第四戒 最终版  

递归时至少要有一个参数变化，并且向终止条件方向变化。变化的参数必须有终止测试条件：当递归原子(lat)时使用(cdr lat)。当递归数n时使用(sub1 n)。当递归一个 S-expression的列表l,当(null? l)或者(atom? (car l))使用(car l)和(cdr l)。
递归时参数要向终止条件方向变化：当使用cdr时，用null?测试终止；
当使用sub1时，用zero?测试终止。



**occur* 函数**  

	(define rember*
	  (lambda (a l)
	    (cond
			((null? l) 0)
			( (atom? (car l))
				( cond
					((eq? (car l) a) (add1 (occur* a (cdr l))))
				(else (occur* a (cdr l))) ))
			(else (+ (occur* a (car l)) (occur* a (cdr l)))) )))


**subst* 函数**  

	(define subst*
	  (lambda (new old l)
	    (cond
			((null? l) (quote()))
			( (atom? (car l))
				( cond
					((eq? (car l) old)  (cons new (subst* new old (cdr l))))
				(else (cons (car l) (subst* new old  (cdr l)))) ))
			(else (cons (subst* new old (car l)) (subst* new old (cdr l)))) )))



**insertL* 函数**  

	(define insertL*
	  (lambda (new old l)
	    (cond
			((null? l) (quote()))
			( (atom? (car l))
				( cond
					((eq? (car l) old)  (cons (cons new old) (insertL* new old (cdr l))))
				(else (cons (car l) (insertL* new old  (cdr l)))) ))
			(else (cons (insertL* new old (car l)) (insertL* new old (cdr l)))) )))


**member* 函数**  

	(define member*
	  (lambda (a l)
	    (cond
			((null? l) #f)
			((atom? (car l))
				(or (eq? (car l) a) (member* a (cdr l))
			(else (or (member* a (car l)) (member* a (cdr l)))) )))


Here is our description : 
"The function leftmost finds the leftmost atom in a non-empty list of S-expressions that does not contain the empty list."   
“函数 leftmost 查找 S-expression 表达式中非空 list 中的的最左边的一个原子。  

	(define leftmost*
	  (lambda (l)
	    (cond
			((atom? (car l)) (car l))
			(else (leftmost* (car l))) )))


(or ...) 一次查询一个直到出现真然后停下来，返回真。如果找不到真，那么(or ...)的值是假。

(and ...)一次查询一个，直到出现否，然后返回假。如果总找不到假的表达式，(and ...)的值为真。

	(define eqlist*
	  (lambda (l1 l2)
	    (cond
			((and (null? l1) (null? l2)) #t)
			((or (null? l1) (null? l2)) #f)
			(else
				(and (equal? (car l1) (car l2)))
					(eqlist* (cdr l1) (cdr l2)))) )))


### 第六戒  
Simplify  only  after the  function  is  correct.  
仅当函数正确后再简化  


## 第6章 6. Shadows

[P112/211]

我们这样描述
“对于这一章，算术表达式可以是atom原子(包括数)，或者由+，×，或者^连接的两个算术表达式。”


numbered? 是什么  
一个函数，查询一个算术表达式是否只包含有在 + ，× ，和 ^ ，及其后边的数。  

  
	(define numbered?
	 (lambda (aexp)
	   (cond
	     ((atom? aexp) (number? aexp))
	     ((eq? (car (cdr aexp)) (quote +))
	      (and (numbered? (car aexp))
	           (numbered? (car (cdr (cdr aexp))))))
	     ((eq? (car (cdr aexp)) (quote ×))
	      (and (numbered? (car aexp))
	           (numbered? (car (cdr (cdr aexp))))))
	     ((eq? (car (cdr aexp)) (quote ^))
	      (and (numbered? (car aexp))
	           (numbered? (car (cdr (cdr aexp)))))))))

写个再简单点版本。

我自己写的简化版本：

	(define numbered?
	 (lambda (aexp)
	   (cond
	     ((atom? aexp) (number? aexp))
	      ((or (eq? (car (cdr aexp)) (quote +) (eq? (car (cdr aexp)) (quote ×) (eq? (car (cdr aexp)) (quote ^)) )
	      (and (numbered? (car aexp))
	           (numbered? (car (cdr (cdr aexp)))))))))

书上给的简化版本：

	(define numbered?
	 (lambda (aexp)
	   (cond
	     ((atom? aexp) (number? aexp))
	     (else (and (numbered? (car aexp)) 
	                (numbered? (car (cdr (cdr aexp)))))))))	



### 第七戒
Recur on the subparts that are of the same nature: 
•  On the sublists of a list. 
•  On the sub expressions of an arithmetic expression.

在相同本性的东西上递归子组成部分：
*list表
*算术表达式

算术表达式的表达的第1个子表达式

	(define 1st-sub-exp
	 (lambda (aexp)
	   (car (cdr aexp))))

算术表达式的表达的第2个子表达式

	(define 2nd-sub-exp
	 (lambda (aexp)
	   (car (cdr (cdr aexp)))))

算术表达式的 操作符

	(define  operator
	 (lambda (aexp)
	   (car aexp)))

现在再一次重写函数value

	(define value
	 (lambda (nexp)
	   (cond
	    ((atom? nexp) nexp)
	    ((eq? (operator nexp) (quote +))
	     (+ (value (1st-sub-exp nexp))
	        (value (2nd-sub-exp nexp))))
	    ((eq? (operator nexp) (quote x))
	     (x (value (1st-sub-exp nexp))
	        (value (2nd-sub-exp nexp))))
	    (else
	     (^ (value (1st-sub-exp nexp))
	        (value (2nd-sub-exp nexp)))))))


### 第八戒

Use  help functions  to  abstract  from  representations.   
使用辅助函数来抽象表达   

还记得我们有多少个对数使用的元函数吗？

四个：numbers? zero? add1和sub1

本章后面这些内容：
用 ()代表0， (()) 代表1， ((())) 代表2， 没看懂什么意思。
还可以用 ()代表0， (()) 代表1， (()()) 代表2， (()()()) 代表3

就是说可以随便来定义数字么。

通过这种嵌套关系或者并列关系，可以随便来定义？


## 第7章 7. Friends and Relations                               

[P126/211]

关系型数据库？

这是一个set集合吗？
 (apple peaches apple plum)

不是，apple出现了不止一次

	(define set?
	 (lambda (lat)
	   (cond
	     ((null? lat) #t)
	     ((member? (car lat) (cdr lat)) #f)
	     (else (set? (cdr lat))))))

(makeset lat)是什么，其中lat是
(apple peach pear peach plum apple lemon peach)

(apple peach pear plum lemon)

试试用member?写出函数makeset **makeset 去掉列表中重复的元素，生成一个集合**

	(define makeset
	 (lambda (lat)
	   (cond
	     ((null? lat) (quote ()))
	     ((member? (car lat) (cdr lat))
	      (makeset (cdr lat)))
	     (else (cons (car lat)
	                  (makeset (cdr lat)))))))

(subset? set1 set2)
是什么，其中
set1 是(5 chicken wings)
set2 是(5 hamburgers 2 pieces fried chicken and light duckling wings)

 	#t，因为每一个set1中的原子也在set2中。

试着写出来定义  

	(define subset?
	 (lambda (set1 set2)
	   (cond
	     ((null? set1) #t)
	     ((member? (car set1) set2)
	      (subset? (cdr set1) set2))
	     (else #f))))


(eqset? set1 set2)
是什么，其中
set1 是(6 large chickens with wings)，
set2 是(6 chickens with large wings)

	#t

试着写出来定义  

	(define eqset?
	 (lambda (set1 set2)
	   (and (subset? set2 set1) (subset? set1 set2))
	    ))

但这样根本无法保证 set1 和 set2 一定是 集合啊。

(intersect? set1 set2)
是什么，其中
set1 是 (stewed tomatoes and macaroni)
set2 是 (macaroni and cheese)

	#t，因为set1至少有一个原子出现在set2中。交集

试着写出来定义 

	(define intersect?
		 (lambda (set1 set2)
			(cond
				((null? set1) #f)
		   (else (or (member? (car set1) set2) (intersect? (cdr set1) set2)))
		    )))


(intersect set1 set2)是什么，其中
set1 是(stewed tomatoes and macaroni)
set2 是(macaroni and cheese)

	(and macaroni)

试着写出来定义 **intersect 两个列表的交集**
	
	(define intersect
	 (lambda (set1 set2)
	   (cond
	     ((null? set1) (quote()))
	     ((member? (car set1) set2) (cons (car set1) (intersect (cdr set1) set2)))
		(else (intersect (cdr set1) set2)) 
	     )))

(union set1 set2)
是什么，其中
set1 是(stewed tomatoes and macaroni casserole)
set2 是(macaroni and cheese)

	(stewed tomatoes casserole macaroni and cheese)

试着写出来定义  **union 两个列表的并集**

[P131/211]

	(define union
	 (lambda (set1 set2)
	   (cond
	     ((null? set1) set2)
	     ((member? (car set1) set2)
	      (union (cdr set1) set2))
	     (else (cons (car set1)
	                 (union (cdr set1) set2))))))


“这个函数返回的是set1中有，但是set2中没有的原子。

	(define xxx
	  (lambda (set1 set2)
	    (cond
	     ((null? set1) '())
	     ((member? (car set1) set2)
	      (xxx (cdr set1) set2))
	     (else
	      (cons (car set1)
	            (xxx (cdr set1) set2))))))	


(intersectall l-set)
是什么，其中
l-set 是 ((a b c) (c a d e) (e f g h a b))

	(a)

它返回在l-set的所有子set中都有的原子。

试着写出来定义 **intersectall 所有列表的交集**

	(define intersectall
	  (lambda (l-set)
	    (cond
	     ((null? (cdr l-set))  (car l-set))
	     ((intersect (car l-set) (intersectall (cdr l-set)))
		)))

 
(a-pair? l)
其中 l 是
(full (house))

	#t。 因为列表中只有两个表达式。

定义函数 a-pair?
	
	(define a-pair?
	 (lambda (x)
	   (cond
	     ((atom? x) #f)
	     ((null? x) #f)
	     ((null? (cdr x)) #f)
	     ((null? (cdr (cdr x))) #t)
	     (else #f))))

取得一个piar的第一个 S-expression表达式

	(define first
	  (lambda (p)
	    (car p)))

取得一个piar的第二个 S-expression表达式
	
	(define second
	  (lambda (p)
	    (car (cdr p))))

怎样用两个原子构建一个 pair
	
	(define build
	  (lambda (s1 s2)
	    (cons s1 (cons s2 (quote ())))))

rel: pair对的一个的集合

(revrel rel)
是什么，其中 l 是
(8 a) (pumpkin pie) (got sick))

	((a 8) (pie pumpkin) (sick got))

从 pair对的集合中每个列表 抽取第1个元素，生成一个了列表，看这个列表是否是一个set

	(define fun?
	 (lambda (rel)
	   (set? (firsts rel))))

好吧，上面这个函数没有看懂，为啥可以生成一个集合。

试着写出来定义 **revrel **

	(define revrel
	 (lambda (rel)
	   (cond
	    ((null? rel) quote ())
	    (else (cons (build
	            (second (car rel))
	            (first (car rel)))
	           (revrel (cdr rel)))))))

假设我们有如下函数revpair可以反转一个piar对的两个成员。

	(define revpair
	  (lambda (pair)
	    (build (second pair) (first pair))))

如何用这个辅助函数重写revrel

	(define revrel
	 (lambda (rel)
	   (cond
	    ((null? rel) quote ())
	    (else (cons (revpair (car rel))
	           (revrel (cdr rel)))))))

从 pair对的集合中每个列表 抽取第2个元素，生成一个了列表，看这个列表是否是一个set

	(define fullfun?
	 (lambda (fun)
	   (set? (seconds fun))))


## 第8章 8. Lambda the Ultimate                             

[P140/211]

(rember-f test? a l)
是什么，其中
test? 是 =
a 是 5
l 是 (6 2 5 3)

	(6 2 3)

	注： Lisp:(rember-f (function =) 5 '(6 2 5 3))

试着写出函数 rember-f

	(define rember-f
	 (lambda (test? a l)
	   (cond
	    ((null? l) (quote ()))
	    ((test? (car l) a) (cdr l))
	    (else (cons (car l)
	                (rember-f test? a
	                          (cdr l)))))))

换句话， test? 这个函数 被当作 一个参数 传递给了函数 rember-f

现在重写 rember-f 为一个参数test?的函数，返回一个如同用eq?替换test?的rember。

	(define rember-f
	 (lambda (test?)
	   (lambda (a l)
	     (cond
	       ((null? l) (quote ()))
	       ((test? (car l) a) (cdr l))
	       (else (cons (car l) 
	               ((rember-f test?) a (cdr l))))))))

仔细体会 上下两个 函数的差别，在最后一行的地方。
一个是接收3个参数，一个是接收一个函数。
然后返回值也不同。

[P142/211]

本章的剩下页数 没有杂细看了，以后再仔细看，感觉就是 函数可以作为参数和返回结果罢了。
























## 第9章 9. ... and Again, and Again, and Again, ...       


讲 total function 和 partial function

就是非自然的递归。



### Y Combinator - Y 组合子

一句话解释： Y Combinator 用于计算（高阶）函数的不动点，使得lambda演算可以定义匿名递归函数。
>  
http://www.zhihu.com/question/20115649

**JavaScript(ES6) 实现** - 重新发明 Y 组合子 JavaScript(ES6) 版
>  
http://picasso250.github.io/2015/03/31/reinvent-y.html

**scheme 实现** - Reinventing the Y combinator - 王垠 写的
>  
http://www.slideshare.net/yinwang0/reinventing-the-ycombinator

**Python 实现** - Y Combinator in Python 
>  
http://air.googol.im/2007/08/31/y-combinator-in-python.html

### Fixed-point combinator，不动点组合子

其他教材  
>   
https://zh.wikipedia.org/wiki/%E4%B8%8D%E5%8A%A8%E7%82%B9%E7%BB%84%E5%90%88%E5%AD%90

魂断不动点——Y组合子的前世今生  
>   
http://deathking.github.io/2015/03/21/all-about-y-combinator/


这个函数有名字吗？

有，它被称为Y 算子(Y combinator)。

	(define Y
	 (lambda (le)
	   ((lambda (f) (f f))
	    (lambda (f)
	      (le (lambda (x) ((f f) x)))
		))))

---

一步一步讲解Y组合子 (Y-Combinator Explained Step by Step) 
[作者最后一句:"接下来的推导就比较困难了，我现在还没能完全弄清楚怎么到我们常见的最终形式。" 简直想让我吐血啊!]
>  
http://www.cppblog.com/wuwu/archive/2014/07/07/207556.aspx  

Y组合子常见的最终形式是：  
Y = λf.(λx.f (x x)) (λx.f (x x))  
用Scheme写出来则是：   

	(define y-combinator
	  (lambda (f)
	    ((lambda (x) (f (lambda (y) ((x x) y))))
	     (lambda (x) (f (lambda (y) ((x x) y)))))
		))

这个最终形式的Y组合子可以工作在非Lazy的正确实现的Scheme里。  


看不懂啊,看不懂啊!!!  到第四点-不动点 这里就看不懂了!!!!

---

scheme下的停机问题和Y组合子
>  
http://www.cppblog.com/huaxiazhihuo/archive/2013/07/13/201689.html

回家仔细看.




                                           