# 自然语言处理面试题汇总

## Transformer

### 1.为什么Transformer 需要进行 Multi-head Attention？

简单回答就是，多头保证了transformer可以注意到不同子空间的信息，捕捉到更加丰富的特征信息。其实本质上是论文原作者发现这样效果确实好。

同时研究人员通过分析发现：

+ 同一层中的多数头的关注模式是一样的，但总有那么一两个头与众不同。

+ 层数越高不同头之间的方差越大。

+ 头的功能可以分为三类：关注左右的Token、关注语法、关注罕见词。

+ 对多头进行适当剪枝不影响模型效果。

解析: https://www.zhihu.com/question/341222779

### 2.Transformer为什么Q和K使用不同的权重矩阵生成，为何不能使用同一个值进行自身的点乘？

注解：简单回答就是，使用Q/K/V不相同可以保证在不同空间进行投影，增强了表达能力，提高了泛化能力。

K和Q使用了不同的W_k, W_Q来计算，可以理解为是在不同空间上的投影。正因为有了这种不同空间的投影，增加了表达能力，这样计算得到的attention score矩阵的泛化能力更高。这里解释下我理解的泛化能力，因为K和Q使用了不同的W_k, W_Q来计算，得到的也是两个完全不同的矩阵，所以表达能力更强。

解析：https://www.zhihu.com/question/319339652

### 3.Transformer计算attention的时候为何选择点乘而不是加法？两者计算复杂度和效果上有什么区别？

答案解析：为了计算更快。矩阵加法在加法这一块的计算量确实简单，但是作为一个整体计算attention的时候相当于一个隐层，整体计算量和点积相似。在效果上来说，从实验分析，两者的效果和dk相关，dk越大，加法的效果越显著。

### 4.为什么在进行softmax之前需要对attention进行scaled（为什么除以dk的平方根），并使用公式推导进行讲解

将attention进行缩放，使得其方差为1，避免输入的数量级很大时，softmax梯度消失为0，造成参数更新困难。

解析：https://www.zhihu.com/question/339723385/answer/782509914

### 5.在计算attention score的时候如何对padding做mask操作？

答案解析：padding位置置为负无穷(一般来说-1000就可以)。

## BERT
### 1.BERT有什么
答案解析：
* bert认为mask之间是相互独立的，但实际上mask和mask之间肯定是有联系的，但是bert在预训练的时候没有考虑到这点。
* bert的预训练存在mask字符，但是在微调的时候没有mask字符，造成预训练和微调存在gap。
