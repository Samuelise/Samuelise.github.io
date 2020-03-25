---
title: Tensorflow 2.0 学习----回归问题
date: 2020-03-17 19:17:53
tags: tensorflow2.0神经网络
---

## 神经网络导入

成年人大脑中包含了约1000亿个神经元，每个神经元通过树突获取输入信号，通过轴突传递输出信号，神经元之间相互连接构成了巨大的神经网络，从而形成了人脑的感知和意识基础。

![典型生物神经元结构](https://github.com/Samuelise/img-folder/raw/master/img/%E5%85%B8%E5%9E%8B%E7%94%9F%E7%89%A9%E7%A5%9E%E7%BB%8F%E5%85%83%E7%BB%93%E6%9E%84.jpg)

1943年，提出了模拟生物神经元机制的人工神经网络的数学模型。抽象成下面的数学结构：

![神经元数学结构](https://github.com/Samuelise/img-folder/raw/master/img/%E7%A5%9E%E7%BB%8F%E5%85%83%E6%95%B0%E5%AD%A6%E7%BB%93%E6%9E%84.jpg)

## 线性问题

神经元输入向量$x=[x_1,x_2,x_3,...,x_n]^T$，经过函数映射：$f_\theta:x\rightarrow y$后得到输出y，其中$\theta$为函数自身的参数，在一种简化的情况下，即线性变换：$f(x)=w^Tx+b，w=[w_1,w_2,w_3,...,w_n]^T$，展开成标量形式:$f(x)=w_1x_1+w_2x_2+w_3x_3+...+w_nx_n+b$，参数$\theta$确定了神经元的状态，固定参数即可确定此神经元的处理逻辑。

对于N输入的神经元模型，只需采样N+1组不同数据点即可完美求解出参数。但实际上，对于任何采样点，都有可能存在一定的误差，我们假设误差𝜖满足均值为𝜇，方差$𝜎^2$的高斯分布，所以采集到的样本为𝑦 = 𝑤𝑥 + 𝑏 + 𝜖, 𝜖~𝒩(𝜇, $𝜎^2$)。

下面以w=1.477，b=0.089为例

```python
#生成样本数据
data=[]  #样本集
for i in range(100):       #100个点
    x=np.random.uniform(-10.,10.)         #随机采样输入x
    eps=np.random.normal(0.,0.1)          #高斯分布的误差
    y=1.477*x+0.089+eps               #加入误差的函数
    data.append([x,y])                    #把数据加入样本集中
data=np.array(data)                     #转换成numpy数组
```



在引入误差后，即使简单如线性模型，如果只采集两个点，也会带来很大的估计误差。为了减少估计误差，可以通过采样多组数据样本集合$𝔻$ ={$(𝑥^{(1)}, 𝑦^{(1)}), (x^{(2)}, x^{(2)}), … , (𝑥^{(𝑛)}, 𝑦^{(𝑛)})$}，然后找出一条最好的直线，使它尽可能让所有点，离这条直线的误差之和最小。常用的用预测值𝑤𝑥(𝑖) + 𝑏与真实值𝑦(𝑖)之间的差的平方和作为总误差$L：L=\frac{1}{n}\sum^{n}_{i=1}(wx^{(i)}+b-y^{(i)})^2$，然后搜索到一组w,b，使得$L$的值最小。

```python
#function:计算误差
#param:b,w---待优化参数     points---样本点
#return：均方误差（float）
def mse(b,w,points):
    totalerror=0    #均方差误差
    for i in range(0,len(points)):
        x=points[i,0]               #第i个点的x值
        y=points[i,1]               #第i个点的y值
        totalerror+=(y-(w*x+b))**2         #计算均方误差
    return totalerror/float(len(points))
```



## 寻找方法

### 梯度下降法

在单输入的情况下，可以通过计算推到获得一组准确的解（解析解），但在一些更复杂的情况，有可能求不出解析解，而随机的搜索试错又太耗时间。

梯度下降算法(Gradient Descent)是神经网络训练中最常用的优化算法，配合强大的图形处理芯片GPU(Graphics Processing Unit)的并行加速能力，非常适合优化海量数据的神经网络模型，自然也适合优化我们这里的神经元线性模型。函数的梯度(Gradient)定义为函数对各个自变量的偏导数(Partial Derivative)组成的向量。![函数及其梯度向量](https://github.com/Samuelise/img-folder/raw/master/img/%E5%87%BD%E6%95%B0%E5%8F%8A%E5%85%B6%E6%A2%AF%E5%BA%A6.png)图中𝑥𝑦平面的红色箭头的长度表示梯度向量的模，箭头的方向表示梯度向量的方向。可以看到，箭头的方向总是指向当前位置函数值增速最大的方向，函数曲面越陡峭，箭头的长度也就越长，梯度的模也越大。

通过上面的例子，我们能直观地感受到，函数在各处的梯度方向∇𝑓总是指向函数值增大的方向，那么梯度的反方向−∇𝑓应指向函数值减少的方向。

利用这一性质，我们只需要按照$x'=x-𝜂∙∇𝑓$ 更新迭代$x'$ ，就能获得越来越小的函数值，其中𝜂用来缩放梯度向量，一般取较小的值，如0.01，0.001等，对于一维函数，上述向量形式可以退化成标量形式：$𝑥′ = 𝑥 − 𝜂 ∙\frac{d𝑦}{d𝑥}$ ，迭代更新$x'$若干次，这样得到的$x'$处的函数值$y'$是最小的。

这里我们要最小化的是$L=\frac{1}{n}\sum^{n}_{i=1}(wx^{(i)}+b-y^{(i)})^2$，因为我们要优化的是w，b，所以有                         $𝑤′ = 𝑤 − 𝜂\frac{𝜕L}{𝜕𝑤}$                             $b'=b- 𝜂\frac{𝜕L}{𝜕b}$

### 梯度计算

```python
#function:计算导数，更新w,b
#param:b_current,w_current---当前b,w的值  points---样本点  lr---学习率
#return：[new_b,new_w]---新的[b，w] 
def step_gradient(b_current,w_current,points,lr):
    b_gradient=0  #初始化
    w_gradient=0
    m=float(len(points))    #总共m个点
    for i in range(0,len(points)):
        x=points[i,0]
        y=points[i,1]
        b_gradient+=(2/m)*((w_current*x+b_current)-y)   #L对b求偏导
        w_gradient+=(2/m)*x*((w_current*x+b_current)-y)  #L对w求偏导
    new_b=b_current-(lr*b_gradient)  #新的w，b
    new_w=w_current-(lr*w_gradient)
    return [new_b,new_w]
```

### 迭代更新

```python
#function:梯度更新
#param:points---样本点  starting_b---起点b  starting_w---起点w
#param:lr---学习率   num_iterations---迭代次数
#return:[b,w]---迭代结果[b,w]
def gradient_descent(points,starting_b,starting_w,lr,num_iterations):
    b=starting_b
    w=starting_w
    for step in range(num_iterations):
        b,w=step_gradient(b,w,np.array(points),lr)
        loss=mse(b,w,points)
        if step%50==0:    #每50次打印一下当前的loss，w,b
            print(f'iteration:{step},loss:{loss},w:{w},b:{b}')
    return [b,w]
```



### 测试

```python
#主函数
def main():
    lr=0.01    #学习率
    initial_b=0        #初始化b
    initial_w=0        #初始化w
    num_iterations=1000           #训练次数
    [b,w]=gradient_descent(data,initial_b,initial_w,lr,num_iterations)
    loss=mse(b,w,data)
    print(f'final loss:{loss},w:{w},b:{b}')  #打印最终结果
```

### 测试结果

![测试结果](https://raw.githubusercontent.com/Samuelise/img-folder/master/img/测试结果.png))

可见，此方法在解决线性问题参数求解时的强大之处，在非线性或非常复杂的问题上，可能梯度下降得到的并不是全局最优解，但在实践中，求得的数值解，仍能发挥很好的效果，可以直接使用。

## 总结

换一个角度，该问题其实是一个连续量的预测问题，从数据集中训练出模型，从而预测未出现样本的输出值。更准确的说，是使用线性模型去逼近真实模型的线性回归问题。