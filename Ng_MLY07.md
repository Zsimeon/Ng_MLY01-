# 和人类等级的性能对比 


# 33、为什么我们要和人类水平的性能比较

许多机器学习系统旨在用自动化替代人类做的好的事情。包括图像识别，语音识别和垃圾邮件分类。学习算法也
有了很大的提高，使得我们现在在越来越多的任务上要远超人类水平。

进一步说，有一些原因来很容易的构造一个机器学习系统，如果你尝试一个人类能做的很好地任务的时候：

*1、从人类标记者中轻松的获得数据。*例如，因为人类识别猫咪图片很厉害，人类能为你的学习算法直接提供
高准确度标签

*2、误差分析可以利用人类的直觉。*假设一个语音识别算法比人类级别的识别要差。它错误的把语音片段转录
了，“This recipe calls for a pear of apples”，把“pair” 写作“pear”。你能根据人类的直觉并尝试理解
人类想要得到的正确的翻译，并使用这个知识来修改学习算法。

*3、使用人类级别的性能来估计最优误差率并同时设置一个“期望误差率”。*假设你的算法在一项任务中获得了
10%的误差，但是人类是2%的误差。那么我们知道最优的误差或者更低，可避免的误差至少是8%。因此你应该尝
试减少偏差的技巧。

即使第三项听起来不重要，我发现有一个有道理的且可获得的目标误差率帮助加速团队进程。知道你的算法
有高可避免的偏差是非常有用的并打开了一系列可以尝试的选项。

也有些工作即使是人类也做的不好。例如，选择一本书来推荐给你；或者选择一个广告来在网站上显示给使用者；
或者预测股票市场。计算机以及远超大部分人类在这些工作上的性能了。对于这些应用，我们走进了以下问题：

- *很难获得标签。*例如，对于人类标记者很难注释一个使用者数据库用“最优的”书推荐。如果你操作一个网站
或者app来卖书，你能够获得通过把书展示给读者并观察他们买什么来获得数据。如果你不运行这样一个网站，
你需要找到一个更有创造性的方法来获得数据。

- *人类直觉更难计算。*例如，几乎没人能够预测股票市场。因此，如果我们的股票预测算法不比随机猜好的话，
很难搞明白怎么来提升他。

- *很难知道什么是最优误差率和合理的期望误差率。*假设你已经有一个图书推荐系统并工作的很好。
你怎么知道他在没有人类帮助的情况下能有多少提升呢？


# 34、怎么定义人类级别的性能

假设你工作在一个医学图像应用，能自动的从X光照片做诊断。一个普通人没有医学背景的而只有一些基础
训练的在这个工作上有5%的误差。一个小的医生组对每个图片讨论能有2%的误差。这俩误差哪一个定义了
“人类级别的性能”？

在这个例子中，我会把2%作为人类水平的性能代表我们的最优误差率。你有能够设置2%作为期望的性能等级
因为从前面章节得到的比较人类水平性能的三个原因都能在这里使用：

- *从人类标记者处能轻松的获得被标记的数据。*你能够用一个医生团队来给你提供标签，并有2%的误差率

- *可以根据人类的之间来做误差分析。*通过一整个医生组的讨论，你能依据他们的直觉。

- *使用人类水平的性能来估计最优误差并同时设置可获得的“期望误差率”。*把2%的误差率当作我们的估计
最优误差是很合理的，因为对于一个医生团队获得2%的误差是有可能的。相反，使用5%或10%作为最优误差率的
估计是不合适的，因为我们知道这些估计值太高了。

当涉及到获得标签数据时，你可能不想用整个医生团队来讨论每一幅图像因为他们的时间很宝贵。假设你能够有
一个中级的医生标记大部分的例子，只把最难得例子给最有经验的医生或医生团队。

如果你的系统现在有40%的误差，那么它并不关心你是否使用一个中级医生（10%误差的）或者一个有经验的医生
（5%误差的）来标记你的数据并提供指引。但是如果你的系统已经有了10%的误差，那么定义人类水平为2%能给你
更好地工具来使你提升你的系统。


# 35、超越人类水平性能

你在语音识别上工作并有了语音片段的数据集。假设你的数据集有很多噪声语音片段因此即使是人也有10%的误差。
假设你的系统已经能达到8%的误差。你能使用使用第33章提到的任意一个技巧来继续快速前进么？

如果你能定义一个数据子集，人类在其中能显著的超过你的系统，那么你能继续使用这些技巧来驱动快速进展。
例如，假设你的系统比人类识别噪声环境下的语音要好得多，但是人类仍然在转录快速说话中表现得更好。

对于有快语速演讲数据的子集：

1、你能够继续获得人类的比你的算法输出质量高的多的抄录。

2、你能根据人类的直觉来理解为什么他们能正确的听见快速的说话但你的系统不行。

3、你能使用人类在快语速的演讲上的性能作为期望的性能目标。

更通常的说，只要有人类正确而你的算法错的开发集，那么前面提到的很多技巧都能用。即使在整个开发、测试集
的平均水平上，你的性能已经超过人类水平的性能了。

有很多的重要机器学习应用，其中机器已经超过人类的水平了。例如，机器能更好地预测电影星级，货车能花多长
时间来行驶到某地，或者是否批准贷款。一旦有人能很难辨别的例子且算法明显的错了，那么上面提到的技巧的
一个子集就能用。因此，在机器已经超过人类的地方进展通常更慢，而在机器仍需要赶超人类的地方进展会更快。
