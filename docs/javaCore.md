[1. Lock和synchronized的区别？](#1)

[2. 锁的分类](#2)

[3. wait（）和sleep（）的区别？](#3)



### <span id="1">1. Lock和synchronized的区别?</span>

​	（1）synchronized遇到异常时自动释放锁，lock不会（所以需要在finally中lock.unlock释放锁）

​	（2）lock可以指定等待时间（lock.trylock(5,TimeUnit.SECOND)）

​	（3）synchronized是java关键字 lock是一个类



### <span id="2">2.锁的分类？</span>

​	（1）可重入锁：一个synchronized方法调用另外一个synchronized方法时，不需要重新申请锁（synchronized ReentLock）

​	（2）可中断锁：获取锁的过程可以被中断（lock 是可中断的lockInterrutibly（） synchronized不可被中断）

​	（3）公平锁：以请求顺序获取锁 等待时间最久的线程获得锁（synchronized是非公平锁，ReentrantReadWriteLock 默认非公平锁，但可设为公平锁new reentlock（true））

​	（4）读写锁：将对一个资源的访问分成两个锁，一个读锁，一个写锁 通过readLock（）获取读锁，writeLock（）获取写锁

​	（5）悲观锁：每次拿数据的时候认为别人都会修改，所以都会上锁（synchronized）

​	（6）乐观锁：每次拿数据的时候认为别人不会修改，所以不上锁（concurrnet.automic就是使用了乐观锁的一种实现方式CAS实现的）CAS：多个线程同时操作一个变量时，只有一个线程能修改这个变量，其他线程修改失败并不被挂起，而是可以继续尝试。



### <span id="3">3. wait()和sleep()的区别？</span>

​	（1）sleep（）是Thread类的方法，wait（）是Object类的方法

​	（2）sleep（）不释放锁对象，wait（）释放锁对象

​	（3）wiat（） notify（）只能在同步方法或同步代码块中使用，sleep（）可以在任何地方使用



### <span id="4">4. 浅克隆和深克隆的区别？</span>

​	（1）浅克隆：创建一个新对象，对于非基本类型属性，仍指向原属性所指向的内存地址

​	（2）深克隆：创建一个新对象，非基本类型属性也不指向原属性指向的内存地址

​	（clone（）是浅克隆，如何实现深克隆？实现cloneable接口，重写clone（），让它的引用类型属性调用clone（）方法）





















































