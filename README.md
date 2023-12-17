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
8、使用storage代替memory for structs/arrays节省gas  
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
30、打包结构变量，对结构进行重新排序MarketState可确保有效地打包结构变量，从而最大限度地降低存储成本。  
31、不要缓存仅使用一次的调用。  
32、调用转账前应检查金额是否为 0  
33、使用布尔值进行存储会产生开销  
34、不要用默认值初始化变量  
35、使用 uint256(1)/uint256(2) 代替 true 和 false 布尔状态  
36、只使用一次并且不被继承的修饰符应该内联以节省gas  
37、缓存全局变量比使用实际变量更昂贵（使用 msg.sender 而不是缓存它）  
38、可以将变量放在循环之外以节省gas  
39、使用 != 0 代替 > 0 表示无符号整数比较  
40、使用 do while 循环代替 for 循环  
41、重复的 require()/revert() 检查应重构为修饰符或函数  
42、构造函数中未能检查零地址导致合约再次部署  
43、++i 比 i++ 花费更少的 Gas，特别是当它用于 for 循环时 (--i/ i-- 也是）  
44、除以二应使用位移位  
45、使用自定义错误而不是revert()/require()字符串来节省gas  
46、使用 selfbalance() 代替 address(this).balance  
47、不要收缩变量，这意味着，如果您使用 uint8 或任何小于 256 位的变量，EVM 必须首先将其转换为 uint256 才能对其进行处理，并且转换会花费额外的 Gas！https://github.com/code-423n4/2023-09-maia-findings/blob/main/data/zabihullahazadzoi-G.md  
48、映射比数组更有效  
49、优化名称以节省 Gas，可以优化公共/外部函数名称和公共成员变量名称以节省gas。网址如上  
50、对于变量来说，运算符 += 比 = + 花费更多的 Gas  
51、外部调用接收者造成的无限燃气消耗风险，当调用外部函数而不指定gas limit时，被调用的合约可能会消耗所有剩余的gas，导致交易被恢复。为了缓解这种情况，建议在进行低级别外部调用时明确设置气体限制。  
52、避免不必要的存储更新，避免在值未更改时更新存储。如果旧值等于新值，则不重新存储该值将避免 SSTORE 操作（花费 2900 Gas），可能会损失 SLOAD 操作（2100 Gas）或 WARMACCESS 操作（100 Gas）。  
53、位移位中应使用乘法和除法 2  
54、传递函数参数时，使用calldata区域代替memory可以提高gas效率。  
55、++numMinted 与 相比，花费更少的 GasnumMinted += 1  
56、在循环中使用calldata带她memory，前提是calldata的数值不会改变  
57、可以标记普通用户调用时保证恢复的功能payable，https://code4rena.com/reports/2023-07-axelar#gas-optimizations  
58、
***
