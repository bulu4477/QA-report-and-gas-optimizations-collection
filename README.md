# QA-report-and-gas-optimizations-collection
## QA report
1、
***
## gas optimizations
1、仅在构造函数中设置的状态变量应声明为immutable  
2、状态变量可以被缓存，而不是从存储中重新读取它们，在函数中设置一个状态变量的复制。  
3、映射/数组的多次访问应使用存储指针，不要重复调用proposals[_proposalId]，而是通过存储指针保存其引用：Proposal storage proposal_ = proposals[_proposalId]并使用该指针。https://github.com/code-423n4/2023-03-aragon-findings/blob/main/data/JCN-G.md#multiple-accesses-of-a-mappingarray-should-use-a-storage-pointer  
4、避免多次调用同一个内部函数，如果多次调用同一个内部函数，可以通过缓存内部函数的返回值来节省gas。  
5、删除initializer修饰符，如果我们可以确保该initialize()函数只能从构造函数内部调用，那么我们就不必担心它会被再次调用。  
6、使用硬编码地址代替address(this)，与 相比address(this)，通过硬编码预先计算和使用地址会更节省 Gas 效率。Foundryscript.sol和 solmateLibRlp.sol合同可以做到这一点。  
7、删除未使用的struct。  
8、使用storage代替memoryforstructs/arrays节省gas  
9、删除空的函数。  
10、如果不支持 EIP-2771，请勿使用 _msgSender()，使用msg.sender  
11、如果数据可以容纳 32 个字节，则可以使用 bytes32 数据类型来代替string  
12、将public函数可见性更改为external  
13、使用小于 32 字节（256 位）的 uint/int 会产生开销  
14、internal只调用一次的函数可以内联以节省gas  
15、将构造函数设置为payable，https://github.com/code-423n4/2023-03-aragon-findings/blob/main/data/0xSmartContract-G.md  
16、对于不会越界的计算使用uncheck  
17、使用嵌套 if 和，而不是&&  
18、使用更新版本的 Solidity  
19、<=比<+1花费更多的gas  
20、仅调用一次的内部函数可以内联以节省gas  
21、函数返回时不使用命名返回变量，浪费部署气体  
22、abi.encode() 的效率低于 abi.encodePacked()  
23、使用常量代替type(uintx).max  
24、用assembly写入地址存储值  
25、优化名称以节省gas，https://github.com/code-423n4/2023-03-aragon-findings/blob/main/data/0x6980-G.md  
26、使用private而不是public常量，可以节省gas。  
27、<array>.length不应在for循环的每个循环中查找  
28、反转 if-else 语句的条件会浪费气体。  
29、检查输入参数的 require() 或 revert() 语句应位于函数的顶部  
30、
***
