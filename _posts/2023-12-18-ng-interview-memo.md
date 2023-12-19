---
title: 2023年秋找工面试记录
tags: cmu career
---

这里dump了CMU最后一个学期的面试记录. 

<!--more-->

# Shoreline, BackEnd
第一轮线上, 考了到达某个终点的最短路径, 最短路径是path上的权重和.

当时没有确认清楚, 要求的是在DAG中任何一个没有outgoing edge的都算终点.

我就直接dfs了, 最后考官不认为时间复杂度是O(N), 我非常不解, 还是坚持这是O(N). 现在想明白了, 不是每个node都visit一次, 实现的时候有问题, 而应该记录该node到终点的最短距离. 这是因为不同路径访问到这个点是可能产生不一样的解的.

提问了大量513 Dynamic Memory Allocation和PP的优化细节, 证明这个team确实是做rust backend optimization的.

第2、3轮背靠背线下, 第一轮考了file copy service的一个业务代码, 用python写的, 用一个队列就好了?
然后要处理copy失败re-copy. 当然有好多方法, 这里边改边确认, 最后也很模糊, 只能大概觉得考官是想要push回原来的队列, 然后在正常batch操作中进行甄别.

提问了一个比较challenging的project. 我就说了高通的实习项目.

第二轮考的是一个vector of vector, 每个sub vector挑一个数字, output所有不同的组合. 我tmd直接brute force了, 一层一层往下, 维持一个当前组合的vector of vector, 考官没说什么.

提问了最近比较challenging的project, 前一轮说过高通的, 我就说了PP. 问的比较细节, 很多我忘记了, 比如当时为什么要对circles进行exclusive scan. 下次还得好好复习一下PP.

总的来说, 准备的不是很充分. 没犯大错, 也没啥亮点. 大事不妙.

结局: 被拒, position filled. 可能不是面试的问题, 推荐去了另外一个组.

# Shoreline, DevOps
第一次是一个简单的聊天, 我表达了我对在start up工作的热情. 哦我的上帝, 完全是拷贝了上次和Tacit AI面试的时候那个founder说的话. 确定了这个组是做云计算的DevOps, 兴趣乏乏, 但是还是表达了我的表面热情, 约上了下一次面试. 就当练练手了.

第一轮技术面
```
A team is playing a game. At the beginning of the game, the team has 0 points.
The team can gain points in non-negative integer increments of a_1, a_2, ..., or a_k points.
Every time the team gains points, the mascot has to do a number of pushups
equal to the new score.
You are given the possible point increments a_1, a_2, a_3, ..., a_k,
and the total number of pushups p the mascot ended up doing.
Find a possible final score s for the team, or correctly assert that
such a number of pushups is impossible the given game.
---
Example:
a = [3, 4, 5]
p = 23
A possible value for s is 12.
```
```C++
void backtrack(vector<int>& a, int p, int& answer, int current_score, int current_p, unordered_set<string> memo) {
  if(current_p > p) return;
  if(answer != -1) return;
  if(current_p == p) {
    answer = current_score;
    return;
  }
  string key = to_string(current_score) + " " + to_string(current_p);
  if(memo.find(key) != memo.end()) return;
  for(int i : a) {
    backtrack(a, p, answer, current_score + i, current_p + current_score + i, memo);
    if(answer != -1) return;
  }
  memo.insert(key);
}

int solution(vector<int>& a, int p) {
  int answer = -1;
  unordered_set<string> memo;
  backtrack(a, p, answer, 0, 0, memo);
  return answer;
}

p = 28431, a = [13, 15] -> s in [855, 857, 859, 861, 863, 865, 867, 869, 871, 873, 874, 875, 876, 877, 878, 879, 880, 881, 882, 883, 884, 885, 886, 887, 888, 889, 890, 892, 894, 896, 898, 900, 902, 904, 906, 908, 910, 912, 914]
```
这是极度类似CombinationIV的, 带memo的combination. 但是有一点奇怪, memo加进去之后更慢了. 面试官也不知道为什么. **TODO** 经过研究, C++是可以这样用state组成string做key的. 仍然不知道没效果的原因.

