# 40 从训练集退管到开发集

假设你在一个训练集合开发、测试集来自不同分布的集合上应用机器学习。训练集包括网络图片+手机图片，开发、测试集只包含手机图片。然而，算法工作的并不好：可能会比你想象的有更高的开发、测试集误差。以下是些会出错的可能性：

1. 在训练集撒花姑娘工作的不好。这是在训练集上有高（可避免的）方差的问题。

2. 在训练集上工作的很好，但是不能很好地推广到和训练集从同一分布中提取的未知的数据上。这是高偏差。

3. 很好地推广到和训练集从相同分布中提取的新数据上，但是在开发、测试集分布中提取的数据上不行。我们把这叫做数据不匹配，因为这是有训练集数据和开发、测试集数据匹配的很差导致的。

例如，假设人类在猫咪识别任务上获得将近完美的性能。你的算法获得了以下：

- 在训练集上有1%的误差

- 在算法没见过的、和训练集来自同一分布的数据上有1.5%的误差

- 开发集上有10%的误差

在这种情况下，你明显有了数据不匹配的问题。为了解决这个问题，你需要尝试把训练集变的和开发、测试集更相似。我们在后续会讨论一些技巧。

为了断定算法对上述3个问题的容忍程度，有另一个数据集是很有效的。特别是，相对于给撒U你发所有能获取的训练数据，你可以把它分为两个子集：算法会在其上训练的实际训练集，和一个我们称为“训练开发”集的单独的集，我们不再其上训练。

你现在有了四个数据子集：

- 训练集。算法会在其上学习的数据（例如，网络图片+手机图片）。不用和我们真正关心的数据（开发、测试集分布）来自相同的分布。

- 训练开发集：这个数据和训练集（网络图片+手机图片）来自相同分布。他通常比训练集要更小；只要足够大来让我们评估和掌握我们学习算法的进展就可以了。

- 开发集：和测试集来自喜爱那个痛分布，他反映了我们最终关心想要表现得好的数据分布（手机图片）

- 测试集：和开发集来自相同分布（手机图片)

有了这四个分开的数据集，你现在可以评估：

- 训练误差，通过评估训练集

- 算法的推广到来自训练集分布新数据的能力，通过评估训练开发集

- 算法在你关心的任务上的性能，通过评估开发或测试集。

大多数的章节5-7中的关于选择开发集尺寸的指导原则也使用于训练开发集。


# 41 识别偏差、方差和数据不匹配误差

假设人类在猫咪检测任务上获得了接近完美的性能（≈0%），因此最优误差率也是大约0%。假设你有：

- 训练集上有1%的误差

- 训练开发集上有5%的误差

- 开发集有5%的误差

这告诉了你什么？由此，你知道你有高方差。前面讨论过的减少方差的技巧会帮助你有进展的。

现在，假设你的算法获得了：

- 训练集上有10%的误差

- 训练开发集上有11%的误差

- 开发集有12%的误差

这个告诉你。你在训练集上有很高的可避免的偏差。也就是说，算法在训练集上表现得很差。偏差减少技巧会有帮助的。

上面的两个例子，算法只遭受了高可避免偏差或者高方差。对于算法很可能忍受来自任一自己的高可避免偏差，高方差，和数据不匹配。例如：

- 训练集上有1%0的误差

- 训练开发集上有5%的误差

- 开发集有20%的误差

这个算法遭受了高可避免偏差和数据不匹配。然而，在训练集分布上没有受到高方差。

通过把不同类别的误差做成表格的项，能更轻松的理解不同类别的误差之间有什么联系。

|---------------------------------------|分布A：网络+手机图片-------|--------分布B：手机图片-----|

----------------------------------------------------

|--------------人类水平--------------|“人类水平的误差”（≈0）-----|----------------------------------|

----------------------------------------------------

|算法在训练过的样例上的误差---|“训练误差”（10%）---------|----------------------------------|

----------------------------------------------------

|算法在未训练过的样例上的误差|“训练开发集误差”（11%）|“开发测试集误差”（20%）--|

继续猫咪图片检测的例子，你能发现在x轴上有两种不同的数据分布，我们有三种类型的误差：人类水平的误差，在算法训练过的样例上的误差，在算法未训练过的样例上的误差。我们可以用前面章节我们识别的误差类别来填这个表格。

如果你期望的话，你可以填表格中的剩余的两个空：你能填右上的空（人类在手机图像上的性能）通过文一些人来标记你的手机猫咪图片和测量他们的误差。你能填下一个空通过拍摄手机猫咪图片（分布B）并把一小部分放进训练集，以至于神经网络也能在其上训练。填这两个额外的项可能会有事给额外的观点关于算法在两个不同的数据分布上工作的怎样（分布A和分布B）

通过了解算法忍受哪种误差最多，你会更好地决定是否要关注与减少偏差、减少方差，或者减少数据不匹配。


