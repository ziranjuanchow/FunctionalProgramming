


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

规则1：car 只对非空的列表有定义。

规则2：cdr 也只对非空的列表有定义，并且它总是返回一个列表。

规则3：cons 需要两个参数。第二个参数必须是列表，返回值也一定是列表。

**The Law of Car**：The primitive car is defined only for non-empty lists. 

**The Law of Cdr**：The primitive cdr is defined only for non-empty lists. The cdr of any non­-empty list is always another list.  

这样理解 cdr:就是把列表里 第1个元素砍掉。

a = peanut
l = (butter and jelly) 

cons a l = (peanut butter and jelly) 

cons adds an atom to the front of a list. 

其实可以这样 理解 cons: 就是把第1个 元素插入到 第2元素【必须是列表】中第1个位置。

这样看来 好像 cdr 和  cons 很像 逆向的操作。 一个砍掉列表的第1个元素，一个插入1个元素到列表头。

**The Law of Cons**：
The primitive cons takes two arguments. 
The second argument to cons must be a list. The result is a list. 

**The Law of Null? ： The primitive null? is de­fined only for lists.** 

atom?

eq?		可以用来比较相同的元素atom,但是不能是数字。


**The Law of Eq?**：
The primitive eq? takes two ar­guments. Each must be a non­-numeric atom. 


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

(null? l) 查询参数l是否是null。如果是，则是真；否则为假，cond执行下一个查询。


else查询是什么意思？
else查询是否为真。

else是真吗？
是的，else总是真的。

P31 ～36 /211

### 第一戒 





