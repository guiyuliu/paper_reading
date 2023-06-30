### QAT

LSQ

文章是把量化参数 step size (也叫 scale) 也当作参数进行训练。这种把量化参数也进行求导训练的技巧也叫作**可微量化参数**。在这之后，高通也发表了增强版的 LSQ+，把另一个量化参数 zero point 也进行训练，从而把 LSQ 推广到非对称量化中。

[Fake quantization](https://zhuanlan.zhihu.com/p/404445525) 

原来的浮点输入x就转换成了整型q_x,但是很不幸，pytorch并不支持对整型的运算，所以如果我们要把这个量化算法嵌入模型中，就需要对其进行一个反量化的操作，反量化就是将整型q_x再变成浮点型

**注意:**这里进行一个反量化的操作，主要就是一个方便训练的方式，且**q_x经过反量化之后在逻辑上与q_x是等价的，而且与原始数据x并不相等，**这就是为什么量化训练会存在精度损失的原因之一。**因为 q_x经过反量化以后在逻辑上是一致的，所以在训练时我们方便训练就做反量化，而在推理时，我们就不需要做反量化了，让处理器跑整型即可。**

```python
#反量化，将整型转换为浮点
def dequantize_tensor(q_x, scale, zero_point=0):
     return scale * (q_x - zero_point)
```

[LSQ](https://zhuanlan.zhihu.com/p/406891271)

链接：https://zhuanlan.zhihu.com/p/396001177