# 42 处理数据不匹配

假设你开发了一个语音识别系统，并在训练集和训练开发集上运行的非常好。然而，在你的开发集上表现得很差：你有数据不匹配问题了。你能做什么？

我建议你：（i）努力搞清楚训练集合开发集分布中数据的哪些属性不一致。（ii）努力找到更多的能更好的匹配你的算法有麻烦的开发集样例的训练数据。

例如，假设实验语音识别开发集上的误差分析：你手动的尝试100个例子，努力理解算法在那里出了错。你发现你的系统表现得差是因为开发集中大多数的语音片段都是在有车的情况下采集的，而大多数的训练样例都是在一个安静的背景下记录的。引擎和路面噪音显著的损害你的语音系统的性能。在这种情况下，你需要尝试获取更多的有车的情况下采集的语音片段组成的训练数据。误差分析的目的是理解训练集和开发集之间的现状的区别，是什么导致了数据不匹配。

如果你的训练和训练开发集包括在车内的语音片段，你会再次确认你的系统的性能在这个数据子集上。如果在训练集的车数据上表现得好，但是在训练开发集的车数据上表现得不好，那么这进一步的正是了获得更多的车数据的假设会有帮助。这就是为什么我们在前面的章节讨论在你训练集中的一些数据和你的开发、测试集来自同一分布的可能性。这么做允许你比较你的在训练集中车数据和开发、测试集车数据的性能。

不幸的是，这个方法没有保证书。例如，如果你没有任何办法来获得更多的能更好地匹配开发集数据的训练数据，你可能不会有一个清楚的路来提高性能。


# 43 人造数据合成

你的语音系统需要更多的数据听起来好像他是来自车内。相比于在开车的时候收集大量数据，有一个简单的方法来获得这些数据：通过手动的合成。

假设你获得了大量的车、路噪音语音片段。你能从一些网站中下载这些数据。假设你也有大量的人们在安静的房间中讲话的训练集。如果你把一个人说话的语音片段“加”到车、路面噪音的语音片段中，你会获得语音片段听起来像是人们在嘈杂的车里说话。使用这个方法，你能“合成”，大量的数据听起来像是在车中收集的一样。

更通常的说，有一些情况下人造合成数据允许你创造一个大的数据集，并能有效的匹配开发集。让我们使用猫咪图片检测器作为第二个例子。你注意到开发集图像有很大的运动模糊，因为他们趋向于来自手机用户，在拍摄照片时会轻微的抖动他们的手机。你能用来自训练集的网络无模糊图片，并给他们添加上模拟的运动模糊，以此来使他们更接近开发集。

记住，人造合成数据也有他的挑战：通常创造的合成数据对于人类而言比对于电脑看上去更逼真。例如，假设你有1000小时的语音训练数据，但是只有1小时的汽车噪声。如果你重复使用者相同的1小时汽车噪声和不同的初始1000小时训练数据，你最终会得到一个相同汽车噪声不断重复的合成数据集。然而人听这个语音可能不会分辨-所有的汽车噪声对于我们而言都差不多-很可能一个学习算法会“过拟合”这1小时的汽车噪声。因此当他推广到一个汽车噪声可能听起来不一样的新语音片段的时候，可能会适应的很差。

或者，假设你有1000小时罕有的汽车噪声，但是他们全部只来自10个不同的车。在这种情况下，算法可能“过拟合”这10个车，并在从另一个车取得的语音信号片段上测试时表现得不好。不幸的是，这个问题很难发现。

再举一个例子，假设你构建一个计算机视觉系统来识别汽车。假设你和一个视频游戏公司合作，他们有一些汽车的计算机图形模型。为了训练你的算法，你使用这些模型来生成合成汽车图片。即使合成图片看起来很逼真，这种方法（已经被很多人独立提出）可能不会工作的很好。这个视频游戏中可能有大约20个车。建立一个汽车的3D汽车模型很贵；如果你曾玩过这个游戏，你可能不会注意到你一次又一次的看过相同的车。但是和路上的所有汽车集相比-因此你在开发测试集汇总看到的-这个20合成汽车集值捕捉了极小的世界汽车分布的一部分。因此，如果你的100000训练样本全都来自这20个车，你的系统会“过拟合”这20中汽车设计，并会在推广到包含其他汽车设计的开发、测试集时表现得很差。

当合成数据时，考虑你是否真的合成一个典型的样例集。尝试避免给合成数据一些特性使得它可能对于学习算法能从非合成样例中辨别出合成数据-例如所有的合成数据来自一个20种车的设计，或者所有的合成语音来自只有一小时的误差。这个建议可能很难遵循。

当在合成数据上工作时，在我们生成细节的、能够和真实分布数据足够接近的、能有显著的效果合成数据之前，我的团队通常花费几周时间。但是如果你能够得到正确的细节，能忽然获得一个比之前大得多的训练集。