# Getting-Started-with-TensorFlow-
对这本书的翻译及总结：

第5章：深度学习

深度学习在许多应用中，例如图像识别和语音识别取得了前所未有的成果。

原因有二：硬件的进步，新处理器的可用性，例如图形处理单元(gpu)； 第二个是可以找到更多的数据集来训练一个系统，需要对某一深度的架构进行训练，并具有高维度的输入数据。

深度学习由一组方法组成，这些方法允许系统在多个层次上获得数据的分层表示。这是通过组合简单的单元(而不是线性)来实现的，每一个单元都从输入层开始，从输入级转换为一个更高层次的表示，稍微抽象一点。如果有足够多的这些转换，就可以学习相当复杂的输入输出函数。

例如，对于分类问题，代表的最高级别，突出了与分类相关的输入数据的各个方面，从而抑制对分类目的没有影响的内容。比如图像分类系统(人脸识别器):每个块逐渐提取输入图像的特征，
从前面的块中处理已经预先处理的数据，提取输入图像的越来越复杂的特性，从而构建一个基于深度学习的系统的分层数据表示。pixel --> edge --> texture --> motif --> part --> object
在文字识别中则是：character --> word --> word group --> clause --> sentence --> story

因此，深度学习架构是一个多层次的架构，由简单单元（simple units）组成，都要接受培训，其中许多都带有非线性转换（non-linear transformations）。每一个单元都转换其输入，以改进其属性，仅为分类目的选择和放大相关方面，以及它的不变性（invariance），即它倾向于忽略无关的方面，忽略不计。

多级非线性转换,因此,深度约5至20的水平,深入学习系统学习,可以实现非常复杂的功能,同时对最小的相关细节非常敏感,对输入数据的大变化无关的方面极其不敏感,可以在对象识别的情况下:图像的背景亮度,或代表对象的位置。

深度神经网络的两种重要类型:卷积神经网络(CNNs)，主要针对分类问题，然后是递归神经网络(RNNs)，针对自然语言处理(NLP)问题。

一、CNN

卷积神经网络(CNNs)是一种面向神经网络的特殊类型，在许多实际应用中取得了优异的效果，特别是图像中的目标识别。实际上，CNNs的目的是处理以多个数组形式表示的数据，例如，通过包含像素颜色强度的3个二维数组来表示彩色图像。CNNs与普通神经网络的本质区别在于前者直接在图像上操作，后者则直接在图像中提取。因此，与普通的神经网络不同，CNN的输入将是二维的，而特征将是输入图像的像素。CNN是几乎所有识别问题的主要方法。

CNN结构：CNN使用三个基本概念:局部接受域、卷积和池。

CNNs背后的一个概念是本地连接。事实上，CNNs利用在输入数据中可能存在的空间相关性。第一个后续层的每个神经元只连接一些输入神经元。这个区域称为局部接受区（local receptive field）。在下面的图中，它由黑色5x5平方表示，它收敛于一个隐藏的神经元:当然，隐藏的神经元只会处理它的接收域内的输入数据，而不是实现它之外的变化。然而，通过叠加几个层，它们是局部连接的，从而使你的单位能够处理越来越多的全局数据，而不是输入，按照深度学习的基本原理，将性能提升到始终在增长的抽象层次。

注意：本地连接的原因在于，在数组形式的数据中，例如图像，值通常是高度相关的，形成不同的数据组，这些数据可以很容易地识别出来。
每个连接都学习了一个权重(因此它将得到5x5 = 25)，而不是隐藏的神经元与一个关联的连接学习完全的偏差，然后我们将通过执行一个从时间到时间的转换将区域连接到单个的神经元，如下图所列:

这个操作叫做卷积。这样做，如果我们有28x28输入和5x5区域的图像，我们将在隐藏层中得到24x24个神经元。我们说每个神经元都有与该区域相关的偏差和5x5的权重:我们将对所有24x24神经元使用这些权重和偏差。这意味着，第一个隐藏层中的所有神经元都会识别相同的特征，只是在输入图像中放置了不同的位置。由于这个原因，从输入层到隐藏特征映射的映射称为共享权重（shared weights），而偏差称为共享偏差（shared bias），因为它们实际上是共享的。

