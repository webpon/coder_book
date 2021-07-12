# 计算机基础

## 计算机组成原理

## 操作系统

## 汇编语言

## C语言

## 数据结构与算法

### 数据结构

#### 简介

> 数据结构是相互之间存在一种或多种特定关系的数据元素的集合

#### 栈（stack)

> 栈是一种操作受限的线性表只允许从一端插入和删除数据。
>
> 栈有两种存储方式，即线性存储和链接存储（链表）。栈的一个最重要的特征就是栈的插入和删除只能在栈顶进行，所以每次删除的元素都是最后进栈的元素，故栈也被称为后进先出（LIFO）表。每个栈都有一个栈顶指针，它初始值为-1，且总是指向最后一个入栈的元素，栈有两种处理方式，即进栈（push）和出栈（pop），因为在进栈只需要移动一个变量存储空间，所以它的时间复杂度为O(1)，但是对于出栈分两种情况，栈未满时，时间复杂度也为O(1)，但是当栈满时，需要重新分配内存，并移动栈内所有数据，所以此时的时间复杂度为O(n)。以下举例栈结构的两种实现方式，线性存储和链接存储。

##### javascript栈的实现（线性存储）

```
 //封装栈类
    function Stack() {
      //栈中的属性
      this.items = []
      //栈的相关操作
      //  1、将元素压入栈
      Stack.prototype.push = function (element) {
        this.items.push(element)
      }
      // 2、从栈中取出元素
      Stack.prototype.pop = function () {
        return this.items.pop()
      }
      //3、查看栈顶元素
      Stack.prototype.peek = function () {
        return this.items[this.items.length - 1]
      }
      // 4、判断栈是否为空
      Stack.prototype.isEmpty = function () {
        return this.items.length == 0
      }
      // 5、获取栈中元素的个数
      Stack.prototype.size = function () {
        return this.items.length
      }
      // 6、toString方法
      Stack.prototype.toString = function () {
        let resultString = ""
        for (let i = 0; i < this.items.length; i++) {
          resultString += this.items[i] + ' '
        }
        return resultString
      }
    }
    //栈的使用
    var s = new Stack()
    s.push('2')
    s.push('3')
    s.push('4')
    console.log(s);  
    console.log(s.size()); //3
    console.log(s.peek()); //4
    console.log(s.isEmpty()); //false
```

##### 栈的应用

**1、把十进制数转为二进制数**

​		要把十进制转化成二进制，我们可以将十进制数字和2整除(向下取整)（二进制是满二进一），直到结果是0为止。

​		举个例子，把十进制的数字10转化成二进制的数字，过程大概是这样：

​		<img src="https://i.loli.net/2021/02/27/XLHYj2UJDnf8g4I.png" alt="image-20210116173230197" style="zoom:80%;" />

代码实现（栈实现代码继续使用上面的代码）：

```
 // 函数：将十进制转成二进制   
      // 十进制  二进制
      // Decimal binary
    function dec2bin(decNumber) {
      // 1、定义栈对象
      let stack = new Stack()

      // 2、循环操作
      while(decNumber > 0){
        // 2.1、获取余数，并且放入到栈中
        stack.push(decNumber % 2)

        //2.2、获取整除后的结果，作为下一次运行的数字
        decNumber = Math.floor(decNumber / 2)
      }
      
      // 3、从栈中取出0和1
      let binaryString = ""
      while(!stack.isEmpty()){
        binaryString += stack.pop()
      }
      return binaryString
    }
    //测试十进制转二进制的函数
    console.log(dec2bin(5));  //101
    console.log(dec2bin(50)); //110010
```

#### 队列(queue)

> 只允许在一端插入数据操作，在另一端进行删除数据操作的特殊线性表；
>
> 进行插入操作的一端称为队尾（入队列），进行删除操作的一端称为队头（出队列）；队列具有先进先出（FIFO）的特性。

##### 普通队列的实现（线性存储）

```
//封装队列类
    function Queue() {
      //队列中的属性
      this.items = []
      //栈的相关操作
      //  1、将元素加入到队列中
      Queue.prototype.add = function (element) {
        this.items.push(element)
      }
      // 2、从队列中删除前端元素
      Queue.prototype.delete = function () {
        return this.items.shift()
      }
      //3、查看队列前端元素
      Queue.prototype.front = function () {
        return this.items[0]
      }
      // 4、判断队列是否为空
      Queue.prototype.isEmpty = function () {
        return this.items.length == 0
      }
      // 5、获取队列中元素的个数
      Queue.prototype.size = function () {
        return this.items.length
      }
      // 6、toString方法
      Queue.prototype.toString = function () {
        let resultString = ""
        for (let i = 0; i < this.items.length; i++) {
          resultString += this.items[i] + ' '
        }
        return resultString
      }
    }
    //队列的使用
    let q = new Queue()
    q.add('22')
    q.add('33')
    q.add('44')
    q.delete()
    console.log(q);
    console.log(q.size());
    console.log(q.front());
    console.log(q.isEmpty());
```

##### 优先级队列的实现

​		实现优先级队列主要考虑两个方面：

​		1、封装元素和优先级放在一起（可以封装一个新的构造函数）

​		2、添加元素时，将新插入元素的优先级和队列中已经存在的元素优先级进行比较，以获得自己正确的位置。

```
//封装优先级队列
    function PriorityQueue() {
      //在PriorityQueue重新创建了一个类：可以理解为内部类
      function QueueElement(element, priority) {
        this.element = element
        this.priority = priority
      }
      //封装属性
      this.items = []
      //实现插入方法
      PriorityQueue.prototype.add = function (element, priority) {
        //1、创建QueueElement实例
        let queueElement = new QueueElement(element, priority)

        // 2、判断队列是否为空
        if (this.items.length == 0) {
          this.items.push(queueElement)
        } else {
          let added = false
          for (let i = 0; i < this.items.length; i++) {
            if (queueElement.priority > this.items[i].priority) {
              this.items.splice(i, 0, queueElement)
              added = true
              break
            }
          }
          if (!added) {
            this.items.push(queueElement)
          }
        }

      }
      // 2、从队列中删除前端元素
      PriorityQueue.prototype.delete = function () {
        return this.items.shift()
      }
      //3、查看队列前端元素
      PriorityQueue.prototype.front = function () {
        return this.items[0]
      }
      // 4、判断队列是否为空
      PriorityQueue.prototype.isEmpty = function () {
        return this.items.length == 0
      }
      // 5、获取队列中元素的个数
      PriorityQueue.prototype.size = function () {
        return this.items.length
      }
      // 6、toString方法
      PriorityQueue.prototype.toString = function () {
        let resultString = ""
        for (let i = 0; i < this.items.length; i++) {
          resultString += "<——" + "[" + this.items[i].element + '，' + this.items[i].priority + "]"
        }
        return resultString
      }
    }
    let q = new PriorityQueue()
    q.add('三级(a)', 1)    
    q.add('一级(b)', 100)
    q.add('二级(c)', 10)
    q.add('三级(d)', 1)
    q.add('一级(e)', 100)
    q.add('二级(f)', 10)
    console.log(q.front())    // QueueElement {element: "一级(b)", priority: 100}element: "一级(b)"priority: 100__proto__: Object
    console.log(q.size())	//6
    console.log(q.toString()) //<——[一级(b)，100]<——[一级(e)，100]<——[二级(c)，10]<——[二级(f)，10]<——[三级(a)，1]<——[三级(d)，1]
    q.delete()				
    console.log(q.size()) 	//5
    console.log(q.toString()) //<——[一级(e)，100]<——[二级(c)，10]<——[二级(f)，10]<——[三级(a)，1]<——[三级(d)，1]
    q.add('二级(g)', 10)
    console.log(q.size())     //6
    console.log(q.toString()) //<——[一级(e)，100]<——[二级(c)，10]<——[二级(f)，10]<——[二级(g)，10]<——[三级(a)，1]<——[三级(d)，1]
```

#### 链表结构

##### 单向链表简介

