本项目实现一个基于写时复制字典树的键值对存储系统

字典树（Trie Tree）
定义与基本概念
字典树，又称前缀树或 Trie 树，是一种树形数据结构，用于高效地存储和检索字符串集合中的键。它的核心思想是利用字符串之间的公共前缀来减少时间和空间的开销，使得在查找时可以通过公共前缀快速定位。
结构特点
节点结构：每个节点包含若干子节点，通常通过字符索引（如 26 个英文字母）来链接。
根节点：不包含任何字符，其他节点每个节点只包含一个字符。
路径表示：从根节点到某一节点的路径上的所有字符连接起来，即为该节点对应的字符串。
标记机制：节点可能包含一个标记，用于表示该路径是否构成一个完整的字符串。
核心操作
插入（Insert）：将字符串逐个字符插入树中，从根节点开始，沿字符对应的子节点向下，若不存在则创建新节点，最后标记字符串结束位置。
查找（Search）：从根节点出发，按字符依次遍历，若某一步骤无法找到对应子节点则查找失败，若遍历完所有字符且节点有结束标记则查找成功。
前缀匹配（Prefix Match）：查找是否存在以给定字符串为前缀的字符串，只需遍历到前缀长度的节点是否存在即可。

写时复制（Copy-on-Write, COW）
定义与基本原理
写时复制是一种优化技术，用于在复制对象时不立即复制其内容，而是在对象被修改时才进行真正的复制。其核心思想是多个对象可以共享同一资源，只有当某个对象需要修改该资源时，才会创建专属的副本，避免无意义的复制开销。
实现机制
共享阶段：当复制对象时，仅复制对象的引用或指针，使原对象和副本共享底层数据资源（如内存块、文件句柄等）。
写操作触发复制：当其中一个对象尝试修改共享资源时，系统检测到写操作，立即为该对象创建独立的副本，后续修改仅作用于副本，原对象的资源保持不变。
读操作无开销：多个对象同时读取共享资源时，无需复制，直接访问原始数据。


简单说，COW-Tire-Tree与其他树区别就在于进行写操作时，直接复制一份（即生成一份新的）需要修改节点及其所有祖先至根节点，并加上所做修改（如修改节点值、添加子结点等），适合读多写少场景，可以保留旧版本进行回滚，进行写操作时其他共享者也可以同步进行对旧版本的读操作，无需加锁