显然，我们需要识别的图像不仅仅是一个特征的地图，所以一个完整的卷积层是由多个特征映射构成的。

在前面的图中，我们看到了三个特征映射;当然，它的数量可以增加，你可以使用卷积层，甚至有20或40个特征图。在权重和偏差的共享中，一个很大的优势是显著地减少了涉及到卷积网络的参数。考虑到我们的示例，对于每个feature map，我们需要25个权重(5x5)和偏差(共享);总共26个参数。假设我们有20个功能映射，我们将定义520个参数。有一个全连通的网络，有784个输入神经元，例如，30个隐藏层神经元，我们需要30个784x30的偏置权重，达到23.550个参数。

区别是很明显的。卷积网络还使用汇聚层（pooling layers），即在卷积层之后立即放置的层;这些简化了前一层的输出信息(卷积)。它接受从卷积层出来的输入特征映射，并准备一个压缩的(condensed)特征映射。例如，我们可以说，在所有单元中，可以用前一层的2x2区域的神经元来总结汇聚层。This technique is called pooling。

我们来总结一下:有28x28的输入神经元，接着是一个卷积层，有一个局部接受场5x5和3个特征图。我们得到的结果是一个隐藏的神经元层3x24x24。然后，在特征图3个区域的2x2上应用了max -池，得到了一个隐藏的3x12x12层。最后一层是完全连接的:它将max -池层的所有神经元连接到所有10个输出神经元，这有助于识别相应的输出。

为了减少过度拟合（over fitting），我们应用了dropout技术。这个术语指的是在一个神经网络中删除单位(隐藏、输入和输出)。决定消除哪些神经元是随机的;一种方法是应用概率，正如我们在代码中看到的那样。为此，我们定义了以下参数(要调优):

使用tf.nn.conv2d函数，它从输入张量和共享权重计算一个2D卷积。这个操作的结果将被添加到偏差bc1矩阵中。而tf.nn.relu函数是Relu函数（纠正线性单元）：在深层神经网络的隐藏层中，通常的激活函数。我们将这个激活函数应用于卷积函数的返回值。填充值是相同的，这表明输出张量输出的输入张量相同。

在卷积运算后，我们会将之前创建的卷积层的输出信息简化为汇集步骤（pooling step）。在我们的例子中，让我们取一个卷积层的2x2区域，我们将在池层的每一个点上总结信息。conv1 = max_pool(conv1, k=2)

tf.nn.max_pool函数在输入上执行最大池(max pooling)。当然，我们为每一个卷积层应用了max池，而且会有很多层的池和卷积。在池化阶段结束时，我们将有12x12x32卷积隐藏层。

最后一个操作是通过应用在卷积层上的tf.nn.dropout来减少过度拟合（overfitting），为了做到这一点，我们为概率(keep_prob)创建一个占位符，这个概率是一个神经元的输出被保存，在dropout阶段：
keep_prob = tf. placeholder(tf.float32)
conv1 = tf.nn.dropout(conv1,keep_prob)

正如您所注意到的，第二个隐藏层将有64个特性用于5x5窗口，而输入层的数量将从第一个卷积得到的层得到。我们接下来将第二层应用于卷积conv1张量，但这次我们应用64套5x5 filter，每一层为32个conv1层:

RNN（Recurrent neural networks）

RNNs的基本思想是利用输入中的顺序信息类型。在神经网络中，我们通常假设每个输入和输出都是独立于所有其他输入和输出的。然而，对于许多类型的问题，这种假设并不会带来积极的结果。例如，如果你想预测一个短语的下一个单词，了解它之前的那些单词是很重要的。这些神经网络被称为周期性，因为它们对输入序列的所有元素执行相同的计算，而且每个元素的输出，除了当前的输入外，还依赖于所有以前的计算。

RNNs处理一个连续的输入项，维持一种更新的状态向量，其中包含有关序列的所有过去元素的信息。

