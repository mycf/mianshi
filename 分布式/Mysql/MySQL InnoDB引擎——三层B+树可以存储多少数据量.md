> 3层B+树大概可以存：
> 
> *   **主键为bigint**：约**2000w**
> *   **主键为int**：约**4000w**

\*备注：

> 在《阿里开发手册》中建议，单表行数**超过500万行**或者单表容量**超过2GB**，才推荐进行分库分表，如果预计三年后数据量根本达不到这个级别，就不必要在创建表时就分库分表。 

计算
--

在[Innodb存储引擎](https://so.csdn.net/so/search?q=Innodb%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E&spm=1001.2101.3001.7020)里面，**最小存储单元是页，而一个页的大小默认是16KB。** 一个节点（叶子节点或非叶子节点）的大小就是一页。

> 在MySQL Innodb存储引擎中的B+树的**一个节点大小为“1页”，也就是16k。**也即代表B+树的每个节点可以存16KB数据。

为什么设计成这个大小？解释一下：

> 之所以设置为一页，是因为对于大部分业务，一页就足够了。  
> _（一条数据必须是放在一个节点中，**不能拆开**到多个节点存储。而一般一条数据大概1KByte，那么一页能容纳的大概16条，即一个节点容纳不止一条数据，而且是多达16条！！**所以大小采用最小存储单元足够用了~**）_

非叶子节点：  
ps：**指针是用来指向下一个“节点”的。**

> *   假设**主键类型为bigint，占用8Byte，指针可以设置为占用6Byte**，总共**14Byte**。这样就可以算出**一个非叶子节点**大概可以存放**16KByte/14Byte=1170个****“主键+指针”的组合**。
> *   假设**主键类型为int，占用4Byte，指针可以设置为占用6Byte**，总共**10Byte**。这样就可以算出**一个非叶子节点**大概可以存放**16KByte/10Byte≈1600个****“主键+指针”的组合**。

叶子节点：

> 在B+树中，真正的数据是**只存储在**叶子节点中的。  
> 这里我们假设我们的一行数据大小是1K，那么我们**一个叶子节点就可以存16KByte/10Byte=16条（行）数据**。

由此，可以推算出公式：

> *   两层总数 = **非叶子节点(根)** \* **叶子节点。**
> *   三层总数 = **非叶子节点(根) \* 非叶子节点 \* 叶子节点。**

**主键为bigint**（约2000w）**：**

*   **2层B+树的话：可以存放1170个\*16条=18720条（行）数据。**
*   **3层B+树的话：可以存放1170个\*1170个\*16条=21902400条（行）数据。**

**主键为int**（约4000w）**：**

*   **2层B+树的话：可以存放1600个\*16条=25600条（行）数据。**
*   **3层B+树的话：可以存放1600个\*1600个\*16条=40960000条（行）数据。**

> **所以三层B+树也就差不多2000w条或4000w条数据。**
