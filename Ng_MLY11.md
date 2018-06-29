# 端到端深度学习

## 47 端到端学习的增长

假设你想要构建一个系统来检测在线商品复审和自动地告诉你作者是否喜爱这个产品。例如，你希望识别如下的检查作为
高度的正面：

这是一个伟大的拖把！

如下为高度的负面：

这个拖把质量差--我后悔买它。

识别积极vs. 消极意见的问题被称为“情感分类”。为了建立这个系统，你可能建立一个有两部分的“管道”：

1. 分析程序：一个系统通过识别最重要的单词来给文本注释信息。例如，你可能使用分析程序来标记所有的形容词和名词。
你会因此得到如下的注释过的文本：
	
这是一个伟大_(形容词)的拖把_(名词)!

2. 情感分类：一个把注释后的文本当输入且预测整体情感的学习算法。分析程序饿注释能够极大的帮助这个算法：
通过给形容词一个较高的权重，你的算法会能够快速的在类似“伟大的”这种重要的词上训练，并忽视不是很重要的
词例如“这”。

我们能可视化你的两部分“管道”如下所示：

Original text ------> Parser -----Annotated text----->  Sentiment Classifier ---->Output 

现在有个趋势使用一个讯息算法来替代管道系统。对于这个问题的一个*端到端的学习算法*会简单的输入未加工的，
原始的文本“这是一个伟大的拖把！”，并尝试直接识别情感：

Original text  --------------> Learning algorithm -----------------> Output

神经网络通常使用端到端的学习系统。术语“端到端”反映了我们让学习撒U你发直接的从驶入到想要的输出的现实。
即，学习算法直接把“输入端”和“输出端”通过系统相连。

在数据很多的问题上，端到端系统获取了显著的成功。但是他们并不是一直是好的选择。下面的几个章节会给更多的
端到端系统的例子，也会给你建议在什么时候应该或不应该使用它们。


## 48、 更多的端到端学习例子

假设你想要构建一个语音识别系统。你可能构建一个有三个部分的系统。

Audio--->Compute features ---Hand-designed MFCC features--> Phonemene recognizer --Recognized phonemes-->Final recognizer--->Transcript

各部分工作如下：

1. 计算特征：提取手动设计的特征，例如MFCC（mel-frequency cepstrum coeffcients）特征，其尝试忽略一些小的不相关的
特性，例如音高后捕捉说话内容。

2. 音素识别：一些语言学家相信有一些基础的声音单元，叫做音素。例如，"keep"中的"k"和"cake"中的"c"音素 一样。这个系统尝试
识别语音片段中的音素。

3. 最终识别：尝试把识别的音素序列串起来成输出转录。

相较而言，一个端到端的系统可能输入一个语音片段，并尝试直接输出转录：

Audio --------------> Learning algorithm ----------------------> Transcript

目前，我们只描述了机器学习“管道”是完全线性的：输出依次的从一个阶段通过到另一个阶段。管道能更复杂。
例如，下面是一个自动汽车的简单结构：

1.           ----->Detect cars----------
2. Camera---							------->Plan path for car------->Steering direction
3.      	 ----->Detect pedestrians---

这有三个部分：一个通过使用相机图片检测其他汽车；一个检测行人；最后那个部分给我们自己的车规划一个路线来避开汽车和行人。

不是管道中的每一个部分都是要学习的。例如，关于“机器人运动规划”的著作中有很多算法给汽车最后的路线规划。这些算法许多不涉及学习。

相较而言，一个端到端的方法可能尝试使用传感器输入和直接输出驾驶方向：

Camera images ---------------> Learning algorithm------------>Steering direction

即使端到端学习只在很多地方成功应用，并不一直是最好的方法。例如，端到端的语音识别工作的很好。但是我对端到端的自动汽车驾驶
持怀疑态度。接下来的几个章节会解释原因。


## 49、端到端学习的正反面

考虑一个和我们前面例子一样的语音管道：

Audio ----> Compute features ---Hand-designed MFCC features-->Phonemene recognizer ---Recognized phonemes--> Final recognizer--->transcript

这个管道的许多部分都是“手工设计的”

- MFCC是一个手工设计的语音特征的集合。尽管他们提供了一个合理的语音输入的概要，但也扔掉了一些信息从而简化了输入信号。

- 音素是语言学家的发明。他们是不完美的语音声音的代表。从程度上来说，音素是现实的一个不好的近似，迫使一个算法使用音素
来表示会限制语音系统的性能。

这些手动设计的部分限制了语音系统的潜在性能。然而，允许手工设计部分也有一些优势：

- MFCC特征对于一些语音特性有鲁棒性，从而不会影响内容，例如说话者的音调。因此，他们帮助学习算法简化了问题。

- 音素是一个合理的语音代表，他们也能帮助学习算法理解基本的声音部分并因此提高性能。

有更多的手工设计的部分通常允许语音系统在更少的数据下学习。通过MFCCs捕捉的手工设计的知识和音素“补充”我们算法
从数据中获得的知识。当我们没有很多数据的时候，这个知识很有用。

现在，考虑端到端的系统：

Audio--------->Learning algorithm--------------->transcript

这个系统缺少手工设计的知识。因此，当训练集很小的时候，他可能比手工设计的管道表现得要差。

因此，当训练集大的时候，他不会受到MFCC或者音素表示限制的妨碍。如果学习算法是一个足够大的神经网络，且如果他有足够
的训练数据，他有潜力做的很好，并有可能接近左右误差率。

端到端学习系统尝试在有很多标记好的数据的时候“对于两端”表现得很好。在这个例子中，我们需要一个大的（语音，转录文本）数据集对。
当这个数据类型无法获得的时候，谨慎使用端到端的学习。

如果你工作在一个训练集很小的机器学习问题上时，你的算法的大部分知识都要来自于你的人工观点。即，来自于你的“手工设计”部分。

如果你选择不使用端到端系统，你必须要决定你的管道中的步骤，及他们怎么加在一起。在接下来的几个章节中，我们会给出一个设计这样的
管道的建议。