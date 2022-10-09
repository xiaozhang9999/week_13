# week_13
## 并查集Union Find
关于于连接问题：网络中节点间的连接状态；数学中集合类的实现（并集）。
对于一组数据，主要支持

 - union(p,q)  //合并p,q所属集合
 - isConnected(p,q)  //p,q是否属于同一集合
 
 Java可以用interface接口进行不同方式的实现。
 ### QuickFind
 基本数据表示：
 
|编号id|0|1|2|3|4|5|6|7|8|9
|--|--|--|--|--|--|--|--|--|--|--|
|所属集合|0|1|0|1|0|1|0|1|0|1|0|1

 可以实现快速查找元素所属集合。isConnected:O(1);union:O(n)。
 ### QuickUnion
 将每个元素看作一个节点，他们都有父亲节点，最后所属同一个节点，连成一个以该节点为根节点的多叉树，同一个树是一个集合。
 初始时定义parent[i]=i,各自为一个集合；
 若union(p,q)，令parent[p所在树的根节点对应的值]=q所在树的根节点对应的值。
 isConnected与union时间复杂度都与元素所在树高度h相关。
 ### 对QuickUnion基于size优化
 union p,q所在的两个树时使树节点个数少的指向树节点个数多的根节点，可以使集合树的高度小一些。需要加一个sz[i]记录根节点下节点个数，每次union时更新对应sz[i]的值。
  ### 对QuickUnion基于rank(树高)优化
  union时树高度小的根节点指向高度大的根节点，加rank[i]记录根节点深度h。
  

 - rank[p]<rank[q]  parent[p]=q  rank不必更新
 - rank[q]<rank[p]  parent[q]=p  rank不必更新
 - rank[p]=rank[q]  parent[p]=q/parent[q]=p   对应rank更新rank[q]+=1/rank[p]+=1
 
 ### 路径压缩 Path Compression
 将一个较高的树压缩成较低的树。
 在find方法的while循环中添加parent[p]=parent[parent[p]]令节点连接该节点的父节点。
 ## 平衡树和AVL
 平衡二叉树：对任意节点，左子树与右子树高度差不大于1。
 平衡因子：左子树高度-右子树高度
 ### AVL左旋转和右旋转维护平衡
 加入新节点后平衡性被打破，沿着节点向上维护平衡性，对|平衡因子|>1的节点进行维护。不平衡分几种情况：
- LL型 ：节点平衡因子>1&&该节点左孩子节点平衡因子>0;进行右旋转
 -RR型 ：节点平衡因子<-1&&该节点右孩子节点平衡因子<0;进行左旋转