> 链表是一种物理[存储单元](https://baike.baidu.com/item/存储单元/8727749)上非连续、非顺序的[存储结构](https://baike.baidu.com/item/存储结构/350782)，[数据元素](https://baike.baidu.com/item/数据元素/715313)的逻辑顺序是通过链表中的引用或者[指针](https://baike.baidu.com/item/指针/2878304)链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。每个结点包括两个部分：一个是存储[数据元素](https://baike.baidu.com/item/数据元素)的数据域，另一个是存储下一个结点地址的[指针](https://baike.baidu.com/item/指针/2878304)域。 

![img](https://img-blog.csdn.net/20180502195346635)

**相对于数组，链表有以下优势：**

​	1、内存空间不是必须连续的，可以充分，利用计算机的内存，实现灵活的内存动态管理

​	2、链表不必在创建时就确定大小，并且大小可以无限的延伸下去

​	3、链表在插入和删除数据时，时间复杂度可以达到O(1),相对数组效率高很多

注：插入操作复杂度：单链表 表头 O(1) 表尾O(n)
										循环链表 表头 O(1) 表尾O(n)
										双向链表 O(1)

**相对于数组，链表有以下缺点：**

​	1、链表访问任何一个位置的元素时，都需要从头开始访问。（无法跳过第一个元素访问任何一个元素)

​	2、无法通过下标直接访问元素，需要从头一个个访问，直到找到对应元素，时间复杂度为O(n),而线性表和顺序表相应的时间复杂度分别是O(logn)和				O(1)。

##### js单链表的实现

```
//封装单向链表类
    function LinkedList() {
      //内部类：节点类
      function Node(data) {
        this.data = data
        this.next = null
      }
      //属性
      this.head = null
      this.length = 0
      //1、append追加方法
      LinkedList.prototype.append = function (data) {
        // 1、创建新的节点
        let newNode = new Node(data)
        //2、判断是否添加的是第一个节点
        if (this.length === 0) {   //2.1、是第一个节点
          this.head = newNode
        } else {                   //2.2、不是第一个节点
          //找到最后一个节点
          let current = this.head
          while (current.next) {
            current = current.next
          }
          //最后节点的next指向新的节点
          current.next = newNode
        }
        this.length++
      }
      // 3、insert插入方法
      LinkedList.prototype.insert = function (position, data) {
        // 1、对position进行越界判断
        if (position < 0 || position > this.length) {
          return false
        }
        //2、根据data创建newNode
        let newNode = new Node(data)
        // 3、判断插入的位置是否是第一个
        if (position === 0) {
          newNode.next = this.head
          this.head = newNode
        } else {
          let index = 0
           
          let previous = null
          while (index < position) {
            index++
            previous = current
            current = current.next
          }
          newNode.next = current
          previous.next = newNode
        }
        // 4、length ++
        this.length++
        return true
      }
      //2、toString将链表转成字符串形式
      LinkedList.prototype.toString = function () {
        let str = ''
        let current = this.head
        //while循环中的括号会进行隐性转换
        while (current) {
          str += current.data + " "
          current = current.next
        }
        return str
      }
      // 3、get方法 获取对应位置的元素
      LinkedList.prototype.get = function (position) {
        // 1、对position进行越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2、获得对应的data
        let current = this.head
        let index = 0
        while (index < position) {
          index++
          current = current.next
        }
        return current.data
      }
      // 4、indexOf 返回元素在列表中的索引
      LinkedList.prototype.indexOf = function (data) {
        // 2、获得对应的data
        let current = this.head
        let index = 0
        while (current) {
          if (current.data === data) {
            flag = true
            return index
          }
          index++
          current = current.next
        }
        return -1
      }
      // 5、update方法 修改某个位置的元素
      LinkedList.prototype.update = function (position, newData) {
        // 1、对position进行越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2、查找正确的节点
        let current = this.head
        let index = 0
        while (index < position) {
          index++
          current = current.next
        }
        // 3、将position位置的node的data修改成newData
        current.data = newData
        return true
      }
      // 6、removeAt 从列表的特定位置移除一项
      LinkedList.prototype.removeAt = function (position) {
        // 1、对position进行越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2、判断是否删除的是第一个节点
        if (position === 0) {
          this.head = this.head.next
          this.head.next = null
        }
        else {
          let index = 0
          let current = this.head
          let previous = null
          while (index < position) {
            index++
            previous = current
            current = current.next
          }
          // 前一个节点的next指current的next即可
          previous.next = current.next
          current.next = null
          // 3、length - 1
          this.length--
          return true
        }
      }
      // 7、remove从列表中移除一项
      LinkedList.prototype.remove = function (data) {
        // 1、获取data在列表中的位置
        let position = this.indexOf(data)
        // 2、根据位置信息，删除节点
        return this.removeAt(position)
      }
      // 8、isEmpty 如果链表中不包含任何元素，返回true,如果链表长度大于0则返回false
      LinkedList.prototype.isEmpty = function () {
        return this.length === 0
      }
      //9、size 如果链表包含的元素个数
      LinkedList.prototype.size = function () {
        return this.length
      }
    }
    //實例化單鏈表
    let link = new LinkedList()
    link.append('10')
    link.append('11')
    console.log(link);
    link.insert(1, 'fuck')
    link.insert(3, 'you')
    console.log(link.toString());
    console.log(link.get(1));
    console.log(link.indexOf('fuck'));
    console.log(link.update(1, 'fuc'));
    link.removeAt(3)
    link.remove('11')
    console.log(link.toString());
    console.log(link.isEmpty());
    console.log(link.size());
  </script>
```

插入操作图示

<img src="https://i.loli.net/2021/02/27/hf1Lld9nB2AFZY5.png" alt="image-20210120214609061" style="zoom:80%;" />

##### 双向链表简介

> 双向链表也叫双链表，是链表的一种，它的每个数据[结点](https://baike.baidu.com/item/结点/9794643)中都有两个引用或[指针](https://baike.baidu.com/item/指针/2878304)，分别指向直接后继和直接前驱。所以，从双向链表中的任意一个结点开始，都可以很方便地访问它的前驱结点和后继结点。一般我们都构造双向[循环链表](https://baike.baidu.com/item/循环链表/3228465)。

<img src="https://images2015.cnblogs.com/blog/1146465/201704/1146465-20170429154748350-758050755.png" alt="img" />

![image-20210121115202505](https://i.loli.net/2021/02/27/PZybI8rs74BgjUD.png)

特点： 

1、既可以从头遍历到尾，也可以从尾遍历到头

2、一个节点既有向前连接的引用，也有一个向后连接的引用

双向链表的缺点：

1、每次在插入或删除某个节点时，需要处理四个引用，而不是两个。也就是实现起来要困难一些

2、并且对比单向链表，占用内存空间更大一些

但是这些缺点和我们使用起来的方便程度相比是微不足道的

##### js双向链表的实现

```
    //封装双向链表
    function DoublyLinkedList() {
      //内部类：节点类
      function Node(data) {
        this.data = data
        this.prev = null
        this.next = null
      }
      //属性
      this.head = null
      this.tail = null
      this.length = 0
      // 常见的操作：方法
      // 1、append方法
      DoublyLinkedList.prototype.append = function (data) {
        // 1、根据data创建节点
        let newNode = new Node(data)
        // 2、判断添加的是否是第一个节点
        if (this.length == 0) {
          this.head = newNode
          this.tail = newNode
        } else {
          newNode.prev = this.tail
          this.tail.next = newNode
          this.tail = newNode
        }
        this.length++
      }
      // 2、insert 插入
      DoublyLinkedList.prototype.insert = function (position, data) {
        // 1、越界判断
        if (position < 0 || position > this.length) {
          return false
        }
        // 2、根据data创建新的节点
        let newNode = new Node(data)
        // 3、判断原来的列表是否为空
        if (this.length === 0) {
          this.head = newNode
          this.tail = newNode
        } else {
          if (position === 0) {   //2.1、判断position是否为0
            this.head.prev = newNode
            newNode.next = this.head
            this.head = newNode
          } else if (position === this.length) {
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
            newNode.next = null
          } else {
            let current = this.head
            let index = 0
            while (index < position) {
              index++
              current = current.next
            }
            //修改指针
            current.prev.next = newNode
            newNode.prev = current.prev
            newNode.next = current
            current.prev = newNode
          }
        }
        this.length++
        return true
      }
      // 3、get方法      
      DoublyLinkedList.prototype.get = function (position) {
        // 1、越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2、获取元素
        let current = this.head
        let index = 0
        while (index < position) {
          index++
          current = current.next
        }
        return current.data
      }
      // 4、indexOf方法
      DoublyLinkedList.prototype.indexOf = function (data) {
        let current = this.head
        let index = 0
        while (current) {
          if (current.data === data) {
            return index
          } else {
            current = current.next
            index++
          }
        }
        return -1
      }
      // 5、update方法      
      DoublyLinkedList.prototype.update = function (position, data) {
        // 1、越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2、获取元素
        let current = this.head
        let index = 0
        while (index < position) {
          index++
          current = current.next
        }
        current.data = data
        return true
      }
      // 6、removeAt 从列表的特定位置移除一项
      DoublyLinkedList.prototype.removeAt = function (position) {
        // 1、越界判断
        if (position < 0 || position >= this.length) {
          return false
        }
        // 2,判断是否只有一个节点
        let current = this.head
        if (this.length === 0) {
          this.head = null
          this.tail = null
        } else {
          if (position == 0) {   //判断是否删除的是第一个节点
            this.head.next.prev = null
            this.head = this.head.next
          } else if (position == this.length - 1) {  //最后节点
            current = this.tail
            this.tail.prev.next = null
            this.tail = this.tail.prev
          } else {
            let index = 0

            while (index < position) {
              current = current.next
              index++
            }
            current.prev.next = current.next
            current.next.prev = current.prev
          }
        }
        this.length--
        return current.data
      }
      // 7,remove方法
      DoublyLinkedList.prototype.remove = function (data) {
        // 1,根据data获取下标值
        let index = this.indexOf(data)
        // 2,根据index删除对应位置的节点
        return this.removeAt(index)
      }
      // 8,isEmpty方法
      DoublyLinkedList.prototype.isEmpty = function () {
        return this.length
      }
      // 9,size方法
      DoublyLinkedList.prototype.size = function () {
        return this.length
      }
      // 10,获取链表的第一个元素
      DoublyLinkedList.prototype.getHead = function () {
        return this.head.data
      }
      // 11,获取链表的最后一个元素
      DoublyLinkedList.prototype.getTail = function () {
        return this.tail.data
      }
      // toString方法
      DoublyLinkedList.prototype.toString = function () {
        return this.forwardString()
      }
      //  forwardString 返回正向遍历的节点字符串形式
      DoublyLinkedList.prototype.forwardString = function () {
        let str = ''
        let current = this.head
        while (current !== null) {
          str += current.data + " "
          current = current.next
        }
        return str
      }
      // backwardString 返回反向遍历的节点字符串形式
      DoublyLinkedList.prototype.backwardString = function () {
        let str = ''
        let current = this.tail
        while (current !== null) {
          str += current.data + " "
          current = current.prev
        }
        return str
      }
    }
    let link = new DoublyLinkedList()
    link.append(1)
    link.append(2)
    link.append(3)
    link.insert(2, 4)
    link.insert(3, 5)

    console.log(link.update(1, 'update'))
    console.log(link.removeAt(0));
    console.log(link.toString());
    console.log(link.backwardString());
    console.log(link.get(4));
    console.log(link.indexOf(4));
    console.log(link.size());
    console.log(link.getHead());
```

#### 集合结构

##### 简介

同数学中所学的一样，集合(Set)是由一组无序但彼此之间又有一定关系性的成员构成，每个成员在集合中只能出现一次，不同于我们之前说的[字典](https://www.jb51.net/article/167971.htm)，[链表](https://www.jb51.net/article/167962.htm)之类的，它是一种包含了不同元素的数据结构(集合中的元素称为成员)，从其定义中我们可以看出它具有两个很重要的特征：首先，集合中的成员是无序的，其次，集合中的成员是不相同的，即集合中不存在相同的成员。

**集合的定义**

我们要实现一个集合，首先要对其一些定义做了解

- 不包含任何成员的集合称为**空集**，包含一切可能成员的集合称为**全集**。
- 如果两个集合里的成员都完全相同，则称两个集合相等。
- 如果一个集合所有成员都包含于另一个集合，则前一集合称为后一集合的一个**子集**。

**集合的操作**

通常来说，集合的基本操作有以下三种：

- 并集：将两个集合中的成员进行合并，得到一个新的集合
- 交集：将两个集合中共同存在的成员组成的一个新的集合
- 补集：属于一个集合而不属于另一个集合的成员组成的新的集合

注：集合通常是由一组无序的，不能重复的元素构成。

​		1、和数学中的集合名词比较相似，但是数组中的集合范围更大一些，也允许集合中的元素重复

​		2、在计算机中，集合通常表示的结构中元素是不允许重复的

集合是特殊的数组：

​		1、特殊之处在于里面的元素没有顺序，也不能重复

​		2、没有顺序意味着不能通过下标值进行访问，不能重复意味着相同的对象在集合中只会存在一份

<img src="https://i.loli.net/2021/02/27/5lgpkNYFSyzKrXh.png" alt="image-20210121175253633" style="zoom: 67%;" />

##### js集合实现

```
    function Set(value) {
      //属性
      this.items = {}
      //方法
      //add方法
      Set.prototype.add = function (value) {
          //判断当前集合中是否已经包含了该元素
          if (this.has(value)) {
            return false
          }
          //将元素添加到集合中
          this.items[value] = value
        return true
      }
      //has方法
      Set.prototype.has = function (value) {
        //hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。
        return this.items.hasOwnProperty(value)
      }
      //remove方法
      Set.prototype.remove = function (value) {
        // 1,判断该集合中是否包含该元素
        if (!this.has(value)) {
          return false
        }
        // 2,将元素从属性中删除
        delete this.items[value]
        return true
      }
      //clear方法
      Set.prototype.clear = function () {
        this.items = {}
        return true
      }
      //size方法
      Set.prototype.size = function () {
        return Object.keys(this.items).length
      }
      //获取集合中所有的值
      Set.prototype.values = function () {
      	
        return Object.keys(this.items)
      }
      //集合间的操作
      //并集:对于给定的两个集合,返回一个包含两个集合中所有元素的新集合
      Set.prototype.union = function (otherSet){
        //this:集合对象A
        //otherSet:集合对象B
        // 1,创建新的集合
        let unionSet = new Set()
        // 2,将A集合中所有的元素添加到新集合中
        let values = this.values()
        for(var i = 0; i < values.length; i++){
          unionSet.add(values[i])
        }
        // 3,将B集合中所有的元素添加到新集合中
        values = otherSet.values()
        for (var i = 0; i < values.length; i++) {
          unionSet.add(values[i])
        }
        return unionSet
      }
      //交集:对于给定的两个集合,返回一个包含两个集合中共有元素的新集合
      Set.prototype.intersection = function (otherSet) {
        //this:集合对象A
        //otherSet:集合对象B
        // 1,创建新的集合
        let intersection = new Set()
        // 2,将A集合中所有的元素添加到新集合中
        let values = this.values()
        for (var i = 0; i < values.length; i++) {
          let item = values[i]
          if(otherSet.has(item)){
            intersection.add(item)
          }
        }
        return intersection
      }
      //差集:对于给定的两个集合,返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合
      Set.prototype.difference = function (otherSet) {
         //this:集合对象A
        //otherSet:集合对象B
        // 1,创建新的集合
        let differenceSet = new Set()
        // 2,将A集合中所有的元素添加到新集合中
        let values = this.values()
        for (var i = 0; i < values.length; i++) {
          let item = values[i]
          if (!otherSet.has(item)) {
            differenceSet.add(item)
          }
        }
        return differenceSet
      }
      //子集:验证一个给定集合是否是另一个集合的子集
      Set.prototype.subset = function (otherSet) {
        //this:集合对象A
        //otherSet:集合对象B
        let values = this.values()
        for (var i = 0; i < values.length; i++) {
          let item = values[i]
          if (!otherSet.has(item)) {
            return false
          }
        }
        return true
      }
      if (Array.isArray(value)) {
        let index = 0
        while (index < value.length) {
          this.add(value[index])
          index++
        }
      }
    }

    //测试Set类
    // 1,常见Set类对象
    let set = new Set(['a','b'])
    let setB = new Set(['a','c','d'])
    console.log(set.add(111));
    console.log(set.add(111));
    console.log(set.add(222));
    console.log(set.remove(111));
    console.log(set.size());
    console.log(set);
    console.log(set.union(setB));
    console.log(set.intersection(setB));
    console.log(set.difference(setB));
    console.log(setB.subset(new Set(['a','c','d'])));
```

#### 字典结构

> 字典是以[键，值]的形式来存储元素。字典也称作映射、符号表或关联数组。
>
> 集合、字典、散列表都可以存储不重复的数据。字典和我们上面实现的集合很像。
>
> ES5包含了一个Map类的实现，即我们所说的字典。

代码实现：

```
    class Dictionary {
      constructor() {
        this.items = {}
      }
      // 向字典中添加新元素。如果key已经存在，那么已存在的value会被indeed值覆盖
      set(key, value) {
        this.items[key] = value;
      }
      //通过键值作为参数查找特定的数值并返回。
      get(key) {
        return this.items[key];
      }
      //通过使用键值作为参数来从字典中移除键值对应的数据值
      remove(key) {
        delete this.items[key];
      }
      //将字典所包含的所有键名以数组形式返回
      keys() {
        return Object.keys(this.items);
      }
      //将字典所包含的所有数值以数组形式返回
      values() {
        // es7 提供的 Object.values 方法
        // return Object.values(this.items);

        // 或者循环输出
        return Object.keys(this.items).reduce((previousValue, currentValue, currentIndex) => {
          previousValue.push(this.items[currentValue]);
          return previousValue;
        }, [])
      }
    }
    // 使用
    let dictionary = new Dictionary();
    dictionary.set('Gandalf', 'gandalf@email.com')
    dictionary.set('John', 'johnsnow@email.com')
    dictionary.set('Tyrion', 'tyrion@email.com')
    dictionary.remove('John')
    console.log(dictionary.get('John'));
    console.log(dictionary.get('Gandalf'));
    console.log(dictionary)
    console.log(dictionary.keys())
    console.log(dictionary.values())
    console.log(dictionary.items)
```

#### 哈希表

> `哈希表`（Hash T able/Hash Map，也叫散列表），是根据键（Key）而直接访问在`内存存储位置`的`数据结构`。也就是说，它通过计算一个关于键值的函数，将所需查询的数据映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做`哈希函数`，`存放记录`的`数组`称做`哈希表`。

**一.数组的缺点**

```
     1.数组进行插入操作时，效率比较低。     2.数组基于索引去查找的操作效率非常高，基于内容去查找效率很低。     3.数组进行删除操作，效率也不高。
```

**二.哈希表**

1.几乎所有的编程语言都有直接或间接的应用这种数据结构

2.哈希表是基于 数组 实现的，但相对于数组有很多优势。

```
     1.它可以提供非常快速的 插入-删除-查找 操作     2.无论多少数据，插入和删除需要接近常量的时间。即O（1）的时间级     3.哈希表的速度比树还要快，基本可以瞬间找到想要的元素。     4.哈希表相对于树来说编码要容易
```

3.哈希表对于数组的一些不足

```
     1.哈希表中的数据是没有顺序的，所以不能以一种固定的方式来遍历其中的元素。     2.通常情况下，哈希表中的key是不允许重复的，不能放置相同的key，用于保存不同的元素。
```

4.哈希表的实质

```
     1.哈希表不同于（数组和链表，甚至于树可以画出他的结构）。     2.他的结构就是数组，但他神奇的地方在于它对下标值的一种变换，这种变换称为 哈希函数 ， 通过哈希函数可以获取到 HashCode。
```

5.哈希表的一些概念

```
     1.哈希化：将大数字转化为数组范围内下表的过程，我们称之为哈希化。（对大数字取余）     2.哈希函数：通常我们会将单词转化成大数字，大数字在进行哈希化的代码实现放在一个函数中，这个韩式称为哈希函数。     3.哈希表：最终的数据插入到的这个数组，对整个结构的封装，我们称之为是一个哈希表。
```

6.解决 哈希化后的下标值冲突 方案

------------------------1. 链地址法---------------------------

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404122342926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5ODEwMg==,size_16,color_FFFFFF,t_70)
1）每个存储单元存放的不再是单个数据，而是一个链条。
2）链条的结构可以是，数组或者链表。
3）比如是链表，一旦哈希化的下标值发生重复。将重复的元素插入到链表的首端或者尾端即可。
4）查询的时候，先根据哈希化后的下标值找到相应的位置，再取出链表，依次查询寻找需要的数据
5）根据业务需要选择数组还是链表，需要插在链条的最前面，选择链表。
插在后端选择数组或者链表都可以。

------------------------2. 开放地址法---------------------------

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404123845995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5ODEwMg==,size_16,color_FFFFFF,t_70)
1）开放地址法的主要工作方式是 寻找空白的的单元格来添加重复的数据
2）寻找位置的方法有三种 线性探测 ， 二次探测， 再哈希化。

-------------线性探测---------------

```
     1. 下标值重复时，采取index+1向后寻找空白位置插入数据。     2. 查找数据时，先去用哈希化后的索引去取值比对，如果不符合，向下继续线性查找。     3. 查找数据时，当哈希化后的索引值上的数据不符合，如果在线性查找时遇到数组项空白时，则停止查找，此数组中不存在目标数据。     4. 删除某数组中存的数据时，不能把值设置为null，可以进行特殊处理（比如设置为-1) 。来防止下次线性查找失败。
```

！！！！线性探索的问题：线性探测会产生聚集，即数据聚在在一连串的存储单元当中。影响之后的插入查询 删除 操作的效率，影响哈希表的性能。

-------------二次探测---------------

```
     1. 二次探测， 对步长进行了优化，index + 1 平方 ， + 2 平方， +3 平方，这样就可以一次探测比较长的距离，避免聚集带来的影响。     2. 可是还是会造成步长不一 的 一种聚集，还是会影响效率。
```

-------------再哈希化---------------

```
     1.把关键字用另一个哈希函数再做一次哈希化，用这个哈希化 的 结果 作为步长，对于指定的关键字，步长在探索中是不变的，不同的关键字使用不同的步长。     2.需要和第一个哈希函数不同，不能输出为0.     3. stepszies（步长） = constant（常数，小于数组容量） - （key % constant）
```

结论： -------链地址法，使哈希表性能下降较为稳定。------开发地址法，由于填充因子，步长等因素会使性能下降的急剧。

**哈希函数**

<img src="https://i.loli.net/2021/02/27/xiCFyMAd89n4qzN.png" alt="image-20210208150444904" style="zoom:67%;" /><img src="https://i.loli.net/2021/02/27/kA7awjPhRQYx3tX.png" alt="image-20210208150738643" style="zoom: 80%;" />

<img src="https://i.loli.net/2021/02/27/3NHWPUmhrfbKXly.png" alt="image-20210208150754449" style="zoom: 80%;" />

<img src="https://i.loli.net/2021/02/27/ReksFliDnWt5TPj.png" alt="image-20210208150944918" style="zoom:67%;" />

**散列函数的构造方法**

好的散列函数要求：（1）计算简单，至少散列函数的计算时间不应该超过其他查找技术与关键字比较的时间；（2）计算出的散列地址分布均匀，这样可以保证存储空间的有效利用，并减少为处理冲突而耗费的时间。

**1.** **直接定址法**

取关键字或关键字的某个线性函数值为散列地址。即H(key)=key或H(key) = a·key + b，其中a和b为常数（这种散列函数叫做自身函数）。

**2.** **数字分析法**

假设某公司的员工登记表以员工的手机号作为关键字。手机号一共11位。前3位是接入号，对应不同运营商的子品牌；中间4位表示归属地；最后4位是用户号。不同手机号前7位相同的可能性很大，所以可以选择后4位作为散列地址，或者对后4位反转（1234 -> 4321）、循环右移（1234 -> 4123）、循环左移等等之后作为散列地址。

数字分析法通常适合处理关键字位数比较大的情况，如果事先知道关键字的分布且关键字的若干位分布比较均匀，就可以考虑这个方法。

**3.** **平方取中法**

假设关键字是1234、平方之后是1522756、再抽取中间3位227，用作散列地址。平方取中法比较适合于不知道关键字的分布，而位数又不是很大的情况。

**4.** **折叠法**

将关键字从左到右分割成位数相等的几部分，最后一部分位数不够时可以短些，然后将这几部分叠加求和，并按散列表表长，取后几位作为散列地址。

比如关键字是9876543210，散列表表长是3位，将其分为四组，然后叠加求和：987 + 654 + 321 + 0 = 1962，取后3位962作为散列地址。

折叠法事先不需要知道关键字的分布，适合关键字位数较多的情况。

**5.** **除留余数法**

f(key) = key mod p (p≤m)，m为散列表长。这种方法不仅可以对关键字直接取模，也可在折叠、平方取中后再取模。根据经验，若散列表表长为m，通常p为小于或等于表长（最好接近m）的最小质数，可以更好的减小冲突。

此方法为最常用的构造散列函数方法。

**6.** **随机数法**

f(key) = random(key)，这里random是随机函数。当关键字的长度不等时，采用这个方法构造散列函数是比较合适的。

实际应用中，应该视不同的情况采用不同的散列函数。如果关键字是英文字符、中文字符、各种各样的符号，都可以转换为某种数字来处理，比如其unicode编码。下面这些因素可以作为选取散列函数的参考：（1）计算散列地址所需的时间；（2）关键字长度；（3）散列表大小；（4）关键字的分布情况；（5）查找记录的频率。

**哈希表的基本实现：**

```
class HashTable {
  constructor() {
    this.storage = []
    this.count = 0
    this.limit = 7
  }
  // 1、将字符串转成比较大的数字：hasCode
  // 2、将大的数字hashCode压缩到数组范围（大小）之内
  //! 哈希算法
  hashFunc (str, size) {
    //   //1、定义hashCode变量
    var hashCode = 0
    // cats -> Unicode编码
    //! 普通模式
    //   for(var i = 0; i < str.length; i++){
    //     let j = 1
    //     let flag = str.length -1 - i
    //     while(flag){
    //       console.log('flag:'+flag);
    //       j *= 37
    //       console.log(j);
    //       flag--
    //     }
    //     hashCode += str.charCodeAt(i) * j
    //   }
    //   console.log(hashCode);
    //! 2、使用霍纳算法，来计算hashCode的值
    // for (var i = 0; i < str.length; i++) {
    //   hashCode = 37 * hashCode + str.charCodeAt(i)
    // }
    //! 3、自创的随机算法
    // 通过split把字符串分割成单字符数组
    let numArray = str.split('').map(char => char.charCodeAt(0));
    //将所有数组元素连成字符串，不能直接相加
    numArray = numArray.join('')
    // 变成随机数值
    hashCode = Math.sin(numArray).toString().split('.')[1].slice(0, 10);
    // 3、取余操作
    var index = hashCode % size
    return index
  }
  //!添加元素
  put (key, value) {
    //1.根据key获取对应的index
    var index = this.hashFunc(key, this.limit)
    //2.根据index取得对应的bucket(bucket是key的hashCode对应下表位置，)
    var bucket = this.storage[index]
    //3.判断当前bucket是否为空
    if (bucket == null) {
      bucket = []
      this.storage[index] = bucket
    }
    //4.判断是否修改数据
    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i]
      if (tuple[0] == key) {
        tuple[1] = value
        return
      }
    }
    //5.当前bucket(链表)中没有该数据,就直接添加该数据
    bucket.push([key, value])
    this.count += 1
    //数组扩容
    if (this.count > this.limit * 0.75) {
      this.resize('expand')
    }
  }
  //!get获取元素
  get (key) {
    /**
    *1.根据key,获得index;
    * 2.根据index,获得bucket;
    * 3.判断bucket是否为null,为null就直接返回null
    * 4.bucket不为null，则遍历找到对应的key
    * 5.遍历完后，没有找到对应的key，就返回null
    **/
    var index = this.hashFunc(key, this.limit)
    var bucket = this.storage[index]
    if (bucket == null) {
      return null
    }
    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i]
      if (tuple[0] == key) {
        return tuple[1]
      }
    }
    //在index对应的bucket（不为null）中没有找到对应的key
    return null

  }
  //!remove方法
  remove (key) {
    var index = this.hashFunc(key, this.limit)
    var bucket = this.storage[index]
    if (bucket == null) {
      return null
    }
    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i]
      if (tuple[0] == key) {
        bucket.splice(i, 1)//删除当前位置的元素https://wangdoc.com/javascript/stdlib/array.html#splice
        this.count--
        return tuple[1]
        //缩小容量
        if (this.limit > 7 && this.count < this.limit * 0.25) {
          this.resize('narrow')
        }
      }
    }
    return null
  }
  //!判断哈希表是否为空
  isEmpty () {
    return this.count == 0
  }
  //!获取哈希表的长度
  size () {
    return this.count
  }
  //!哈希表的扩容
  resize (newLimit) {
      console.log(this);
      //! 1、获取哈希表新的数组大小限制
      getNewLimit.call(this)
      //! 2、重新初始化哈希表
      var oldStorage = init.call(this)
      //! 3、把值转移到新的哈希表上
      moveHash.call(this, oldStorage)
    // 获取哈希表新的limit
    function getNewLimit () {
      if (newLimit === 'expand') {
        var newSize = this.limit * 2
        var newPrime = getPrime(newSize)
      } else if (newLimit === 'narrow') {
        var newSize = Math.floor(this.limit / 2)
        var newPrime = getPrime(newSize)
      }
      this.limit = newPrime
      //*判断数字是否是质数
      function isPrime (num) {
        //1.获得num的平方根
        //特点：智能被1和自己整除，不能被2到num-1数字整除
        //*普通算法
        // for(var i = 2;i < num;i++){
        //   if (num % i == 0) {
        //     return false
        //   }
        // }
        //*优化算法
        var temp = Math.ceil(Math.sqrt(num))
        for (var i = 2; i <= temp; i++) {
          if (num % i === 0) {
            return false
          }
        }
        return true
      }
      //*获取质数的方法
      function getPrime (num) {
        while (!isPrime(num)) {
          num++
        }
        console.log(num);
        return num
      }
    }
    //*初始化哈希表
    function init () {
      console.log(this);
      var oldStorage = this.storage
      this.storage = []
      this.count = 0
      var bucket = []
      this.storage = []
      this.count = 0

      console.log(oldStorage);
      return oldStorage
    }
    //*转移哈希表
    function moveHash (oldStorage) {
      var bucket = []
      for (var i = 0; i < oldStorage.length; i++) {
        bucket = oldStorage[i]
        if (!bucket) {
          continue
        }
        for (var j = 0; j < bucket.length; j++) {
          var tuple = bucket[j]
          this.put(tuple[0], tuple[1])
        }
      }
    }
  }
  
}
var hash = new HashTable()

hash.put('abc', '123')
hash.put('abd', '456')
hash.put('cbd', '789')
hash.put('a34c', '123')
hash.put('a43bd', '456')
hash.put('c434bd', '789')
hash.put('ab434c', '123')
hash.put('a434bd', '456')
hash.put('c434bd', '789')
hash.put('ab2434c', '123')
hash.put('a434b3d', '456')
hash.put('c21314bd', '789')
hash.put('c212334bd', '789')
hash.put('c2123314bd', '789')
console.log(hash);
console.log(hash.get('ab2434c'));
```

**哈希表的扩容：**

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png" alt="image-20210213160949118" style="zoom:67%;" />

#### 树（tree)

> 树结构是一种非线性存储结构，存储的是具有“一对多”关系的数据元素的集合 。
>
> 树是由结点或顶点和边组成的(可能是非线性的)且不存在着任何环的一种数据结构。没有结点的树称为空(null或empty)树。一棵非空的树包括一个根结点，还(很可能)有多个附加结点，所有结点构成一个多级分层结构。

##### 一、树结构简介

###### 1.1.简单了解树结构

**什么是树？**

真实的树：

![image-20200229205530929](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png)

**树的特点：**

- 树一般都有一个**根**，连接着根的是**树干**；
- 树干会发生分叉，形成许多**树枝**，树枝会继续分化成更小的**树枝**；
- 树枝的最后是**叶子**；

现实生活中很多结构都是树的抽象，模拟的树结构相当于旋转`180°`的树。

![image-20200229205630945](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png)



**树结构对比于数组/链表/哈希表有哪些优势呢：**

**数组：**

- 优点：可以通过**下标值访问**，效率高；
- 缺点：查找数据时需要先对数据进行**排序**，生成**有序数组**，才能提高查找效率；并且在插入和删除元素时，需要大量的**位移操作**；

**链表：**

- 优点：数据的插入和删除操作效率都很高；
- 缺点：**查找**效率低，需要从头开始依次查找，直到找到目标数据为止；当需要在链表中间位置插入或删除数据时，插入或删除的效率都不高。

**哈希表：**

- 优点：哈希表的插入/查询/删除效率都非常高；
- 缺点：**空间利用率不高**，底层使用的数组中很多单元没有被利用；并且哈希表中的元素是**无序**的，不能按照固定顺序遍历哈希表中的元素；而且不能快速找出哈希表中**最大值或最小值**这些特殊值。

**树结构：**

优点：树结构综合了上述三种结构的优点，同时也弥补了它们存在的缺点（虽然效率不一定都比它们高），比如树结构中数据都是有序的，查找效率高；空间利用率高；并且可以快速获取最大值和最小值等。

总的来说：**每种数据结构都有自己特定的应用场景**

**树结构：**

- **树（Tree）**:由 n（n ≥ 0）个节点构成的**有限集合**。当 n = 0 时，称为**空树**。

对于任一棵非空树（n > 0），它具备以下性质：

- 数中有一个称为**根（Root）**的特殊节点，用 **r **表示；
- 其余节点可分为 m（m > 0）个互不相交的有限集合 T1，T2，...，Tm，其中每个集合本身又是一棵树，称为原来树的**子树（SubTree）**。

**树的常用术语：**

![image-20200229221126468](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png)

- **节点的度（Degree）**：节点的**子树个数**，比如节点B的度为2；
- **树的度**：树的所有节点中**最大的度数**，如上图树的度为2；
- **叶节点（Leaf）**：**度为0的节点**（也称为叶子节点），如上图的H，I等；
- **父节点（Parent）**：度不为0的节点称为父节点，如上图节点B是节点D和E的父节点；
- **子节点（Child）**：若B是D的父节点，那么D就是B的子节点；
- **兄弟节点（Sibling）**：具有同一父节点的各节点彼此是兄弟节点，比如上图的B和C，D和E互为兄弟节点；
- **路径和路径长度**：路径指的是一个节点到另一节点的通道，路径所包含边的个数称为路径长度，比如A->H的路径长度为3；
- **节点的层次（Level）**：规定**根节点在1层**，其他任一节点的层数是其父节点的**层数加1**。如B和C节点的层次为2；
- **树的深度（Depth）**：树种所有节点中的**最大层次**是这棵树的深度，如上图树的深度为4；

###### 1.2.树结构的表示方式

- **最普通的表示方法**：

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png" alt="image-20200229230417613" style="zoom:80%;" />

如图，树结构的组成方式类似于链表，都是由一个个节点连接构成。不过，根据每个父节点子节点数量的不同，每一个父节点需要的引用数量也不同。比如节点A需要3个引用，分别指向子节点B，C，D；B节点需要2个引用，分别指向子节点E和F；K节点由于没有子节点，所以不需要引用。

这种方法缺点在于我们无法确定某一结点的引用数。

- **儿子-兄弟表示法**：

![image-20200229232805477](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210624225152.png)



这种表示方法可以完整地记录每个节点的数据，比如：

```
//节点ANode{  //存储数据  this.data = data  //统一只记录左边的子节点  this.leftChild = B  //统一只记录右边的第一个兄弟节点  this.rightSibling = null}//节点BNode{  this.data = data  this.leftChild = E  this.rightSibling = C}//节点FNode{  this.data = data  this.leftChild = null  this.rightSibling = null}
```

这种表示法的优点在于每一个节点中引用的数量都是确定的。

- **儿子-兄弟表示法旋转**

以下为儿子-兄弟表示法组成的树结构：

![image-20200229234549049](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/6.png)



将其顺时针旋转45°之后：

![image-20200229235549522](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/7.png)



这样就成为了一棵**二叉树**，由此我们可以得出结论：**任何树都可以通过二叉树进行模拟**。但是这样父节点不是变了吗？其实，父节点的设置只是为了方便指向子节点，在代码实现中谁是父节点并没有关系，只要能正确找到对应节点即可。

##### 二、二叉树

###### 2.1.二叉树简介

**二叉树的概念**：如果树中的每一个节点最多只能由**两个子节点**，这样的树就称为**二叉树**；

二叉树十分重要，不仅仅是因为简单，更是因为几乎所有的树都可以表示成二叉树形式。

**二叉树的组成**：

- 二叉树可以为空，也就是没有节点；
- 若二叉树不为空，则它由根节点和称为其左子树TL和右子树TR的两个不相交的二叉树组成；

**二叉树的五种形态**：

![image-20200301001718079](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/8.png)



上图分别表示：空的二叉树、只有一个节点的二叉树、只有左子树TL的二叉树、只有右子树TR的二叉树和有左右两个子树的二叉树。

**二叉树的特性**：

- 一个二叉树的第 i 层的最大节点数为：2^(i-1)，i >= 1；
- 深度为k的二叉树的最大节点总数为：2^k - 1 ，k >= 1；
- 对任何非空二叉树，若 n0 表示叶子节点的个数，n2表示度为2的非叶子节点个数，那么两者满足关系：n0 = n2 + 1；如下图所示：H，E，I，J，G为叶子节点，总数为5；A，B，C，F为度为2的非叶子节点，总数为4；满足n0 = n2 + 1的规律。

![image-20200301092140211](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/9.png)

**完美二叉树**

完美二叉树（Perfect Binary Tree）也称为满二叉树（Full Binary Tree），在二叉树中，除了最下一层的叶子节点外，每层节点都有2个子节点，这就构成了完美二叉树。

![image-20200301093237681](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/10.png)

**完全二叉树**

完全二叉树（Complete Binary Tree）:

- 除了二叉树最后一层外，其他各层的节点数都达到了最大值；
- 并且，最后一层的叶子节点从左向右是连续存在，只缺失右侧若干叶子节点；
- 完美二叉树是特殊的完全二叉树；

![image-20200301093659373](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/11.png)



在上图中，由于H缺失了右子节点，所以它不是完全二叉树。

###### 2.3.二叉树的数据存储

常见的二叉树存储方式为**数组**和**链表**：

**使用数组：**

- **完全二叉树**：按从上到下，从左到右的方式存储数据。

![image-20200301094919588](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/12.png)



| 节点     | A     | B     | C     | D     | E     | F     | G     | H     |
| -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| **序号** | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** |

使用数组存储时，取数据的时候也十分方便：左子节点的序号等于父节点序号 * 2，右子节点的序号等于父节点序号 * 2 + 1 。

- **非完全二叉树**：非完全二叉树需要转换成完全二叉树才能按照上面的方案存储，这样会浪费很大的存储空间。

![image-20200301100043636](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/13.png)



| 节点     | A     | B     | C     | ^     | ^     | F     | ^     | ^     | ^     | ^      | ^      | ^      | M      |
| -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ------ | ------ | ------ | ------ |
| **序号** | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **10** | **11** | **12** | **13** |

**使用链表**

二叉树最常见的存储方式为**链表**：每一个节点封装成一个Node，Node中包含存储的数据、左节点的引用和右节点的引用。

![image-20200301100616105](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/14.png)

##### 三、二叉搜索树

###### 认识二叉搜索树

**二叉搜索树**（**BST**，Binary Search Tree），也称为**二叉排序树**和**二叉查找树**。

二叉搜索树是一棵二叉树，可以为空；

如果不为空，则满足以下**性质**：

- 条件1：非空左子树的**所有**键值**小于**其根节点的键值。比如三中节点6的所有非空左子树的键值都小于6；
- 条件2：非空右子树的**所有**键值**大于**其根节点的键值；比如三中节点6的所有非空右子树的键值都大于6；
- 条件3：左、右子树本身也都是二叉搜索树；

![image-20200301103139916](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/15.png)

如上图所示，树二和树三符合3个条件属于二叉树，树一不满足条件3所以不是二叉树。

**总结：**二叉搜索树的特点主要是**较小的值**总是保存在**左节点**上，相对**较大的值**总是保存在**右节点**上。这种特点使得二叉搜索树的查询效率非常高，这也就是二叉搜索树中"搜索"的来源。

###### 二叉搜索树应用举例

下面是一个二叉搜索树：

![image-20200301111718686](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/16.png)



若想在其中查找数据10，只需要查找4次，查找效率非常高。

- 第1次：将10与根节点9进行比较，由于10 > 9，所以10下一步与根节点9的右子节点13比较；
- 第2次：由于10 < 13，所以10下一步与父节点13的左子节点11比较；
- 第3次：由于10 < 11，所以10下一步与父节点11的左子节点10比较；
- 第4次：由于10 = 10，最终查找到数据10 。

<img src="https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/17.png" alt="image-20200301111751041" style="zoom:80%;" />

同样是15个数据，在排序好的数组中查询数据10，需要查询10次：



![image-20200301115348138](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/树一/18.png)



其实：如果是排序好的数组，可以通过二分查找：第一次找9，第二次找13，第三次找15...。我们发现如果把每次二分的数据拿出来以树的形式表示的话就是**二叉搜索树**。这就是数组二分法查找效率之所以高的原因。

###### 二叉搜索树的实现

   ![image-20210214212225358](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182900.png)                                   

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182903.png" alt="image-20210214222239631" style="zoom:67%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182909.png" alt="image-20210214222327176" style="zoom:67%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182911.png" alt="image-20210214231854466" style="zoom: 80%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210505223158.png" alt="image-20210215032041388" style="zoom:67%;" />

//前中后序遍历属于深度优先遍历

//而层序遍历则属于广度优先遍历

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182918.png" alt="image-20210215002601824" style="zoom:67%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182920.png" alt="image-20210215002223399" style="zoom:67%;" /><img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182922.png" alt="image-20210215030506361" style="zoom:67%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182924.png" alt="image-20210215120744477" style="zoom:67%;" />

删除总结：

1、**删除的节点是叶子节点的话，就直接让其父节点的指向它的引用为null就行**

2、**删除的节点含有左子树或者右子树，用其子树来代替成为被删除节点的父节点的子树**

3、**删除左右都有孩子的节点，找到右边子树最小的节点作为父节点**

代码实现：

```
     //封装二叉搜索树
     function BinarySearchTree () {
          function Node (key) {
          this.key = key
          this.left = null
          this.right = null
          }

          // 属性
          this.root = null

          //方法
          // 插入数据： 对外给用户调用的方法
          BinarySearchTree.prototype.insert = function (key) {
          //1、根据key值创建节点
          var newNode = new Node(key)

          // 2、判断根节点是否有值
          if (this.root === null) {
          this.root = newNode
          } else {
          this.insertNode(this.root, newNode)
          }
          }

          BinarySearchTree.prototype.insertNode = function (node, newNode) {
          if (newNode.key < node.key) {
          //向左查找
          if (node.left === null) {
          node.left = newNode
          } else {
          this.insertNode(node.left, newNode)
          }
          } else {
          //向右查找
          if (node.right === null) {
          node.right = newNode
          } else {
          this.insertNode(node.right, newNode)
          }
          }
          }

          //树的遍历
          //! 1、先序遍历 --- 根->左->右
          BinarySearchTree.prototype.preOrderTraversal = function () {
          var str = ''
          function handler (key) {
          str += key + ' '
          }
          this.preOrderTraversalNode(this.root, handler)
          console.log(str);
          }
          BinarySearchTree.prototype.preOrderTraversalNode = function (node, handler) {
          if (node !== null) {
          // 1、处理经过的节点
          handler(node.key)
          // 2、处理经过节点的左子节点
          this.preOrderTraversalNode(node.left, handler)
          // 3、处理经过节点的右子节点
          this.preOrderTraversalNode(node.right, handler)
          }
          }
          //! 2、中序遍历 --- 左->根->右
          BinarySearchTree.prototype.midOrderTraversal = function () {
          var str = ''
          function handler (key) {
          str += key + ' '
          }
          this.midOrderTraversalNode(this.root, handler)
          console.log(str);
          }
          BinarySearchTree.prototype.midOrderTraversalNode = function (node, handler) {
          if (node !== null) {
          // 1、处理经过节点的左子节点
          this.midOrderTraversalNode(node.left, handler)
          // 2、处理经过的节点
          handler(node.key)
          // 3、处理经过节点的右子节点
          this.midOrderTraversalNode(node.right, handler)
          }
          }
          //! 3、后序遍历 --- 左->右->根
          BinarySearchTree.prototype.postOrderTraversal = function () {
          var str = ''
          function handler (key) {
          str += key + ' '
          }
          this.postOrderTraversalNode(this.root, handler)
          console.log(str);
          }
          BinarySearchTree.prototype.postOrderTraversalNode = function (node, handler) {
          if (node !== null) {
          // 1、处理经过节点的左子节点
          this.postOrderTraversalNode(node.left, handler)
          // 2、处理经过节点的右子节点
          this.postOrderTraversalNode(node.right, handler)
          // 3、处理经过的节点
          handler(node.key)
          }
          }
          //!获取最小值
          BinarySearchTree.prototype.min = function () {
          var node = this.root
          while (node.left !== null) {
          node = node.left
          }
          return node.key
          }
          //!获取最大值
          BinarySearchTree.prototype.max = function () {
          var node = this.root
          while (node.right !== null) {
          node = node.right
          }
          return node.key
          }
          //!搜索特定的值
          BinarySearchTree.prototype.search = function (key) {
          return this.searchNode(this.root, key)
          }
          BinarySearchTree.prototype.searchNode = function (node, key) {
          // 1、如果传入的node为null，那么就退出递归
          if (node === null) {
          return false
          }

          //2、判断node节点值和传入的key大小
          if (node.key > key) {
          return this.searchNode(node.left, key)
          } else if (node.key < key) {
          return this.searchNode(node.right, key)
          } else {
          return true
          }
          }
          //!删除节点
          BinarySearchTree.prototype.remove = function (key) {
          // 1、寻找要删除的节点
          // 1、1定义变量，保存一些信息
          var current = this.root
          var parent = this.root
          var isLeftChild = true
          //1、2 开始寻找删除的节点
          while (current.key !== key) {
          if (key < current.key) {
          isLeftChild = true
          parent = current
          current = current.left
          } else {
          isLeftChild = false
          parent = current
          current = current.right
          }

          //某种情况：已经找到了最后的节点，依然没有找到 == key
          if (current === null) {
          return false
          }
          }
          console.log(current);
          //2、根据对应的情况删除节点
          //2、1删除的节点是叶子节点（没有子节点）
          if (current.left === null && current.right == null) {
          if (current === this.root) {
          this.root = null
          } else if (isLeftChild) {
          parent.left = null
          } else {
          console.log('ok');

          parent.right = null
          }
          }
          // 2、2.删除的节点有一个子节点
          else if (current.right === null) {
          if (current === this.root) {
          this.root = current.left
          }
          else if (isLeftChild) {
          parent.left = current.left
          } else {
          parent.right = current.left
          }
          }
          else if (current.left === null) {
          if (current === this.root) {
          this.root = current.right
          }
          else if (isLeftChild) {
          parent.left = current.right
          } else {
          parent.right = current.right
          }
          }
          //2.3.删除的节点有两边子节点
          else {
          //1、获取后继节点
          var successor = this.getSuccssor(current)

          //2、判断是否根节点
          if (current === this.root) {
          this.root = successor
          } else if (isLeftChild) {
          parent.left = successor
          } else {
          parent.right = successor
          }

          //3、将删除节点的左子树  = current.left
          successor.left = current.left
          }
          }
          //找后继的方法
          BinarySearchTree.prototype.getSuccssor = function (delNode) {
          // 1、定义变量，保存找到的后推
          var successor = delNode
          var current = delNode.right
          var successorParent = delNode

          // 2、循环查找
          while (current !== null) {
          successorParent = successor
          successor = current
          current = current.left
          }

          // 3、判断寻找的后继节点是否直接就是delNode的right节点
          if (successor !== delNode.right) {
          successorParent.left = successor.right
          successor.right = delNode.right
          }
          return successor
          }
     }

     //测试代码
     var bst = new BinarySearchTree()
     bst.insert(11)
     bst.insert(1)
     bst.insert(14)
     bst.insert(6)
     bst.insert(131)
     bst.insert(171)
     bst.insert(8)
     bst.insert(9)
     bst.insert(2)
     console.log(bst);

     bst.preOrderTraversal()
     bst.midOrderTraversal()
     bst.postOrderTraversal()
     console.log(bst.min());
     console.log(bst.max());
     console.log(bst.search(111));

     //测试删除代码
     bst.remove(6)
     console.log(bst);
```

###### 二叉搜索树的缺陷

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182930.png" alt="image-20210217003627441" style="zoom: 50%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182932.png" alt="image-20210217004018720" style="zoom: 67%;" />

#### 图结构

##### 图简介

图看起来就像下图这样：

![img](https:////upload-images.jianshu.io/upload_images/4064751-9ef7887aca675269.png?imageMogr2/auto-orient/strip|imageView2/2/w/526/format/webp)

在计算机科学中，一个图就是一些*顶点*的集合，这些顶点通过一系列*边*结对（连接）。顶点用圆圈表示，边就是这些圆圈之间的连线。顶点之间通过边连接。

**注意：**顶点有时也称为节点或者交点，边有时也称为链接。

一个图可以表示一个社交网络，每一个人就是一个顶点，互相认识的人之间通过边联系。

![img](https:////upload-images.jianshu.io/upload_images/4064751-98d670ae394f3695.png?imageMogr2/auto-orient/strip|imageView2/2/w/651/format/webp)

图有各种形状和大小。边可以有*权重（weight）*，即每一条边会被分配一个正数或者负数值。考虑一个代表航线的图。各个城市就是顶点，航线就是边。那么边的权重可以是飞行时间，或者机票价格。

![img](https:////upload-images.jianshu.io/upload_images/4064751-7d75da02d729e64c.png?imageMogr2/auto-orient/strip|imageView2/2/w/824/format/webp)

有了这样一张假设的航线图。从旧金山到莫斯科最便宜的路线是到纽约转机。

边可以是*有方向的*。在上面提到的例子中，边是没有方向的。例如，如果 Ada 认识 Charles，那么 Charles 也就认识 Ada。相反，有方向的边意味着是单方面的关系。一条从顶点 X 到 顶点 Y 的边是将 X 联向 Y，不是将 Y 联向 X。

继续前面航班的例子，从旧金山到阿拉斯加的朱诺有向边意味着从旧金山到朱诺有航班，但是从朱诺到旧金山没有（我假设那样意味着你需要走回去）。

![img](https:////upload-images.jianshu.io/upload_images/4064751-c9ece5586fa955f8.png?imageMogr2/auto-orient/strip|imageView2/2/w/824/format/webp)

下面的两种情况也是属于图：

![img](https:////upload-images.jianshu.io/upload_images/4064751-0f7469ff0be704de.png?imageMogr2/auto-orient/strip|imageView2/2/w/485/format/webp)

左边的是树，右边的是链表。他们都可以被当成是树，只不过是一种更简单的形式。他们都有顶点（节点）和边（连接）。

第一种图包含*圈（cycles）*，即你可以从一个顶点出发，沿着一条路劲最终会回到最初的顶点。树是不包含圈的图。

另一种常见的图类型是单向图或者 DAG：

![img](https:////upload-images.jianshu.io/upload_images/4064751-af16c3d5a506e610.png?imageMogr2/auto-orient/strip|imageView2/2/w/634/format/webp)

就像树一样，这个图没有任何圈（无论你从哪一个节点出发，你都无法回到最初的节点），但是这个图有有向边（通过一个箭头表示，这里的箭头不表示继承关系）。

##### 为什么要使用图？

也许你耸耸肩然后心里想着，有什么大不了的。好吧，事实证明图是一种有用的数据结构。

如果你有一个编程问题可以通过顶点和边表示出来，那么你就可以将你的问题用图画出来，然后使用著名的图算法（比如广度优先搜索 或者 深度优先搜索）来找到解决方案。

例如，假设你有一系列任务需要完成，但是有的任务必须等待其他任务完成后才可以开始。你可以通过非循环有向图来建立模型：

![img](https:////upload-images.jianshu.io/upload_images/4064751-afa3948a9e805a67.png?imageMogr2/auto-orient/strip|imageView2/2/w/457/format/webp)

每一个顶点代表一个任务。两个任务之间的边表示目的任务必须等到源任务完成后才可以开始。比如，在任务B和任务D都完成之前，任务C不可以开始。在任务A完成之前，任务A和D都不能开始。

现在这个问题就通过图描述清楚了，你可以使用深度优先搜索算法来执行执行拓扑排序。这样就可以将所有的任务排入最优的执行顺序，保证等待任务完成的时间最小化。（这里可能的顺序之一是：A, B, D, E, C, F, G, H, I, J, K）

不管是什么时候遇到困难的编程问题，问一问自己：“如何用图来表述这个问题？”。图都是用于表示数据之间的关系。 诀窍在于如何定义“关系”。

程序员常用的另一个图就是状态机，这里的边描述了状态之间切换的条件。下面这个状态机描述了一个猫的状态：

![img](https:////upload-images.jianshu.io/upload_images/4064751-8f5334960c871d0b.png?imageMogr2/auto-orient/strip|imageView2/2/w/500/format/webp)

图真的很棒。Facebook 就从他们的社交图中赚取了巨额财富。如果计划学习任何数据结构，则应该选择图，以及大量的标准图算法。

##### 顶点的表示

**图的一些概念**

- **顶点的度** - 指与该顶点相关联的边的条数。
- **路径** - 从一个顶点到另一个顶点的路径，存在0个或多个。
  - 简单路径：简单路径要求不包含重复的顶点
  - 回路：第一个顶点和最后一个顶点相同的路径称为回路
- **权** - 路径上附属携带了数据信息。
- **连通图**指任意两个顶点之间存在关系边；
- **无向图**：图的边是没有方向的
- **有向图**：图的边是由方向的
- **带权图**：带权图的边有一定的权重

理论上，图就是一堆顶点和边对象而已，但是怎么在代码中来描述呢？

有两种主要的方法：邻接矩阵和邻接列表。

**邻接矩阵：**在邻接矩阵实现中，由行和列都表示顶点，由两个顶点所决定的矩阵对应元素表示这里两个顶点是否相连、如果相连这个值表示的是相连边的权重。例如，如果从顶点A到顶点B有一条权重为 5.6 的边，那么矩阵中第A行第B列的位置的元素值应该是5.6：

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182940.png" alt="image-20210217213702542" style="zoom: 50%;" />

往这个图中添加顶点的成本非常昂贵，因为新的矩阵结果必须重新按照新的行/列创建，然后将已有的数据复制到新的矩阵中。

在 *稀疏*图的情况下，每一个顶点都只会和少数几个顶点相连，这种情况下相邻列表是最佳选择。如果这个图比较*密集*，每一个顶点都和大多数其他顶点相连，那么相邻矩阵更合适。

**邻接列表**：在邻接列表实现中，每一个顶点会存储一个从它这里开始的边的列表。比如，如果顶点A 有一条边到B、C和D，那么A的列表中会有3条边

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182947.png" alt="image-20210217214143571" style="zoom: 67%;" />

所以使用哪一个呢？`大多数时候，选择邻接列表是正确的`。下面是两种实现方法更详细的比较。

假设 *V* 表示图中顶点的个数，*E* 表示边的个数。

| 操作       | 邻接列表 | 邻接矩阵 |
| ---------- | -------- | -------- |
| 存储空间   | O(V + E) | O(V^2)   |
| 添加顶点   | O(1)     | O(V^2)   |
| 添加边     | O(1)     | O(1)     |
| 检查相邻性 | O(V)     | O(1)     |

“检查相邻性” 是指对于给定的顶点，尝试确定它是否是另一个顶点的邻居。在邻接列表中检查相邻性的时间复杂度是**O(V)**，因为最坏的情况是一个顶点与*每一个*顶点都相连。

在 *稀疏*图的情况下，每一个顶点都只会和少数几个顶点相连，这种情况下相邻列表是最佳选择。如果这个图比较*密集*，每一个顶点都和大多数其他顶点相连，那么相邻矩阵更合适。

##### 图的实现

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182951.png" alt="image-20210217214656033" style="zoom:67%;" />

我们希望从图中某一顶点出发访遍图中其余顶点，且是每一个顶点仅被访问一次，这个过程就叫做图的遍历

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227182955.png" alt="image-20210218001329867" style="zoom:50%;" />



<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227183000.png" alt="image-20210218003559608" style="zoom: 67%;" />

<img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227183008.png" alt="image-20210218015619367" style="zoom:67%;" />

### 算法（algorithm）

#### 简介

> Algorithm这个单词本意就是解决问题的办法/步骤逻辑
>
> 算法是解决特定问题求解步骤的描述，在计算机中表现为指令的有限序列，并且每条指令表示一个或多个操作。

![image-20210115122327003](https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227183014.png)

#### 算法的基本特性

> 算法具有五个基本特性：输入、输出、有穷性、确定性和可行性

#### 算法设计的要求

> 算法设计的要求：正确性、可读性、健壮性、时间效率高和储存量低

正确性：算法的正确性是指算法至少应该具有输入、输出和加工处理无歧义性、能正确反映问题的需求、能够得到问题的正确答案。

可读性：算法设计的另一目的是为了便于阅读、理解和交流。

健壮性：当输入数据不合法是，算法也能够做出相关的处理，而不是产生异常或莫名其妙的结果。

时间效率：时间效率指的是算法的执行时间，对于同一个问题，如果有多个算法能够解决，执行效率短的算法效率高，执行时间长的效率低。

储存量低：储存量需求指的是算法在执行过程中需要的最大储存空间，主要指算法程序运行时所占的内存或外部硬盘储存空间。	

#### 算法的度量

> 算法的度量方法：事后统计方法（不科学、不准确）、事前分析估算方法
>
> 事前分析估算方法`主要利用算法的时间复杂度和算法的空间复杂度的对比`

##### 算法时间复杂度

> 在进行算法分析时，语句总的执行次数T(n)是关于问题规模n的函数，进而分析T(n)随n的变化情况并确定T(n)的数量级。
>
> 算法的时间复杂度，也就是算法的时间度量，记住：T(n) = O(f(n))。它表示随问题规模n的增大，算法执行时间的增长率和f(n)的增长率相同，
>
> 称作算法的渐进时间复杂度，简称为时间复杂度。其中f(n)是问题规模n的某个函数。

这样用大写O( )来体现算法时间复杂度的记法，我们称之为大O记法。

一般情况下，随着n的增大，T(n)增长最慢的算法为最优算法。

**推导大O阶方法**

​	1、用常数1取代运行时间中的所有加法常数。

​	2、在修改后的运行次数函数中，只保留最高阶项。

​	3、如果最高阶项存在且不是1，则去除与这个项目相乘的常数。得到的结果就是大O阶。

**推导示例**

**1、常数阶**

   首先顺序结构的时间复杂度。下面这个算法，是利用高斯定理计算1，2，……n个数的和。

```
 int sum = 0, n = 100; /*执行一次*/ sum = (1 + n) * n / 2; /*执行一次*/  printf("%d",sum); /*执行一次*/
```

   这个算法的运行次数函数是f (n) =3。 根据我们推导大0阶的方法，第一步就是把常数项3 改为1。在保留最高阶项时发现，它根本没有最高阶项，所以这个算法的时间复杂度为**0(1)**。
   另外，我们试想一下，如果这个算法当中的语句 sum = (1+n)*n/2; 有10 句，则与示例给出的代码就是3次和12次的差异。`这种与问题的大小无关（n的多少），执行时间恒定的算法，我们称之为具有O(1)的时间复杂度，又叫常数阶。`对于分支结构而言，无论是真，还是假，执行的次数都是恒定的，不会随着n 的变大而发生变化，所以单纯的分支结构(不包含在循环结构中)，其时间复杂度也是0(1)。

**2、线性阶**

  线性阶的循环结构会复杂很多。要确定某个算法的阶次，我们常常需要确定某个特定语句或某个语句集运行的次数。因此，我们要分析算法的复杂度，关键就是要分析循环结构的运行情况。

  下面这段代码，它的循环的时间复杂度为**O(n)**， 因为循环体中的代码须要执行n次。

```
 int i;   for(i = 0; i < n; i++){  /*时间复杂度为O(1)的程序步骤序列*/ }
```

**3、对数阶**

  如下代码：

```
 int count = 1;   while (count < n){  count = count * 2;   /*时间复杂度为O(1)的程序步骤序列*/  }
```

  由于每次count乘以2之后，就距离n更近了一分。 也就是说，有多少个2相乘后大于n，则会退出循环。 由2^x=n 得到x=logn。 所以这个循环的时间复杂度为**O(logn)**。

**4、平方阶**

  下面例子是一个循环嵌套，它的内循环刚才我们已经分析过，时间复杂度为O(n)。

```
  int i, j;     for(i = 0; i < n; i++){    　　for(j = 0; j < n; j++){   /*时间复杂度为O(1)的程序步骤序列*/    　　}  }
```

  而对于外层的循环，不过是内部这个时间复杂度为O(n)的语句，再循环n次。 所以这段代码的时间复杂度为O(n^2)。

  如果外循环的循环次数改为了m，时间复杂度就变为O(mXn)。

  所以我们可以总结得出，循环的时间复杂度等于循环体的复杂度乘以该循环运行的次数。
  那么下面这个循环嵌套，它的时间复杂度是多少呢?

```
  int i, j;      for(i = 0; i < n; i++){   　　for(j = i; j < n; j++){ /*注意j = i而不是0*/     　　/*时间复杂度为O(1)的程序步骤序列*/     　　}   }
```

  由于当i=0时，内循环执行了n次，当i = 1时，执行了n-1次，……当i=n-1时，执行了1次。所以总的执行次数为:

![img](https://img-blog.csdn.net/20170327144317062)

  用我们推导大O阶的方法，第一条，没有加法常数不予考虑；第二条，只保留最高阶项，因此保留时(n^2)/2; 第三条，去除这个项相乘的常数，也就是去除1/2，最终这段代码的时间复杂度为O(n2)。

  从这个例子，我们也可以得到一个经验，其实理解大0推导不算难，难的是**对数列的一些相关运算，这更多的是考察你的数学知识和能力。**

`注：判读一个算法的效率时，函数中的常数和其他次要项常常可以忽略，而更应该关注主项（最高阶项）的阶数`

**常见的时间复杂度**

  常见的时问复杂度如表所示。
![img](https://img-blog.csdn.net/20170327162904721)
  常用的时间复杂度所耗费的时间从小到大依次是：
![img](https://img-blog.csdn.net/20170327162956884)
  我们前面已经谈到了。O(1)常数阶、O(logn)对数阶、O(n)线性阶、 O(n^2)平方阶等，像O(n^3)，过大的n都会使得结果变得不现实。同样指数阶O(2^n)和阶乘阶O(n!)等除非是很小的n值，否则哪怕n 只是100，都是噩梦般的运行时间。所以这种[不切实际](https://www.baidu.com/s?wd=不切实际&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)的算法时间复杂度，一般我们都不去讨论。

**最坏情况与平均情况**

  我们查找一个有n 个随机数字数组中的某个数字，最好的情况是第一个数字就是，那么算法的时间复杂度为O(1)，但也有可能这个数字就在最后一个位置上待着，那么算法的时间复杂度就是O(n)，这是最坏的一种情况了。
  `最坏情况运行时间是一种保证，那就是运行时间将不会再坏了。 在应用中，这是一种最重要的需求， 通常， 除非特别指定， 我们提到的运行时间都是最坏情况的运行时间。`
  而平均运行时间也就是从概率的角度看， 这个数字在每一个位置的可能性是相同的，所以平均的查找时间为n/2次后发现这个目标元素。`平均运行时间是所有情况中最有意义的，因为它是期望的运行时间。`也就是说，我们运行一段程序代码时，是希望看到平均运行时间的。可现实中，平均运行时间很难通过分析得到，一般都是通过运行一定数量的实验数据后估算出来的。`一般在没有特殊说明的情况下，都是指最坏时间复杂度。`

##### 算法空间复杂度

  我们在写代码时，完全可以用空间来换取时间，比如说，要判断某某年是不是[闰年](https://www.baidu.com/s?wd=闰年&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)，你可能会花一点心思写了一个算法，而且由于是一个算法，也就意味着，每次给一个年份，都是要通过计算得到是否是闰年的结果。 还有另一个办法就是，事先建立一个有2050个元素的数组(年数略比现实多一点)，然后把所有的年份按下标的数字对应，如果是闰年，此数组项的值就是1，如果不是值为0。这样，所谓的判断某一年是否是闰年，就变成了查找这个数组的某一项的值是多少的问题。此时，我们的运算是最小化了，但是硬盘上或者内存中需要存储这2050个0和1。这是通过一笔空间上的开销来换取计算时间的小技巧。到底哪一个好，其实要看你用在什么地方。
  **算法的空间复杂度通过计算算法所需的存储空间实现，算法空间复杂度的计算公式记作:S(n)= O(f(n))，其中，n为问题的规模，f(n)为语句关于n所占存储空间的函数。**
  一般情况下，一个程序在机器上执行时，除了需要存储程序本身的指令、常数、变量和输入数据外，还需要存储对数据操作的存储单元，若输入数据所占空间只取决于问题本身，和算法无关，这样只需要分析该算法在实现时所需的辅助单元即可。`若算法执行时所需的辅助空间相对于输入数据量而言是个常数，则称此算法为原地工作，空间复杂度为0(1)。`
   通常， 我们都使用"时间复杂度"来指运行时间的需求，使用"空间复杂度"指空间需求。当不用限定词地使用"复杂度'时，通常都是指时间复杂度。

**一些计算的规则**

1、加法规则

   T(n,m) = T1(n) + T2(m) = O(max{f(n), g(m)})

2、乘法规则

   T(n,m) = T1(n) * T2(m) = O(max{f(n)*g(m)})

3、一个经验

   复杂度与时间效率的关系：
  c(常数) < logn < n < n*logn < n^2 < n^3 < 2^n < 3^n < n!
  l------------------------------l--------------------------l--------------l
          较好             一般           较差

**常用算法的时间复杂度和空间复杂度**

![img](https://img-blog.csdn.net/20170327164357916)

#### 排序算法

##### 简介

0、排序算法说明

- 0.1 排序的定义
  对一序列对象根据某个关键字进行排序。

- 0.2 术语说明

  - **稳定** ：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
  - **不稳定** ：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；不稳定排序记忆口诀：**快**（快排）**些**（希尔）**选**（选择）**一堆**（堆排）
  - **内排序** ：所有排序操作都在内存中完成；
  - **外排序** ：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
  - **时间复杂度** ： 一个算法执行所耗费的时间。
  - **空间复杂度** ：运行完一个程序所需内存的大小。

- 0.3 算法总结

- <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzMwNDMxNjgtMTg2NzgxNzg2OS5wbmc?x-oss-process=image/format,png" alt="image" style="zoom:67%;" />

- 图片名词解释：

  - n: 数据规模
  - k: “桶”的个数
  - In-place: 占用常数内存，不占用额外内存
  - Out-place: 占用额外内存

  

- 0.5 算法分类

  ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzMyMjA2MzctMTA1NTA4ODExOC5wbmc?x-oss-process=image/format,png)

- 0.6 比较和非比较的区别

  常见的**快速排序、归并排序、堆排序、冒泡排序** 等属于**比较排序** 。在排序的最终结果里，元素之间的次序依赖于它们之间的比较。每个数都必须和其他数进行比较，才能确定自己的位置 。

  在冒泡排序之类的排序中，问题规模为n，又因为需要比较n次，所以平均时间复杂度为O(n²)。在归并排序、快速排序之类的排序中，问题规模通过分治法消减为logN次，所以时间复杂度平均O(nlogn)。

  比较排序的优势是，适用于各种规模的数据，也不在乎数据的分布，都能进行排序。可以说，比较排序适用于一切需要排序的情况。

  **计数排序、基数排序、桶排序**则属于**非比较排序** 。非比较排序是通过确定每个元素之前，应该有多少个元素来排序。针对数组arr，计算arr[i]之前有多少个元素，则唯一确定了arr[i]在排序后数组中的位置 。

  非比较排序只要确定每个元素之前的已有的元素个数即可，所有一次遍历即可解决。算法时间复杂度O(n)。

  非比较排序时间复杂度底，但由于非比较排序需要占用空间来确定唯一位置。所以对数据规模和数据分布有一定的要求。

##### 冒泡排序（Bubble Sort）

  **冒泡排序** 是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

 1.3 代码实现

- 1.1 算法描述

  - 步骤1: 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
  - 步骤2: 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
  - 步骤3: 针对所有的元素重复以上的步骤，除了最后一个；
  - 步骤4: 重复步骤1~3，直到排序完成。

- 1.2 动图演示

- ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMjMyMzg0NDktMjE0NjE2OTE5Ny5naWY)

- ```
  function bubbleSort(array) {
    for (let i = array.length - 1; i >= 0 ; i--){
    //反向循环，因此次数越来越少
     //根据j的次数，比较到j位置,因为每次最后面的数都是最大的，所以不需要再交换了
      for (let j = 0; j < i; j++) {
        if(array[j] > array[j+1]){
          let temp = array[j]
          array[j] = array[j+1]
          array[j+1] = temp
        }
      }
    }
  }
  let arr = [2,5,1,10,0,100,12,1]
  console.log(arr);
  bubbleSort(arr)
  console.log(arr);
  ```

  > 1.4 算法分析
  >
  > - 最佳情况：T(n) = O(n)
  > - 最差情况：T(n) = O(n^2)
  > - 平均情况：T(n) = O(n^2)

##### 选择排序（Selection Sort）

  **选择排序** 是表现最稳定的排序算法之一 ，**因为无论什么数据进去都是O(n^2)的时间复杂度** ，所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。理论上讲，选择排序可能也是平时排序一般人想到的最多的排序方法了吧。

  **选择排序(Selection-sort)** 是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

2.3 代码实现 

- 2.1 算法描述

  n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：

  - 步骤1：初始状态：无序区为R[1…n]，有序区为空；
  - 步骤2：第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1…i-1]和R(i…n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1…i]和R[i+1…n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
  - 步骤3：n-1趟结束，数组有序化了。
  - <img src="https://gitee.com/gitopenchina/typora-image/raw/master/img/20210227183028.png" alt="image-20210218131631217" style="zoom: 67%;" />

- 2.2 动图演示

- ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMjQ3MTk1OTAtMTQzMzIxOTgyNC5naWY)

- ```
  //根本思想就是使用标记找到最小的数，放到数组前面
  function selectionSort (array) {
    //1、获取数组的长度
    var length = array.length
    for (let j = 0; j < length; j++) {
      var min = j
      for (let i = min + 1; i < length; i++) {
        if (array[min] > array[i]) {
          min = i
        }
      }
      let temp = array[min]
      array[min] = array[j]
      array[j] = temp
  
    }
  }
  let arr = [2, 5, 1, 10, 0, 100, 12, 1]
  console.log(arr);
  selectionSort(arr)
  console.log(arr)
  ```

  > - 2.4 算法分析
  > - 最佳情况：T(n) = O(n^2)
  > - 最差情况：T(n) = O(n^2)
  > - 平均情况：T(n) = O(n^2)

##### 插入排序（Insertion Sort）

  **插入排序（Insertion-Sort）** 的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，通常采用in-place排序（即只需用到O(1)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

 

- 3.1 算法描述

  一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：

  1.默认从 i = 1 开始判断，这样 preIndex 自然是内部循环的游标；

  2.current 保存 arr[i]，通过循环来确定 current 的最终位置；

  3.每个内循环开始的时候，arr[i] === current === arr[preIndex + 1]，所以在内循环首次时 arr[preIndex + 1] = arr[preIndex] 的时候不必担心 arr[i] 的值  	丢失；

  4.总体思路是，需要排位的元素先额外缓存起来，然后套用内循环，使得需要调整的元素赋值给它后面的一个位置上，形成依次挪位，最后因为内循环在判断条	件不生效的时候停止意味着找到了需要排位的元素的正确位置，然后赋值上去，完成排序。

- 3.2 动图演示

- ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMjU2NDUyNzctMTE1MTEwMDAwMC5naWY)

- 3.3 代码实现

- ```
  function insertSort (array) {
  let len = arr.length;
    let preIndex, current;
    for (let i = 1; i < len; i++) {
      preIndex = i - 1;
      current = arr[i];
      while (preIndex >= 0 && current < arr[preIndex]) {
        arr[preIndex + 1] = arr[preIndex];
        preIndex--;
      }
      arr[preIndex + 1] = current;
    }
    return arr;
  }
  let arr = [2, 5, 1, 10, 0, 100, 12, 1, 111, 6, 1, 76, 23, 18, 0]
  console.log(arr);
  insertSort(arr)
  console.log(arr);
  ```

- 3.4 算法分析

- 最佳情况：T(n) = O(n)

- 最坏情况：T(n) = O(n2)

- 平均情况：T(n) = O(n2)

##### 希尔排序（Shell Sort）

  **希尔排序是希尔（Donald Shell）** 于1959年提出的一种排序算法。希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，也称为缩小增量排序，同时该算法是冲破O(n2）的第一批算法之一。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序。

  希尔排序是把记录按下表的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止。

- 4.1 算法描述

  我们来看下希尔排序的基本步骤，在此我们选择增量gap=length/2，缩小增量继续以gap = gap/2的方式，这种增量选择我们可以用一个序列来表示，{n/2,(n/2)/2…1}，称为增量序列。希尔排序的增量序列的选择与证明是个数学难题，我们选择的这个增量序列是比较常用的，也是希尔建议的增量，称为希尔增量，但其实这个增量序列不是最优的。此处我们做示例使用希尔增量。

  先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：

  - 步骤1：选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1；
  - 步骤2：按增量序列个数k，对序列进行k 趟排序；
  - 步骤3：每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

- 4.2 过程演示

  <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMTE5MjY5OS8yMDE4MDMvMTE5MjY5OS0yMDE4MDMxOTA5NDExNjA0MC0xNjM4NzY2MjcxLnBuZw?x-oss-process=image/format,png" alt="image" style="zoom: 67%;" />

- 4.3 代码实现

  ```
  function shellSort (arr) {
    let len = arr.length;
    // gap 即为增量
    for (let gap = Math.floor(len / 2); gap > 0; gap = Math.floor(gap / 2)) {
      for (let i = gap; i < len; i++) {
        let j = i;
        let current = arr[i];
        while (j - gap >= 0 && current < arr[j - gap]) {
          arr[j] = arr[j - gap];
          j = j - gap;
        }
        arr[j] = current;
      }
    }
  }
  
  var arr = [3, 5, 7, 1, 4, 56, 12, 78, 25, 0, 9, 8, 42, 37];
  shellSort(arr)
  console.log(arr);
  ```

  4.4 算法分析

- 最佳情况：T(n) = O(nlog2 n)

- 最坏情况：T(n) = O(nlog2 n)

- 平均情况：T(n) =O(nlog2n)

##### 快速排序（Quick Sort）

  **快速排序** 的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

- 6.1 算法描述

  快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：

  - 步骤1：从数列中挑出一个元素，称为 “基准”（**pivot** ）；
  - 步骤2：重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
  - 步骤3：递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

- 6.2 动图演示

- ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzA5MzYzNzEtMTQxMzUyMzQxMi5naWY)

- 6.3 代码实现

- ```
  function quickSort (arr) {
    //当array中小于等于一项，则不用处理
    if (arr.length <= 1) {
      return arr;
    }
    let pivotIndex = Math.floor(arr.length / 2);
    let pivot = arr.splice(pivotIndex, 1)[0];
    let left = [];
    let right = [];
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] < pivot) {
        left.push(arr[i]);
      } else {
        right.push(arr[i]);
      }
    }
    return quickSort(left).concat([pivot], quickSort(right));
  }
  let arr = [3, 9, 5, 4, 10, 19, 17, 22, 5];
  console.log(quickSort(arr));
  ```

  > 6.4 算法分析
  >
  > 最佳情况：T(n) = O(nlogn)
  >
  > 最差情况：T(n) = O(n2)
  >
  > 平均情况：T(n) = O(nlogn)



> ## 5、归并排序（Merge Sort）
>
> 和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(n log n）的时间复杂度。代价是需要额外的内存空间。
>
> **归并排序** 是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。归并排序是一种稳定的排序方法。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。
>
> - 5.1 算法描述
>
>   - 步骤1：把长度为n的输入序列分成两个长度为n/2的子序列；
>   - 步骤2：对这两个子序列分别采用归并排序；
>   - 步骤3：将两个排序好的子序列合并成一个最终的排序序列。
>
> - 5.2 动图演示
>
>   ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzA1NTcwNDMtMzczNzUwMTAuZ2lm)
>
> - 5.3 代码实现
>
> ```
> /**
>      * 归并排序
>      *
>      * @param array
>      * @return
>      */
>     public static int[] MergeSort(int[] array) {
>         if (array.length < 2) return array;
>         int mid = array.length / 2;
>         int[] left = Arrays.copyOfRange(array, 0, mid);
>         int[] right = Arrays.copyOfRange(array, mid, array.length);
>         return merge(MergeSort(left), MergeSort(right));
>     }
>     /**
>      * 归并排序——将两段排序好的数组结合成一个排序数组
>      *
>      * @param left
>      * @param right
>      * @return
>      */
>     public static int[] merge(int[] left, int[] right) {
>         int[] result = new int[left.length + right.length];
>         for (int index = 0, i = 0, j = 0; index < result.length; index++) {
>             if (i >= left.length)
>                 result[index] = right[j++];
>             else if (j >= right.length)
>                 result[index] = left[i++];
>             else if (left[i] > right[j])
>                 result[index] = right[j++];
>             else
>                 result[index] = left[i++];
>         }
>         return result;
>     }
> ```
>
> 5.4 算法分析
>
> - 最佳情况：T(n) = O(n)
> - 最差情况：T(n) = O(nlogn)
> - 平均情况：T(n) = O(nlogn)

> ## 7、堆排序（Heap Sort）
>
> **堆排序（Heapsort）** 是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。
>
> - 7.1 算法描述
>
>   - 步骤1：将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区；
>   - 步骤2：将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足R[1,2…n-1]<=R[n]；
>   - 步骤3：由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。
>
> - 7.2 动图演示
>
>   ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzEzMDg2OTktMzU2MTM0MjM3LmdpZg)
>
> - 7.3 代码实现
>
>   注意：这里用到了完全二叉树的部分性质：详情见数据结构二叉树知识点
>
> ```
> //声明全局变量，用于记录数组array的长度；    static int len;    /**     * 堆排序算法     *     * @param array     * @return     */    public static int[] HeapSort(int[] array) {        len = array.length;        if (len < 1) return array;        //1.构建一个最大堆        buildMaxHeap(array);        //2.循环将堆首位（最大值）与末位交换，然后在重新调整最大堆        while (len > 0) {            swap(array, 0, len - 1);            len--;            adjustHeap(array, 0);        }        return array;    }    /**     * 建立最大堆     *     * @param array     */    public static void buildMaxHeap(int[] array) {        //从最后一个非叶子节点开始向上构造最大堆        //for循环这样写会更好一点：i的左子树和右子树分别2i+1和2(i+1)        for (int i = (len/2- 1); i >= 0; i--) {            adjustHeap(array, i);        }    }    /**     * 调整使之成为最大堆     *     * @param array     * @param i     */    public static void adjustHeap(int[] array, int i) {        int maxIndex = i;        //如果有左子树，且左子树大于父节点，则将最大指针指向左子树        if (i * 2 < len && array[i * 2] > array[maxIndex])            maxIndex = i * 2 + 1;        //如果有右子树，且右子树大于父节点，则将最大指针指向右子树        if (i * 2 + 1 < len && array[i * 2 + 1] > array[maxIndex])            maxIndex = i * 2 + 2;         //如果父节点不是最大值，则将父节点与最大值交换，并且递归调整与父节点交换的位置。        if (maxIndex != i) {            swap(array, maxIndex, i);            adjustHeap(array, maxIndex);        }    }
> ```
>
> 7.4 算法分析
>
> - 最佳情况：T(n) = O(nlogn)
> - 最差情况：T(n) = O(nlogn)
> - 平均情况：T(n) = O(nlogn)

> ## 8、计数排序（Counting Sort）
>
> **计数排序** 的核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。 作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。
>
> **计数排序(Counting sort)** 是一种稳定的排序算法。计数排序使用一个额外的数组C，其中第i个元素是待排序数组A中值等于i的元素的个数。然后根据数组C来将A中的元素排到正确的位置。它只能对整数进行排序。
>
> - 8.1 算法描述
>
>   - 步骤1：找出待排序的数组中最大和最小的元素；
>   - 步骤2：统计数组中每个值为i的元素出现的次数，存入数组C的第i项；
>   - 步骤3：对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）；
>   - 步骤4：反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1。
>
> - 8.2 动图演示
>
>   ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzE3NDA4NDAtNjk2ODE4MS5naWY)
>
> - 8.3 代码实现
>
> ```
>     /**     * 计数排序     *     * @param array     * @return     */    public static int[] CountingSort(int[] array) {        if (array.length == 0) return array;        int bias, min = array[0], max = array[0];        for (int i = 1; i < array.length; i++) {            if (array[i] > max)                max = array[i];            if (array[i] < min)                min = array[i];        }        bias = 0 - min;        int[] bucket = new int[max - min + 1];        Arrays.fill(bucket, 0);        for (int i = 0; i < array.length; i++) {            bucket[array[i] + bias]++;        }        int index = 0, i = 0;        while (index < array.length) {            if (bucket[i] != 0) {                array[index] = i - bias;                bucket[i]--;                index++;            } else                i++;        }        return array;    }
> ```
>
> 8.4 算法分析
>
>   当输入的元素是n 个0到k之间的整数时，它的运行时间是 O(n + k)。计数排序不是比较排序，排序的速度快于任何比较排序算法。由于用来计数的数组C的长度取决于待排序数组中数据的范围（等于待排序数组的最大值与最小值的差加上1），这使得计数排序对于数据范围很大的数组，需要大量时间和内存。
>
> - 最佳情况：T(n) = O(n+k)
> - 最差情况：T(n) = O(n+k)
> - 平均情况：T(n) = O(n+k)

> ## 9、桶排序（Bucket Sort）
>
> **桶排序** 是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。
>
> **桶排序 (Bucket sort)的工作的原理：**
> 假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排
>
> - 9.1 算法描述
>
>   - 步骤1：人为设置一个BucketSize，作为每个桶所能放置多少个不同数值（例如当BucketSize==5时，该桶可以存放｛1,2,3,4,5｝这几种数字，但是容量不限，即可以存放100个3）；
>
>   - 步骤2：遍历输入数据，并且把数据一个一个放到对应的桶里去；
>
>   - 步骤3：对每个不是空的桶进行排序，可以使用其它排序方法，也可以递归使用桶排序；
>
>   - 步骤4：从不是空的桶里把排好序的数据拼接起来。 
>
>     注意，如果递归使用桶排序为各个桶排序，则当桶数量为1时要手动减小BucketSize增加下一循环桶的数量，否则会陷入死循环，导致内存溢出。
>
> - 9.2 图片演示
>
>   ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzIxMDcwOTAtMTkyMDcwMjAxMS5wbmc?x-oss-process=image/format,png)
>
> - 9.3 代码实现
>
> ```
>  /**     * 桶排序     *     * @param array     * @param bucketSize     * @return     */    public static ArrayList<Integer> BucketSort(ArrayList<Integer> array, int bucketSize) {        if (array == null || array.size() < 2)            return array;        int max = array.get(0), min = array.get(0);        // 找到最大值最小值        for (int i = 0; i < array.size(); i++) {            if (array.get(i) > max)                max = array.get(i);            if (array.get(i) < min)                min = array.get(i);        }        int bucketCount = (max - min) / bucketSize + 1;        ArrayList<ArrayList<Integer>> bucketArr = new ArrayList<>(bucketCount);        ArrayList<Integer> resultArr = new ArrayList<>();        for (int i = 0; i < bucketCount; i++) {            bucketArr.add(new ArrayList<Integer>());        }        for (int i = 0; i < array.size(); i++) {            bucketArr.get((array.get(i) - min) / bucketSize).add(array.get(i));        }        for (int i = 0; i < bucketCount; i++) {            if (bucketSize == 1) { // 如果带排序数组中有重复数字时                for (int j = 0; j < bucketArr.get(i).size(); j++)                    resultArr.add(bucketArr.get(i).get(j));            } else {                if (bucketCount == 1)                    bucketSize--;                ArrayList<Integer> temp = BucketSort(bucketArr.get(i), bucketSize);                for (int j = 0; j < temp.size(); j++)                    resultArr.add(temp.get(j));            }        }        return resultArr;    }
> ```
>
> 9.4 算法分析
>
>   桶排序最好情况下使用线性时间O(n)，桶排序的时间复杂度，取决与对各个桶之间数据进行排序的时间复杂度，因为其它部分的时间复杂度都为O(n)。很显然，桶划分的越小，各个桶之间的数据越少，排序所用的时间也会越少。但相应的空间消耗就会增大。
>
> - 最佳情况：T(n) = O(n+k)
> - 最差情况：T(n) = O(n+k)
> - 平均情况：T(n) = O(n2)

##### 基数排序（Radix Sort）

  **基数排序**也是非比较的排序算法，对每一位进行排序，从最低位开始排序，复杂度为O(kn),为数组长度，k为数组中的数的最大的位数；

  **基数排序**是按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类推，直到最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序。最后的次序就是高优先级高的在前，高优先级相同的低优先级高的在前。基数排序基于分别排序，分别收集，所以是稳定的。

- 10.1 算法描述

  - 步骤1：取得数组中的最大数，并取得位数；
  - 步骤2：arr为原始数组，从最低位开始取每个位组成radix数组；
  - 步骤3：对radix进行计数排序（利用计数排序适用于小范围数的特点）；

- 10.2 动图演示

  ![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzI0NTM2NjgtMTM5NzY2MjUyNy5naWY)

- 10.3 代码实现

```
 /**



     * 基数排序



     * @param array



     * @return



     */



    public static int[] RadixSort(int[] array) {



        if (array == null || array.length < 2)



            return array;



        // 1.先算出最大数的位数；



        int max = array[0];



        for (int i = 1; i < array.length; i++) {



            max = Math.max(max, array[i]);



        }



        int maxDigit = 0;



        while (max != 0) {



            max /= 10;



            maxDigit++;



        }



        int mod = 10, div = 1;



        ArrayList<ArrayList<Integer>> bucketList = new ArrayList<ArrayList<Integer>>();



        for (int i = 0; i < 10; i++)



            bucketList.add(new ArrayList<Integer>());



        for (int i = 0; i < maxDigit; i++, mod *= 10, div *= 10) {



            for (int j = 0; j < array.length; j++) {



                int num = (array[j] % mod) / div;



                bucketList.get(num).add(array[j]);



            }



            int index = 0;



            for (int j = 0; j < bucketList.size(); j++) {



                for (int k = 0; k < bucketList.get(j).size(); k++)



                    array[index++] = bucketList.get(j).get(k);



                bucketList.get(j).clear();



            }



        }



        return array;



    }
```

- 10.4 算法分析

  - 最佳情况：T(n) = O(n * k)
  - 最差情况：T(n) = O(n * k)
  - 平均情况：T(n) = O(n * k)

- 10.5 基数排序有两种方法：

  - MSD 从高位开始进行排序
  - LSD 从低位开始进行排序

- **基数排序** vs **计数排序** vs **桶排序**

  这三种排序算法都利用了桶的概念，但对桶的使用方法上有明显差异：

  - **基数排序：** 根据键值的每位数字来分配桶
  - **计数排序：** 每个桶只存储单一键值
  - **桶排序：** 每个桶存储一定范围的数值

#### 查找算法

##### 二分查找

非递归

```
 function BinarySearch(arr, item) {
      let left = 0,right = arr.length - 1
      while(left <= right){
        let mid = Math.floor(left + right)
        if(arr[mid] === item){
          return mid
        }else if(item < arr[mid]){
          right = mid - 1
        }else{
          left = mid + 1
        }
      }
      return false
    }
    //使用二分查找必须是已经排好序的数组
    let arr = [1, 4, 8, 20, 77, 266, 888, 1999, 2222, 4982]
    console.log(BinarySearch(arr, 77));  //4
```

递归

```
function BinarySearch(arr,item,start,end) {
        var mid = Math.floor((start+end)/2)
        if(item === arr[mid]){
            return mid
        }
        else if(item<arr[mid]){
            return BinarySearch(arr,item,start,mid-1)
        }
        else{
            return BinarySearch(arr,item,mid+1,end)
        }
        return false
}
```



## 设计模式

> Design Pattern

什么是设计模式？

> 假设有一个空房间，我们要日复一日地往里 面放一些东西。最简单的办法当然是把这些东西 直接扔进去，但是时间久了，就会发现很难从这 个房子里找到自己想要的东西，要调整某几样东 西的位置也不容易。所以在房间里做一些柜子也 许是个更好的选择，虽然柜子会增加我们的成 本，但它可以在维护阶段为我们带来好处。使用 这些柜子存放东西的规则，或许就是一种模式

学习设计模式，有助于写出可复用和可维护性高的程序

设计模式的原则是“找出 程序中变化的地方，并将变化封装起来”，它的关键是意图，而不是结构。

不过要注意，使用不当的话，可能会事倍功半。

### 设计原则

**单一职责原则（SRP）**

一个对象或方法只做一件事情。如果一个方法承担了过多的职责，那么在需求的变迁过程中，需要改写这个方法的可能性就越大。

应该把对象或方法划分成较小的粒度

**最少知识原则（LKP）**

一个软件实体应当 尽可能少地与其他实体发生相互作用 

应当尽量减少对象之间的交互。如果两个对象之间不必彼此直接通信，那么这两个对象就不要发生直接的 相互联系，可以转交给第三方进行处理

**开放-封闭原则（OCP）**

软件实体（类、模块、函数）等应该是可以 扩展的，但是不可修改

当需要改变一个程序的功能或者给这个程序增加新功能的时候，可以使用增加代码的方式，尽量避免改动程序的源代码，防止影响原系统的稳定

### 工厂模式

> 工厂模式定义一个用于创建对象的接口，这个接口由子类决定实例化哪一个类。该模式使一个类的实例化延迟到了子类。而子类可以重写接口方法以便创建的时候指定自己的对象类型（抽象工厂）。
>
> 这个模式十分有用，尤其是创建对象的流程赋值的时候，比如依赖于很多设置文件等。并且，你会经常在程序里看到工厂方法，用于让子类定义需要创建的对象类型。
>
> 工厂模式根据抽象程度的不同可以分为：1.简单工厂 2.工厂方法 3.抽象工厂

#### 1.简单工厂

```
let  factory = function (role) {
function superman() {
    this.name ='超级管理员',
    this.role = ['修改密码', '发布消息', '查看主页']
}

function commonMan() {
    this.name = '普通游客',
    this.role = ['查看主页']
}

switch(role) {
    case 'superman':
    return new superman();
    break;
    case 'man':
    return new commonMan();
    break;
    default:
    throw new Error('参数错误')
}
}
let superman = factory('superman');
let man = factory('man');
```

在上述代码中,factory就是一个简单的工厂,该工厂中有二个构造函数分别对应不同的权限。我们只需要传递相应的参数就可以获取一个实例对象了。

简单工厂的优点: 你只需要传递一个合法的参数,就可以获取到你想要的对象,而无需知道创建的具体的细节。但是在函数内包含了所有对象的构造函数和判断逻辑的代码, 每次如果需要添加一个对象,那么我们需要新增一个构造函数,当我们需要维护的对象不是上面这2个,而是20个或者更多,那么这个函数将会成为超级函数,使得我们难以维护。所以简单工厂模式只适用于在创建时对象数量少,以及逻辑简单的情况。

#### 2.工厂方法

工厂方法模式本意是将实际创造的对象推迟到子类中,这样核心类就变成了抽象类。但是在js中很难像那些传统面向对象语言那样去实现抽象类,所以在js中我们只需要参考他的思想即可。

我们可以把工厂函数看成是一个工厂类。在简单模式我们,我们添加一个新的对象需要修改二处地方,在加入工厂方法模式以后,我们只需要修改一处即可。工厂方法的工厂类,他只做实例化这一件事情。我们只需要修改他的原型类即可。我们采用安全模式创建工厂对象。

```
    function Person(name) {
      this.name = name
    }
    Person.prototype.getName = function () {
      console.log(this.name);
    }
    function Car(model) {
      this.model = model
    }
    Car.prototype.getModel = function () {
      console.log(this.model);
    }
    function create(type, param) {
      if (this instanceof create) {
        return new this[type](param)
      } else {
        return new create(type, param)
      }
    }
    create.prototype = {
      person: Person,
      car: Car
    }
    var person1 = new create('person', 'zhang san')
    var car1 = create('car', 'Benz')
    console.log(person1);
    console.log(car1);
    person1.getName()
    car1.getModel()
```

在上述代码中要是忘记加new了, 那么我们就获取不到Person，Car等构造函数了,使用安全模式可以很好的解决这个问题。

**什么时候使用工厂模式**

工厂模式在应用于以下情况时尤其有用：

当我们创建的对象或组件涉及到了很高的复杂度。当我们需要根据所处的环境生成不同的对象实例时。当我们处理含有相同属性的对象或组件时。当创建的对象是其他对象的实例，而且要求它们有一致的API接口时。有利于解耦。

### 建造者模式

> 建造者模式可以将一个复杂的对象的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。也就是说如果我们用了建造者模式，那么用户就需要指定需要建造的类型就可以得到它们，而具体建造的过程和细节就不需要知道了。建造者模式实际就是一个指挥者，一个建造者，一个使用指挥者调用具体建造者工作得出结果的客户。
>
> 建造者模式主要用于“分步骤构建一个复杂的对象”，在这其中“分步骤”是一个稳定的算法，而复杂对象的各个部分则经常变化。

**建造者模式的作用和注意事项**

模式作用：

1.分步创建一个复杂的对象

2.解耦封装过程和具体创建组件

3.无需关心组件如何组装

注意事项：

1.一定要一个稳定的算法进行支持

2.加工工艺是暴露的

**普通代码**

```
	   // 应聘者信息
         var data = [
           {
             name: 'zhang san',
             age: 23,
             work: 'engineer'
           },
           {
             name: 'li si',
             age: 26,
             work: 'teacher'
           },
           {
             name: 'wang wu',
             age: 13,
             work: 'xxx'
           }
         ]
         //生成应聘者实例的类
         function Candidate(param) {
           var _candidate = {}
           _candidate.name = param.name
           _candidate.age = param.age
           _candidate.firstName = _candidate.name.split(' ')[0]
           _candidate.secondName = _candidate.name.split(' ')[1]
           _candidate.work = {}
         	function switchFun(work) {
             switch (work) {
               case 'engineer':
                 _candidate.work.name = '工程师';
                 _candidate.work.description = '热爱编程';
                 break;
               case 'teacher':
                 _candidate.work.name = '教师';
                 _candidate.work.description = '乐于分享';
                 break;
               default:
                 _candidate.work.name = work
                 _candidate.work.description = '无';
             }
           }
           switchFun(param.work)
           //更改工作的方法
           _candidate.work.changeWork = function (work) {
             this.name = work;
             switchFun(work)
           }
           //更改工作描述的方法
           _candidate.work.changeDes = function (des) {
             this.description = des
           }
           return _candidate
         }
         var candidateArr = []
         // 遍历生成应聘者实例
         for (var i = 0; i < data.length; i++) {
           candidateArr[i] = Candidate(data[i])
         }
         console.log(candidateArr[0]);
         // 更改工作名称
         candidateArr[0].work.changeWork('xxx');
         console.log(candidateArr[0]);
```

**使用建造者模式改造**

```
        // 应聘者信息
         var data = [
           {
             name: 'zhang san',
             age: 23,
             work: 'engineer'
           },
           {
             name: 'li si',
             age: 26,
             work: 'teacher'
           },
           {
             name: 'wang wu',
             age: 13,
             work: 'xxx'
           }
         ]
         //生成应聘者实例的类
         function Candidate(param) {
           var _candidate = new Person(param)
           _candidate.name = new CreateName(param.name)
           _candidate.work = new CreateWork(param.work)
           return _candidate
         }

         function Person(param) {
           this.age = param.age
         }

         function CreateName(name) {
           this.wholeName = name;
           this.firstName = name.split(' ')[0]
           this.secondName = name.split(' ')[1]
         }
         function CreateWork(work) {
           switch (work) {
             case 'engineer':
               this.name = '工程师';
               this.description = '热爱编程';
               break;
             case 'teacher':
               this.name = '教师';
               this.description = '乐于分享';
               break;
             default:
               this.name = work
               this.description = '无';
           }
           //更改工作的方法
           CreateWork.prototype.changeWork = function (work) {
             CreateWork.call(this, work)
           }
           //更改工作描述的方法
           CreateWork.prototype.changeDes = function (des) {
             this.description = des
           }
         }
         var candidateArr = []
         // 遍历生成应聘者实例
         for (var i = 0; i < data.length; i++) {
           candidateArr[i] = Candidate(data[i])
         }
         console.log(candidateArr[0]);
         // 更改工作名称
         candidateArr[0].work.changeWork('xxx');
         console.log(candidateArr[0]);
```

对比可以发现，普通代码把所有实现逻辑都放在一个类中，让这个类看起来非常臃肿阅读性差，而建造者模式会分步骤，一步步简化每个类里面的代码量，从而提高阅读性

### 单体模式

> 单例就是保证一个类只有一个实例，实现方法一般是先判断实例存在与否，如果存在直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。在JavaScript里，单例作为一个命名空间提供者，从全局命名空间里提供一个唯一的访问点来访问该对象。

问题：

```
    function NotSingle() {
      this.a = 123
    }
    var a1 = new NotSingle()
    var a2 = new NotSingle()
    console.log(a1 === a2);   //false
```

同一个类创造的实例不相等，如果就想他们想相等呢？

可以采用单例模式来进行改造

```
   var _unique = null;
    function createSingle() {
      var obj = {
        a: 1
      }
      if (_unique === null) {
        _unique = obj
      }
      return _unique
    }
    var a = createSingle()
    var b = createSingle()
    console.log(a === b);  //true
```

上面这种方法虽然可以实现单例模式，但是由于实例对象定义在全局中，不安全，下面使用闭包来进行改造

```
 //使用闭包进行改造
    var createSingle = (function () {
      var _unique = null
      function single() {
        return {
          a: 1
        }
      }
      return function () {
        if (_unique === null) {
          _unique = single()
        }
        return _unique
      }
    })()
    var a = createSingle()
    var b = createSingle()
    console.log(a === b);  //true
```

### 装饰者模式

> 装饰者模式，希望在不改变原对象的基础上，通过对其拓展功能和属性来实现更复杂的逻辑。

有一个案例：4s店在卖一种车，价格为10万元，如果用户需要在此基础上加装一些配置则需要加钱。比如加热座椅配置需要2万元，电动后视镜需要0.8万元等等

**普通写法：**

```
  function Car() {
      this.price = 10
    }
    Car.prototype = {
      addHeatSeat: function () {
        this.hasHeatSeat = true
        this.price += 2
      },
      addAutoMirror: function () {
        this.hasAutoMirror = true
        this.price += 0.8
      }
    }
    var car1 = new Car()
    console.log(car1.price);  //10
    car1.addHeatSeat()
    car1.addAutoMirror()
    console.log(car1.price);  //12.8
```

**装饰者模式：**

```
	function Car() {
      this.price = 10
    }

    function carWithHeatSeat(carExample) {
      carExample.hasHeatSeat = true
      carExample.price += 2
    }
    function carWithHeatMirror(carExample) {
      carExample.hasAutoMirror = true
      carExample.price += 0.8
    }

    var car2 = new Car();
    console.log(car2.price); //10
    carWithHeatSeat(car2)
    carWithHeatMirror(car2)
    console.log(car2.price); //12.8
```

两种模式对比，装饰者模式是独立与构造函数之外的函数，这样就能减少对构造函数的介入

### 组合模式

> 组合模式作用于将多个部分通过组合变成一个整体。

比如我们在工作中经常会制作一些表单，比如登录，注册，或者一些信息填写等等，这些表单其实都是类似的，如果你今天制作一个注册的表单，明天做个调查问卷的表单，是不是会觉得很妈蛋，有点重复劳动的感觉？

组合模式可以解决这个问题

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    //寄生式组合继承
    function inheritPrototype(subClass, superClass) {
      function F() { }
      F.prototype = superClass.prototype;
      subClass.prototype = new F()
      subClass.prototype.constructor = subClass
    }
    //组合继承
    // function inheritPrototype(subClass, superClass) {
    //   subClass.prototype = new superClass()
    // }
    //基类
    function Container() {
      aa
      this.children = []
      this.element = null
    }
    Container.prototype = {
      init: function () {
        throw new Error('请重写init方法')
      },
      add: function (child) {
        this.children.push(child)
        this.element.appendChild(child.element)
        return this
      }
    }
    //基于容器基类创建表单容器
    function CreateForm(id, method, action, parent) {
      Container.call(this)
      this.id = id || 'get';
      this.method = method || 'get'
      this.action = action || ''
      this.parent = parent
      this.init()
    }
    inheritPrototype(CreateForm, Container)
    CreateForm.prototype.init = function () {
      this.element = document.createElement('form')
      this.element.id = this.id
      this.element.method = this.method
      this.element.action = this.action
    }
    CreateForm.prototype.show = function () {
      this.parent.appendChild(this.element)
    }
    //行容器组件
    function CreateLine(className) {
      Container.call(this)
      this.className = className === undefined ? 'form-line' : 'form-line' + className
      this.init()
    }
    inheritPrototype(CreateLine, Container)
    CreateLine.prototype.init = function () {
      this.element = document.createElement('div')
      this.element.className = this.className
    }
    //label
    function CreateLabel(text, forName) {
      this.text = text || ''
      this.forName = forName || ''
      this.init()
    }
    CreateLabel.prototype.init = function () {
      this.element = document.createElement('label')
      this.element.setAttribute('for', this.forName)
      this.element.innerHTML = this.text
    }
    //input
    function CreateInput(type, id, name, defaultValue) {
      this.type = type || ''
      this.id = id || ''
      this.name = name || ''
      this.defaultValue = defaultValue || ''
      this.init()
    }
    CreateInput.prototype.init = function () {
      this.element = document.createElement('input')
      this.element.type = this.type
      this.element.id = this.id
      this.element.name = this.name
      this.element.value = this.defaultValue
    }
    
    var form = new CreateForm('owner-form', 'GET', 'https://www.baidu.com/s', document.body)
    console.log(new CreateLine());
    var userLine = new CreateLine()
      .add(new CreateLabel('用户名', 'user'))
      .add(new CreateInput('text', 'user', 'wd'))

    var pwdLine = new CreateLine()
      .add(new CreateLabel('密码', 'pwd'))
      .add(new CreateInput('password', 'pwd', 'pwd'))

    var submitLine = new CreateLine()
      .add(new CreateInput('submit', '', '', '登录'))
    form.add(userLine).add(pwdLine).add(submitLine).show()
  </script>
</body>
</html>
```

使用ES6 class来改造

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    //基类
    class Container {
      constructor() {
        this.children = [],
          this.element = null
      }
      init() {
        throw new Error('请重写init方法')
      }
      add(child) {
        this.children.push(child)
        this.element.appendChild(child.element)
        return this
      }
    }
    //基于容器基类创建表单容器
    class CreateForm extends Container {
      constructor(id, method, action, parent) {
        super()
        this.id = id || 'get';
        this.method = method || 'get'
        this.action = action || ''
        this.parent = parent
        this.init()
      }
      init() {
        this.element = document.createElement('form')
        this.element.id = this.id
        this.element.method = this.method
        this.element.action = this.action
      }
      show() {
        this.parent.appendChild(this.element)
      }
    }
    //行容器组件
    class CreateLine extends Container {
      constructor(className) {
        super()
        this.className = className === undefined ? 'form-line' : 'form-line' + className
        this.init()
      }
      init() {
        this.element = document.createElement('div')
        this.element.className = this.className
      }
    }
    //label类
    class CreateLabel {
      constructor(text, forName) {
        this.text = text || ''
        this.forName = forName || ''
        this.init()
      }
      init() {
        this.element = document.createElement('label')
        this.element.setAttribute('for', this.forName)
        this.element.innerHTML = this.text
      }
    }
    //input类
    class CreateInput {
      constructor(type, id, name, defaultValue) {
        this.type = type || ''
        this.id = id || ''
        this.name = name || ''
        this.defaultValue = defaultValue || ''
        this.init()
      }
      init() {
        this.element = document.createElement('input')
        this.element.type = this.type
        this.element.id = this.id
        this.element.name = this.name
        this.element.value = this.defaultValue
      }
    }

    var form = new CreateForm('owner-form', 'GET', 'https://www.baidu.com/s', document.body)
    console.log(new CreateLine());
    var userLine = new CreateLine()
      .add(new CreateLabel('用户名', 'user'))
      .add(new CreateInput('text', 'user', 'wd'))

    var pwdLine = new CreateLine()
      .add(new CreateLabel('密码', 'pwd'))
      .add(new CreateInput('password', 'pwd', 'pwd'))
    var submitLine = new CreateLine()
      .add(new CreateInput('submit', '', '', '登录'))
    form.add(userLine).add(pwdLine).add(submitLine).show()
  </script>
</body>
</html>
```

### 观察者模式

> 观察者模式又叫发布订阅模式或者消息模式。
>
> 是设计模式中非常著名也是非常重要的一种模式，这种模式一般会定义一个主体和众多的个体，这里主体可以想象为一个消息中心，里面有各种
>
> 各样的消息，众多的个体可以订阅不同的消息，当未来消息中心发布某条消息的时候，订阅过他的个体就会得到通知

核心：

​	取代对象之间硬编码的通知机制，一个对象不用再显式地调用另外一个对象的某个接口。

​	与传统的发布-订阅模式实现方式（将订阅者自身当成引用传入发布者）不同，在JS中通常使用注册回调函数的形式来订阅

实现：

```
    //订阅发布中心
    var msgCenter = (function () {
      var _msg = {};  //储存消息
      // var _msg = {
      //   'carInfo' : [fn1,fn2...],
      //   'newsInfo': [fn1,fn2...],
      //    ......
      // }
      return {
        //用于订阅一个消息
        subscribe: function (type, fn) {
          if (_msg[type]) {
            _msg[type].push(fn)
          } else {
            _msg[type] = [fn]
          }
        },
        //用于发布消息
        release: function (type, args) {
          if (!_msg[type]) {
            return
          }
          var event = {
            type: type,
            args: args || {}
          }
          for (let index = 0; index < _msg[type].length; index++) {
            _msg[type][index](event)
          }
        },
        //用于取消订阅消息
        cancel: function (type, fn) {
          if (!_msg[type]) {
            return
          }
          for (let index = 0; index < _msg[type].length; index++) {
            if (_msg[type][index] === fn) {
              _msg[type].splice(index, 1)
              break
            }
          }
        }
      }
    })()
    //订阅者类
    function Person() {
      this.alreadysubscribe = {}
      Person.prototype.subscribe = function (type, fn) {
        //防止重复订阅
        if (this.alreadysubscribe[type]) {
          console.log('你已经订阅过这个消息了，请不要重复订阅！');
        } else {
          msgCenter.subscribe(type, fn)
          //这句话是为了保存每个实例的订阅回调方法，通过对比，可以防止重复订阅
          this.alreadysubscribe[type] = fn
        }
      }
      Person.prototype.cancel = function (type) {
        msgCenter.cancel(type, this.alreadysubscribe[type])
        delete this.alreadysubscribe[type]
      }
    }
    var person1 = new Person()
    var person2 = new Person()
    var person3 = new Person()
    //订阅
    person1.subscribe('carInfo', function (e) {
      console.log('person1得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    person1.subscribe('newsInfo', function (e) {
      console.log('person1得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    person2.subscribe('carInfo', function (e) {
      console.log('person2得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    person3.subscribe('newsInfo', function (e) {
      console.log('person3得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    person3.subscribe('carInfo', function (e) {
      console.log('person3得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    //发布消息
    msgCenter.release('carInfo', { info: '新款汽车上市！' })
    msgCenter.release('newsInfo', { info: '某国家领导人访华' })
    //测试检测重复订阅功能
    person3.subscribe('carInfo', function (e) {
      console.log('person3得到了关于' + e.type + '的消息，消息内容是：' + e.args.info);
    })
    //测试取消订阅功能
    person3.cancel('carInfo')
    msgCenter.release('carInfo',{info:'再发一条消息'})
```

上面代码实现的核心是把每个实例的函数传递给msgCenter函数中的_msg数组保存好，并且通过type属性来进行区分，如果有消息发布，就全部执行一遍该数组的所有方法

### 策略模式

> strategy，
>
> 在现实生活中常常遇到实现某种目标存在多种策略可供选择的情况，例如，出行旅游可以乘坐飞机、乘坐火车、骑自行车或自己开私家车等，超市促销可以釆用打折、送商品、送积分等方法。
>
> 在软件开发中也常常遇到类似的情况，当实现某一个功能存在多种算法或者策略，我们可以根据环境或者条件的不同选择不同的算法或者策略来完成该功能，如数据排序策略有冒泡排序、选择排序、插入排序、二叉树排序等。
>
> 如果使用多重条件转移语句实现（即硬编码），不但使条件语句变得很复杂，而且增加、删除或更换算法要修改原代码，不易维护，违背开闭原则。如果采用策略模式就能很好解决该问题。

通俗解析:

> 假设一段文字需要进行检测是否为数字，是否为空，是否是邮箱等等，一般来说我们会使用if_else来进行判断，如果既要检测是否为数字又是否为电话号码，那么就需要两个if_else，且灵活性和复用性并不强，而策略模式是把一大堆需要用到的检测方法或者算法或者功能封装到一个对象中，并且为对象配置validate方法，用以调用对象中的方法，随便用，一个或者多个，复用，都没问题

例子：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <input type="text">
  <script>
    //策略模式
    var formStrategy = (function () {
      var strategy = {
        notEmpty: function (value) {
          return value.length ? '' : '请填写内容'
        },
        isNumber: function (value) {
          var reg = /^[0-9]+(\.[0-9])?$/;
          return reg.test(value) ? '' : '请填写一个数字'
        },
        isPhone: function (value) {
          //010-12345678 0022-1234567
          var reg = /^\d{3}-\d{8}$|^\d{4}-\d{7}$/
          return reg.test(value) ? '' : '请输入一个正确的电话好吗'
        }
      }
      return {
        // 检测方法，type是输入要检测的类型，value是值
        validate: function (type, value) {
          //去除输入文字两边的空白符
          value = value.replace(/^\s*|\s*$/g, "")
          return strategy[type] ? strategy[type](value) : '没有这个检测方法，请手动添加'
        },
        //临时添加自定义检测算法
        addStrategy: function (type, fn) {
          if (strategy[type]) {
            return '这个方法已经存在'
          } else {
            strategy[type] = fn
          }
        }
      }
    })()
    //测试
    var oInput = document.querySelector('input')
    oInput.onchange = function () {
      var result;
      result = formStrategy.validate('notEmpty', this.value) || formStrategy.validate('isNumber', this.value) || '通过检测'
      console.log(result);
    }
  </script>
</body>

</html>
```

### 链模式

> 链模式是实现链式调用的主要方法，通过在自身方法中返回自身的方式，在一个对象连续多次调用自身方法可以简化写法
>
> 这种链式调用在开很多库和框架如jquery/zepto中频繁的被使用。

简单的链模式

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    var obj = {
      a: function () {
        console.log('aaa');
        return this
      },
      b: function () {
        console.log('bbb');
        return this
      }
    }

    obj.a().b().a().a()    ///aaa bbb aaa aaa
  </script>
</body>
</html>
```

### 委托模式

> 当多个对象需要处理同一请求时，可以将这些请求交由另一个对象统一处理

普通模式

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <ul class="ul1">
    <li>aaaa</li>
    <li>bbbb</li>
    <li>cccc</li>
    <li>dddd</li>
  </ul>
  <script>
     var aLis = document.getElementsByTagName('li')
     for (let index = 0; index < aLis.length; index++) {
       (function (i) {
        aLis[index].onclick = function () {
           console.log(index);
         }
       })(index)
     }
  </script>
</body>
</html>
```

这样会绑定多个事件，会影响性能

使用委托模式进行改进：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <ul class="ul1">
    <li>aaaa</li>
    <li>bbbb</li>
    <li>cccc</li>
    <li>dddd</li>
  </ul>
  <script>
    var oUl = document.querySelector('ul')
    oUl.onclick = function (e) {
      console.log(e);
      var e = e || window.event
      target = e.target || e.srcElement
      if(target.nodeName.toLowerCase() === 'li'){
        console.log(target.innerHTML);
      }
    }
    var oLi = document.createElement('li')
    oLi.innerHTMl = 'eeee'
    oUl.appendChild(oLi)
  </script>
</body>
</html>
```

### 数据访问对象模式

> 数据访问对象模式主要是用来抽象和封装一个对象来对数据源进行访问和存储，这样可以方便对数据的管理，以及避免数据间的重复，覆盖等问题

实例：

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <script>
    // {
    //   key: expire|value
    // }
    function DataVisitor(splitSign) {
      this.splitSign = splitSign || '|'
    }
    DataVisitor.prototype = {
      status: {
        SUCCESS: 1,
        FAILURE: 0,
        OVERFLOWER: 2,
        TIMEOUT: 3
      },
      set: function (key, value, cbFn, expireTime) {
        var status = this.status.SUCCESS;
        expireTime = typeof expireTime === 'number' ? expireTime + new Date().getTime() : -1
        try {
          window.localStorage.setItem(key, expireTime + this.splitSign + value)
        } catch (e) {
          status = this.status.OVERFLOWER
        }
        cbFn && cbFn.call(this, status, key, value);
        return value
      },
      get: function (key, cbFn) {
        var status = this.status.SUCCESS
        var value = window.localStorage.getItem(key)
        if (value) {
          var index = value.indexOf(this.splitSign),
            time = value.slice(0, index)
          if (time > new Date().getTime() || time == -1) {
            value = value.slice(index + this.splitSign.length)
          } else {
            value = null;
            status = this.status.TIMEOUT;
            window.localStorage.removeItem(key)
          }
        } else {
          status = this.status.FAILURE
        }
        cbFn && cbFn.call(this, status, key, value)
      },
      remove: function (key, cbFn) {
        var status = this.status.FAILURE;
        value = window.localStorage.getItem(key);
        if (value) {
          value.slice(value.indexOf(this.splitSign) + this.splitSign.length)
          window.localStorage.removeItem(key);
          status = this.status.SUCCESS;
        }
        cbFn && cbFn.call(this, status)
      }
    }
    
    var test = new DataVisitor();
    test.set('aaa', '1273', function (status) {
      console.log(Boolean(status));
    }, 2000)
    test.get('aaa', function (status, key, value) {
      console.log(status, key, value);
    })
    setTimeout(() => {
      test.get('aaa', function (status, key, value) {
        console.log(status, key, value);
      }, 2000)
    }, 1000);
    setTimeout(() => {
      test.get('aaa', function (status, key, value) {
        console.log(status, key, value);
      }, 2000)
    }, 3000);
    // test.remove('aaa',function (status) {
    //    console.log(Boolean(status));
    // })
    // test.remove('aaab', function (status) {
    //     console.log(Boolean(status));
    //   })
  </script>
</body>

</html>
```

### 等待者模式

> 通过对多个异步进程的监听，对未来事件进行统一管理。

```
 //等待对象
    var Writer = function () {
      var dfd = [],//等待对象容器,When中传入的异步执行方法，实为事件对象数组
        doneArr = [],//成功回调方法容器，用于存放done中传入的成功回调方法
        failArr = [],//失败回调方法容器，用于存放fail中传入的失败回调方法
        slice = Array.prototype.slice,
        that = this;

      //监控对象类
      var Primise = function () {
        //监控成功状态
        this.resolved = false;
        //监控失败状态
        this.rejected = false;
      }

      //扩展对异步逻辑的监控方法，这两个方法都是因异步逻辑状态的改变而执行相应操作的
      Primise.prototype = {
        //解决成功
        resolve: function () {
          //设置当前监控对象解决成功，每一个事件都有自己独立的监控对象，
          //都有自己的独立成功状态与失败状态
          this.resolved = true;
          //如果没有监控对象则取消执行
          if (!dfd.length) return;
          //遍历所有注册了的监控对象
          for (var i = dfd.length - 1; i >= 0; i--) {
            //如果有任意一个监控对象没有被解决或者解决失败则返回
            if (dfd[i] && !dfd[i].resolved || dfd[i].rejected) {
              return;
            }
            //如果已经解决则清除已解决监控对象
            dfd.splice(i, 1);
          }
          //执行解决成功回调方法
          _exec(doneArr);
        },
        //解决失败
        reject: function () {
          //设置当前监控对象解决失败
          this.rejected = true;
          //如果没有监控对象则取消执行
          if (!dfd.length) return;
          //清除所有监控对象
          dfd.splice(0);
          //执行解决失败回调方法
          _exec(failArr);
        }
      }
      //创建监控对象
      that.Deferred = function () {
        return new Primise();
      }
      //监控异步方法 参数：监控对象，用于监测已经注册过的监控对象的异步逻辑
      that.When = function () {
        //设置监控对象
        dfd = slice.call(arguments);
        var i = dfd.length;
        for (--i; i >= 0; i--) {
          //如果不存在监控对象，或者监控对象已经解决，或者不是监控对象
          if (!dfd[i] || dfd[i].resolved || dfd[i].rejected || !dfd[i] instanceof Primise) {
            //清除当前监控对象
            dfd.splice(i, 1);
          }
        }
        return that;
      }
      //解决成功回调函数添加方法，用于向对应的回调容器中添加相应回调
      that.done = function () {
        doneArr = doneArr.concat(slice.call(arguments));
        return that;
      }
      //解决失败回调函数添加方法，用于向对应的回调容器中添加相应回调
      that.fail = function () {
        failArr = failArr.concat(slice.call(arguments));
        return that;
      }
      //回调执行方法
      function _exec(arr) {
        //遍历回调数组执行回调,注意，此处为了按先后顺序执行，不能用逆向循环
        for (var i = 0, len = arr.length; i < len; i++) {
          try {
            arr[i] && arr[i]();
          } catch (e) { }
        }
      }
    }
    //运用场景模拟：假设页面中有多个随机运动的彩蛋，每个彩蛋结束后都要展示一个欢迎页面
    var waiter = new Writer();

    //第1个彩蛋，5秒后停止
    var first = function () {
      //创建监听对象
      var dtd = waiter.Deferred();
      setTimeout(function () {
        console.log('first');
        //发布解决成功消息，执行解决成功回调，
        //当在执行成功回调时，同时会检查其他事件的最后状态，
        //如果其他事件都已经成功执行，则执行成功回调
        //如果有其他事件还未执行完毕，则只负责把自己的状态设置为成功，
        dtd.resolve();
      }, 500);
      //返回监听对象
      return dtd;
    }();

    //第2个彩蛋，10秒后停止
    var second = function () {
      //创建监听对象
      var dtd = waiter.Deferred();
      setTimeout(function () {
        console.log('second');
        //发布解决成功消息
        dtd.resolve();//
      }, 1000);
      //返回监听对象
      return dtd;
    }();

    //最后，我们用等待者对象监听两个彩蛋的工作状态，并执行相应的成功回调与失败回调
    waiter
      .When(first, second)//把异步方法加入when当中监听
      .done(function () {
        //把成功回调方法加入donearr中保存，在监听的事件中，
        //只要有一个事件的最终状态为失败，则整个结果为失败，成功队列中的方法不再执行
        //当且仅当所有的最终结果为成功，才算成功，才会执行done中方法
        console.log('success');
      }, function () {
        console.log('success again');
      })
      .fail(function () {
        //把失败回调方法加入failarr中保存，只要有一个事件的最终结果为失败，则执行失败回调方法
        console.log('fail');
      }, function () {//把失败回调方法加入failarr中保存
        console.log('fail again');
      })

     //first
     //second
     //success
     //success again
```

### MVC模式

> MVC即Model-View-Controller（模型-视图-控制器）是一种软件设计模式，最早出现在Smalltalk语言中，后被Sun公司推荐为Java EE平台的设计模式。

　MVC把应用程序分成了上面3个核心模块，这3个模块又可被称为业务层-视图层-控制层。顾名思义，它们三者在应用程序中的主要作用如下：

**业务层**：负责实现应用程序的业务逻辑，封装有各种对数据的处理方法。它不关心它会如何被视图层显示或被控制器调用，它只接受数据并处理，然后返回一个结果。

**视图层**：负责应用程序对用户的显示，它从用户那里获取输入数据并通过控制层传给业务层处理，然后再通过控制层获取业务层返回的结果并显示给用户。

**控制层**：负责控制应用程序的流程，它接收从视图层传过来的数据，然后选择业务层中的某个业务来处理，接收业务层返回的结果并选择视图层中的某个视图来显示结果。

![img](https://t11.baidu.com/it/u=3066061642,1532806395&fm=173&app=25&f=JPEG?w=504&h=300&s=A983CC1223DA6DC80A761159020050FA)

在MVC里，View是可以直接访问Model的，所以View里会包含Model信息以及一些业务逻辑。 MVC模型关注的是Model的不变，所以在MVC模型里，Model不依赖于View，但是 View是依赖于Model的。不仅如此，因为有一些业务逻辑在View里实现了，导致要更改View也是比较困难的，至少那些业务逻辑是无法重用的。

代码：

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <script>
    //一个简单的mvc实例
    var MVC = {}

    MVC.model = (function () {
      var data = {
        sidebar: [{
          title: 'sidebar1',
          href: './a.html'
        }, {
          title: 'sidebar2',
          href: './b.html'
        }, {
          title: 'sidebar3',
          href: 'http://www.baidu.com'
        }]
      }

      return {
        getData: function (key) {
          return data[key]
        },
        setData: function (key, value) {
          data[key] = value
          MVC.view('createSidebar')
        }
      }
    })()

    MVC.view = (function () {
      var m = MVC.model
      var view = {
        createSidebar: function () {
          var data = m.getData('sidebar')
          var html = ''
          html += '<div id ="#sidebar">'
          for (let i = 0; i < data.length; i++) {
            html += '<div class="sidebar-item"><a href="' + data[i].href + '">' + data[i].title + '</a></div>'
          }
          html += '</div>'

          document.body.innerHTML += html
        }
      }
      return function (v) {
        view[v]()
      }
    })()

    MVC.ctrl = (function () {
      var m = MVC.model
      var v = MVC.view
      var c = {
        initSideBar: function () {
          v('createSidebar')
        },
        updateSiderBar: function () {
          m.setData('sidebar', [{ title: 'new sidebar', href: 'http://www.baidu.com' }])
        }
      }
      return c
    })()

    window.onload = function () {
      //从control修改model
      MVC.ctrl.initSideBar()
      setTimeout(() => {
        MVC.ctrl.updateSiderBar()
      }, 3000);
      setTimeout(() => {
        MVC.view('createSidebar')
      }, 6000);
      setTimeout(() => {
        MVC.view('createSidebar')
      }, 9000);
    }
  </script>
</body>

</html>
```

### MVVM模式

> MVVM模式在传统MVC模式下进行改造，实现其重在数据驱动视图的一种设计模式。

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <script>
    //如何实现数据与视图绑定
    // 1、需要知道哪个数据改变了。一般我们可以使用数据访问对象的方法。在vue中我们使用的是es5的对象访问属性get/set
    // 2、修改视图
    var model = {
      a: 1,
      b: 2
    }
    // vm
    for (var key in model) {
      ; (function (key) {
        var value = model[key]
        Object.defineProperty(model, key, {
          get: function () {
            return value
          },
          set: function (newVal) {
            value = newVal
            render()
          }
        })
      })(key)
    }
    //view
    let render = (function render() {
      console.log('render执行了');
      document.body.innerHTML = '<div><h3>想显示一些文案</h3><p>a的值: ' + model.a + ',b的值：' + model.b + '</p></div>'
      return render
    })()

    setTimeout(() => {
      model.b = 3
    }, 2000);
    setTimeout(() => {
      model.b = 11111
    }, 3000);

  </script>
</body>

</html>
```

## 计算机网络

# 前端开发

## 前端基础

## Flutter





## Vue



# 后端开发