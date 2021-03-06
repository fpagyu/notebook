## 锁

### 锁的分类

锁从宏观上分为乐观锁和悲观锁

**乐观锁**
乐观锁认为读多写少， 遇到并发写的可能性低， 每次去拿数据的时候都认为别人不会修改，所以不会上锁， 但是在更新的时候会判断一下在此期间别人有没有更新这个数据，采取在写时先读出当前版本号，然后加锁操作(), 如果失败则需要重复读-比较-写的操作。
java中的乐观锁基本都是通过CAS操作实现的，CAS是一种更新的原子操作，比较当前值跟传入值是否一样， 一样则更新， 否则失效。

**悲观锁**
悲观锁认为写多，遇到并发写的可能性高，每次去拿数据的时候都认为别人会修改，所以每次在读写数据的时候都会上锁，这样别人想读写这个数据就会block直到拿到锁。java中的悲观锁就是Synchronized

**公平锁/非公平锁**
公平锁和非公平锁的区别在于，公平锁是FIFO机制，谁先来谁就在队列的前面，就能优先得到锁。非公平锁支持抢占模式， 先来的不一定能得到锁.

**CAS(Compare and Swap)**

CAS就是根据乐观锁的设计思想来实现的，在取数据的时候， 判断一下在此期间是否有人修改， 如果没有修改， 则直接使用.

CAS原理: CAS有三个操作数， 即内存值v, 旧的操作数a, 新的操作数b。当我们需要更新v值为b时， 首先我们判断v值是否和我们之前的所见值a相同，若相同则将v值赋值给b，若不同则什么也不做。是一种非阻塞算法，在java中可以通过锁和循环CAS的方式来实现原子操作。

CAS自旋锁适用于锁保持者保持锁时间比较短的情况中，因为自旋锁使用者一般保持锁的时间很短， 所以才选择自旋而不是睡眠。