后续: 拒了. 心态已然逐渐成熟, 即便感觉聊的还可以, 对被拒也并不奇怪了. 毕竟公司那边是不受我影响的.

# Health At Scale, MLE
先是大量ML基础的提问. 由于这个公司是做的医疗, 估计regression用的比较多, 而并不是医疗图像, 没有任何深度学习的问题. 

先问了logistic regression的数学表达式, 我忘了, 我直接说的linear regression的仿射变换. 然后他提示了一下logistic regression output 0 - 1之间, 我想这不会是加了个softmax吧, 就把softmax公式说了. 好像大致猜中了, 实际上是叫sigmoid. 

然后问损失函数, 我说了MSE, 一开始说logistic regression也可以用. 但是考官明显要的不是这个, 我就试探的说是不是交叉熵, 好像确实是, 毕竟加了softmax的. 又问交叉熵的数学表达式, 我猜了一下, 直接的有log, 就猜了sum(log(Y - Y')), 估计不对. 

接下来问正则化, 公式、用途答的不错, 但是问有什么区别, 我忘记了, 只记得L2用的比较多.

总结是我确实完全忘了logistic regression, 考的都是ML的基础而非DL, 我本来本科的时候就没有学好orz.

题目做到原题了, 第一题LC560, 用前缀数组哈希表版;第二题word break. 

做第一题的时候被发现做过了, 笑死. 我在那里把思考经历全说了, 什么brute force, 到滑动窗口为什么不行, 到普通前缀数组, 到用哈希表记录前缀数组, 直接暴露了. 人不得瑟会死吗?

总结, coding没啥亮点, ML问题可能有些扣分, 虽然考官一直“哦, 这很好, 完全正确”.

结局: 被拒, 后来复习的时候确实发现自己的回答太不正确了. 要好好复习ML.

# Tacit plus, MLE
再也不面start up了, 再也不面了. 超时了一个半小时, 太折磨了.

CS的问题是如何加速一个代码, 太他妈的general了, 我说了memory, 他好像不是很感兴趣, 只好说算法, 或者针对处理器的优化, 比如ARM可以反向loop什么的. 

ML的问题我答的稀烂, 他问的有点细节, bias和variance的细节我都忘了. 什么是high bias/high variance模型, 我费了好大的劲才在提示下回答出来. 然后问了一些LLM的问题, 我什么都不懂.

然后最离谱的就是之后的提问, 如何保证两个function的output完全相同, 什么test case的构建, 直接开始讨论业务. 答到一半我有点受不了了, 只想结束.

Coding更是烦, 写一个data type的comparator, 我觉得好无聊. 基本上一边交流一边写, 已经把不耐烦打在脸上了. 分类讨论type, 然后recessively检查data structure里的每个element. 他说一个要求我挤一个实现. 我觉得这完全就是业务代码, 本来就是要根据要求改的嘛.

总结就是我一点也不感兴趣了, 2-3人的start up如果老板很抽象太烦了.

后续: 真香啊, 又面了一轮technical. 题目是两道业务代码, 第一道已经忘记了, 好像是分割出一个字符串里的内容; 第二道是计算TF-IDF, follow up问了如果documents size很大怎么办.

算是没啥问题, 面试官也就是founder说pass all technical了. 下一周的VentureBridge SV event我还去见到了他. 

后续: 和另外一个full-time又聊了聊, 基本确定想要去那里工作是没问题的了.

# Nvidia, Deep Learning Algorithm
收到面试邀请的时候真的好激动, Nv面试, 从来没有收到过, 又是在当前这个环境下. 如果能去Nv, 真的是最好的结局了. 花了很多时间复习ML, 也花了一点时间刷题.

面试官直接是一个lead, 40分钟, 主要可能还是看一下水平大概如何. 该部门是做AI Playground和Community Models(也就是开源社区的模型部署什么的). 

大量的时间在问简历的细节. 当前的TTS project用的VITS模型, SPD的time series模型和数据, 甚至是NTU的character creator 4 dataset. 高通的反而没问.

然后自然是PP, 他们估计见多了CUDA, 每个申请的人多多少少都要说自己的CUDA项目. 主要问了Transformer, 我都如实回答了. Megatron居然是他们部门维护的, 这波啊, 这波是关公面前耍大刀了.

然后是Coding, 用PyTorch写一个training loop. 写的其他的很顺利, 漏掉了一个optimizer.zero_grad(). 他问了这一行的左右, 我确实不知道, 猜测了一下grad清零是因为前向传播并不需要grad, 清零可以加速. 但是事实上是这一步清零, 后面的step才能正确计算, 不然就累积了. 多么简单的答案, 因为我从来没有关注过这个函数, 或者说关注过但是忘了. 总的而言, 实力不够. 决定一定要自学DLS了. 其实暑假就应该准备好, 但是像李晨昊学长这样的努力, 我终究是没能做到.

后续: 杯具. 太可惜了, 得到机会但是实力不够, 继续努力学习ML/DL吧.

# Black Sesame, AI Compiler
AI compiler的概念就类似我们之前高通组里的DirectNN? 我的理解是类似TVM. 但是面试官似乎是说不全是, 共同的概念是打通模型和accelerator之间的强梁, 是属于AI部署和infrastructure.

题目是DAG的topological sort. 语言自然是C++, 一开始我还写错了, 用了preorder, 带个visited. 后来test的时候, 面试官提示有问题, 我想了想反应过来了, 应该用后序, 然后将结果再反过来. 时间复杂度答对了O(N+E).

follow up问了是不是真的需要visited, 我说在当前的算法实现中是必须的, 因为即便图无环, 一个node有多个incoming edge就会重复访问. 如果实在不想要, 只能检查当前的track里是否存在当前node, 但track要保持order只能用vector, 那查找就是O(N).

后来问了浩哲, 得知course schedule实际上有第二种解法, 这可能是面试官觉得有更好解法的关键. 通过是通过了, 但是暴露了需要好好复习经典题目.

也有做的好的地方. 我觉得上次那个workshop去的很值, 看别的同学和google的学姐现场表演, 发现不管怎么样, 打代码要快, 要起码显得自己脑子很快. 然后就是如果做不出来, 不妨大胆猜一个思路, 然后从面试官的reaction和hint中再做尝试. 积极在codepad中记录思路, 因为等下面试官会拷贝记录的.说完思路要和面试官确认, 然后开始写代码.

从心情上来说, 即便我写错了一次, 突然想出来的那一刻是很开心的. 我想面试的时候, 这一刻的心情很独特, 那不是说觉得面试把握很大而开心, 而是想出思路单纯的有些小得意, 纵然这是一道基础的题目. 我偷偷的想, 有的时候, 保持和传递这种积极的情绪, 实际上是有帮助的吧. 

## Onsite 面试
三轮tech + 一轮和lead的, 其中一轮临时取消了. 我第一次参加如此多的back-to-back面试.
第一轮tech面试的题目就是对于一个linkedlist, 对于一个node, 如果该node的右边有大于当前node的值, 就remove.

比如5 -> 2 -> 13 -> 3 -> 8, 最后就应该是13 -> 8. 这道题我就想用postfix了. 那么如何得到右边最大的node呢? 我的想法是reverse traverse一遍. 为了避免reverse新declare linked list的麻烦, 使用了一个stack把value推上去, 然后再一个个拿下来.

最后再正向loop一遍, 比较自己的val和postfix记录的. 有一个小问题是由于leetcode从来不需要写struct, 怎么写constructor我给忘记了. 面试官告诉的怎么写, 有一点尴尬. follow-up的问题是能否优化, 我认为postfix的array可以不用, stack一边pop下来一边放到顶上, 那实际上stack就不能满足条件, deque可以. 另外一个follow-up是对于算法本身. 面试官的意思表达的不甚清楚, 但是我想出来可以用快慢指针. 从后面开始, 实际上不用postfix, 保持一个max就可以了, 如果当前node小于, 那么等一下是要remove的, prev不动; 当前的node大于等于, prev->next到current, 相当于之前所有的小于的都跳过了. 这是一个多么完美的想法. 最后要把result再reverse回来.

最终面试官的想法, 是要用recursion. 我认为recursion是对上述最后解法的一种改进, 会更简洁, 但是思路实际是一样. 能够arrive到这一步, 我自己心里很是满足了. 一开始想的办法并不optimal, 毕竟忘记了原题, 从头想的.

后面简历重点问了quantization, 这极有可能是他们组现在在做的东西, 因为后面一个面试官也问了. 很不幸, 这部分在高通没有细研究, 我只是用了tflite. 我之前专门复习过quantization的概念, 但是这两轮面试都问的更加深, 不是粗通能够应付的. 唉, 这已经是实力的极限了, 简历上是写了quantization, 但是这也是没办法的, 不写估计也拿不到面试...

第二轮两道题目. 第一道startWord -> a1 -> a2 -> endWord, 每一个中间词要在word bank里. 运气真的是很好, 昨天刚刚做过原题433. 确认完题目之后我很快说了reasoning, 可以理解为graph traverse, 最短路径那就是bfs. 面试官无情的说你可以直接开始写了, 并且问我是不是做过这道题目. 我就说我做过哈哈哈哈哈, 也挺好的, 这下我心里也坦诚了. 就是有一个问题, 为什么他们总能发现我做过原题?

写的时候发现之前的solution并不最优. 对于一个词的每一个字母去变换并检查是不是在bank里, 是不如迭代bank并判断每一个是不是能通过current变换的. 这是经过面试官提醒发现的. follow-up里根据时间复杂度O(NNk) (其中k是word length, 因为当时写了一个loop检查current和bank里的word是不是只有一个character不一样)做优化, 面试官提醒是check的那一步. 我就想到如果我们保证每一个word都在wordbank里, 可以提前对于word bank里的词俩俩比较, 这样后面就不用重复检查. 面试官点头表示满意, 我心中也有些小小的开心.

第二道题是给定ascending array构建二叉搜索树, 也是原题, 找中点递归. 在白板上推演了例子.

简历的问题还是回答的不好. 同样是quantization, 而且也问了很多qflow的细节. 看起来比较看不起qflow, 确实只是一个pytorch的wrapper一样的orz, 当时project设计就是这样...NTU的科研问了很多, AI synthetic检查这个面试官也做过, 看起来对转成CV问题用resnet比较不信任. 可惜当时完全对ML一知半解, 我不知道当时的ResNet的最后一层是不是classifier. 问了threshold是多少, 我有点疑惑, 我记得我们是搞成binary classifier, 那就是会sigmoid或者softmax, 之后不需要threshold? 还有如何把audio signal转成histogram/FFT frequency graph, 我没有特别的印象, 但是肯定使用了library没有啥问题...

第三轮临时取消了, 后面team lead就是第一轮面试我的Jack, 只是简单聊了几句. 他真的很厉害, 之前也是高通的GPU team的, 还是intel和google gpu team. 而且人很好, 虽然说的话不多, 但是使人感觉到很沉静, 他的关注一直很集中, 这是我非常想要达到的状态.

我们聊了一点家常, 父母住在哪里呀什么的, 最后他送我到门口. 那一瞬间我感觉他真是一个很好的model, 跟着他工作会学到很多东西. 这是一个非常好的team和机会. 当然现在掌握并不在我手中了.

总结是leetcode都不错, 可惜简历问的深了都答不上来了. 

## Director面试
同一周的周五补上了第三轮director的面试. 主要是问问题, 简历, 和一些知识(八股). 问了一个很有意思的图论, 如果一个subset of nodes互相之间没有连接(任何两个vertex之间没有edge), 就是independent set. 问至少需要多少个edge, 能够保证一个independent set of size n. 我木有想出来, 之前肯定是学过的, 但是没有复习过, 一下子推不出来.

关于C++的部分回答稀烂. 虽然说要努力转C++, 但是目前都是leetcode用C++刷, 还没有比较系统的学习, 以及开发大型的项目. 这和ML的状况是一样的, 有很多科研和实习的AI项目, 但是却没有上过很多相应的课, 导致学的不是很系统, 都是工程中需要的一些碎片知识. C++和AI是目前两个急需系统学习的部分.


# TikTok
I am happy to say that TikTok has been the single worst company that I have ever dealt with, so TikTok, **** you!

当然, 我是没这个腰板这么说. 连着做了三周TT的OA, 第一种题目有问题(但是实际上我做崩了, hackrank不能识别我的摄像头导致我只能借电脑, 别人又急着用, 烦死了我一个小时就放弃了), 第二周做的OA系统丢失了, 第三周又做.

## 第一轮
两个问题, DNS和routing. Routing的部分是问switch当中干了什么, 我忘记了, 含糊的说forwarding table和longest prefix matching.

题目第一道忘记了, 也是leetcode原题, 很快写出来了. 第二题是接雨水, 我早忘记了双指针的方法, 写的前缀后缀数组的方法. follow-up一道升级接雨水, 三维的buildings, 我还是使用了原来的方法, 只不过四个方向. optimal的解估计是前一题双指针, 这一题改进一下. 

面试官人很好, 即便不是最优解, 也说面试很好, 让我过了.

## 第二轮
上来就遇到了技术问题, hackrank再一次无法启动我的摄像头, 导致全程我都开不了摄像头. 心态上没有受到影响, 不知道评估上会不会.

题目是一道graph dp. 给定一个cities的graph, 再给一个path, 比如 beijing -> shanghai -> axiba, path中的每一个node都有可能不合法(不出现在map里, 或者并不是前一个neighbor), 修改path至一个valid path, 并且要修改成本最小(修改成本就是改变node string需要改动的字母数量)

面试官人也很好, 逐字逐句的读了题目, 我也就努力的communicate了一下. 结果就没理解题目的意思, 一下子非常困惑到底是要这个path干嘛, 一度以为成本就是map相邻的两个node的字母差异. 教训是别说这么多的话, 静静心理解题目还是比较重要的, 特别是这道题给的题干一开始不大好理解.

理解错了题目, 写了两个helper之后发现了. 急忙想了一个naive解法: 暴力回溯, 分类讨论第一个和后面的nodes. 时间比较紧张没有测试代码. 分析时间复杂度, 应该是n**k, k是path length, 但是我说是阶乘, 当时真的是有一句话的时间, 来不及细推. 然后感觉说有optimization, 可以带备忘录, 降低时间复杂度到NK, 这应该是猜对了.

很可惜. 面试的时候就像是一个塑料袋套在脑子上, 居然理解错了意思. 不然应该能有时间写带备忘录的版本. TikTok这个组内推的学长之前说面好的都有几个, 故此我一直觉得是陪跑. 现在看起来, 准备的不太充分, 至少应该好好休息的. 准备的两天刷了20+新题目, 用处不大, 应该多投入时间复习经典题目.

TT啊TT, 接连两年, 两次折戟二面. 借口可以很多, 实力确实技不如人. 下一次见到你, 我是否状态会更好呢?

# 接高通offer之后
11月中旬, 很幸运的接到高通的return offer. 拿着return去问black sesame, 也愿意给offer. 高通, 黑芝麻, 还有Tacit start up, 选择了高通.

接offer的那一周以及之后, 还是有几个面试, 不过已经记得不认真了. 一个Y Combinator上找的AI startup, 一个handshake上面的startup, 都感觉很有希望, 不过我直接说不继续面了. 

后面AMD reach out, 超级兴奋, 特地准备. HR call聊的很好, 临到末尾, 问了国籍, 然后就是经典export license, 然后全剧终. 他们需要尽快开始的candidate, EL会等太久.