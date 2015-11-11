


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





