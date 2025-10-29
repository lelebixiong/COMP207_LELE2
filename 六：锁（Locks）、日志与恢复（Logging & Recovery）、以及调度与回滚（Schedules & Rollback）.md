🧩 Exercise 1：锁（Locks）与两段锁协议（2PL）
我们有两个事务,
T1:
read(X);  
read(Y);  
Y := X + 2Y;  
write(Y);

T2:
read(Y);  
read(X);  
X := Y + 2X;  
write(X);

要求：

1. 给这两个事务加上 **共享锁（S）**、**独占锁（X）** 和 **解锁（unlock）** 操作，让它们满足**两段锁协议（2PL）**。
	     - 阶段 1：事务可以不断地申请锁（S 或 X）
	     - 阶段 2：一旦释放了一个锁，就不能再申请新锁

2. 找出一种加锁方式，使得**某些调度会发生死锁**。
	🔒 死锁情况（示例）
	**T1**：
		 S-lock(X);
		 read(X);   
		 S-lock(Y);
		 read(Y);
		 X-lock(Y);
		 Y := X + 2Y;
		 write(Y);
		 unlock(Y);
  		 unlock(X);   
  	**T2**：
	  	 S-lock(Y);
	  	 read(Y);
	  	 S-lock(X);
	  	 read(X);
	  	 X-lock(X);
	  	 X := Y + 2X;
	  	 write(X);
	  	 unlock(X);
	  	 unlock(Y);
🔁 **为什么会死锁：**
- T1 先锁了 X，然后等 Y
- T2 先锁了 Y，然后等 X  
    👉 互相等待，形成死锁

2. 再找出另一种加锁方式，使得**所有调度都不会死锁**。
我们只需要让它们按照**相同顺序加锁**，例如先锁 `X` 再锁 `Y`：
**T1**：
	 S-lock(X);-- 给 X 加共享锁（读）
	 read(X);
	 S-lock(Y);-- 给 Y 加共享锁（读）
	 read(Y);
	 X-lock(Y); -- 写 Y 前要加排他锁
	 Y := X + 2Y;
	 write(Y);
	 unlock(Y);
	 unlock(X);
T2:
	S-lock(X); -- 先锁 X（顺序和 T1 一样）
	read(X); 
	S-lock(Y); -- 再锁 Y（顺序一致）
	read(Y); 
	X-lock(X); -- 写 X 前加排他锁
	X := Y + 2X; 
	write(X); 
	unlock(Y); 
	unlock(X); 
谁先拿完锁谁先跑，重点：等待是允许的，死锁才是我们要避免的。


🧩 Exercise 2：日志与恢复（Undo Logging）
**Undo 日志的规则：*
- 如果事务**没有 commit**，就要把它的更改 **撤销（undo）**。
- 如果事务**已经 commit**，就要确保它的更改 **都落盘（redo）**。


日志顺序如下：
	<START T1> 
	<T1,X,10> T1 修改了 X，原值是 10
	<START T2> ---
	<T2,Y,20> 
	<T1,Z,30> 
	<T2,U,40> 
	<COMMIT T2> ---
	<T1,V,50> ---
	<COMMIT T1> ---
![[Pasted image 20251017143248.png]]