前面的图显示了RNN的方面，它的展开版本，解释了整个输入序列的网络结构，在每一个时刻。很明显，不同于典型的多层神经网络在每个级别上使用多个参数，RNN总是使用相同的参数，以U、V和W表示(见前面的图)。此外，RNN在每个瞬间执行相同的计算，在输入的多个相同的序列上。共享相同的参数，它强烈地减少了网络在训练阶段必须学习的参数的数量，从而提高了训练的时间。

如何培养这种类型的网络，事实上，由于每个时刻的参数都是共享的，所以计算每个输出的梯度不仅取决于当前的计算，还取决于以前的计算。例如，计算时间t＝4的梯度时，有必要对时间的前三个时刻进行梯度传播，然后求出由此得到的梯度。此外，整个输入序列通常被认为是训练集的一个元素。然而,这种类型的网络的训练患有所谓的消失/梯度爆炸问题;梯度,计算和传播,往往会增加或减少在每个瞬间的时间,然后经过一定数量的瞬间的时间,发散到正无穷或收敛于零。

现在让我们检查一个RNN如何运作。Xt是即时T的网络输入，它可以是表示一个句子的单词的向量，而st则是网络的状态向量（state vector）。它可以被看作是系统的一种内存，它包含输入序列的所有以前元素的信息。瞬时t的状态向量是由当前输入(时间t)和通过U和W参数在前一时刻(时间t - 1)计算的状态进行评估:

St=f([U]Xt+[W]St-1)

函数f是一个非线性函数，如整流线性单元(rectified linear unit)(ReLu)，而Ot则是瞬时t的输出，用参数v计算，输出将取决于使用网络的问题类型。例如，如果你想预测一个句子的下一个单词，它可能是系统词汇中每个单词的概率向量。

LSTM networks(Long Short-Term Memory)长短期记忆网络

(LSTM)网络是RNN架构的基本模型的扩展。主要的想法是改进网络，为它提供一个清晰的内存。事实上，LSTM网络，尽管没有一个与RNN本质上不同的架构，但配备了特殊的隐藏单元，称为内存单元，其行为是在很长一段时间内记住以前的输入。

LSTM单元有3个门和4个输入权值，xt(从数据到输入和3个门)，而ht是单元的输出。

LSTM块包含决定输入是否足够重要以保存的门。此区块由四个单位组成:
Input gate: Allows the value input in the structure 允许在结构中输入值
Forget gate: Goes to eliminate the values contained in the structure 去消除结构中包含的值
Output gate: Determines when the unit will output the values trapped in structure  确定单元何时输出被困在结构中的值
Cell: Enables or disables the memory cell  启用或禁用内存单元格

事实证明，RNNs在预测文本中的下一个字符，或者类似地，在一个句子中预测下一个序列单词的问题上有出色的表现。然而，它们也用于更复杂的问题，如机器翻译。在这种情况下，网络将以源语言中的一系列单词作为输入，而您需要在语言目标中输出相应的单词序列。最后，RNNs被广泛应用的另一个重要应用是语音识别。接下来，我们将开发一个计算模型，它可以根据前面单词的顺序来预测文本中的下一个单词。为了测量模型的准确性，我们将使用Penn Tree Bank(PTB)数据集，这是用来测量这些模型精度的基准。

与前面的例子不同，我们将只介绍实现的过程的伪代码，以便了解模型构建背后的主要思想，而不会陷入不必要的实现细节。

该模型使用LSTM实现了RNN的架构。事实上，它计划通过包括存储单元来增加RNN的架构，这些存储单元可以保存关于长期依赖的信息。

在计算过程中，在每个单词检验状态值后，用输出值进行更新，下面是执行步骤的伪代码列表:

损失函数最小化目标词的平均负对数概率，它是TensorFow函数:tf.nn.seq2seq.sequence_loss_by_example，它计算了平均每个单词的困惑，它的值度量模型的准确性(降低值对应最佳性能)，并将在整个训练过程中被监控。

该模型实现了三种类型的配置:小的、中型的和大型的。它们之间的区别在于LSTMs的大小和用于训练的hyper参数的集合。模型越大，得到的结果就越好。
这个小模型应该能够在测试集上达到120以下的困惑，而在80以下的大型模型中，可能需要几个小时的时间来训练。






