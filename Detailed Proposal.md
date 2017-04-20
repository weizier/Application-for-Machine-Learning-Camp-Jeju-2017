## Generative Adversarial Nets(GAN)

In 2014, Ian Goodfellow proposed [Generative Adversarial Nets](http://1dd12277.wiz03.com/share/s/0tQi9T1pJQrz2JgSES0P5-fO3QqqXq1AfQ6Q2EDgaz1cfgI4), and it has been proved to be very effective in Computer Vision, but it is not the same in Natural Language Processing(NLP). One example is [TextGAN](http://people.duke.edu/~yz196/pdf/textgan.pdf). The main problem in NLP is the discretness.

## GAN for NLP via Reinforcement Learning(RL)
However, there are a few works on GAN for NLP, in which RL is used to solve the discretness problem in NLP. There are some awesome papers as following:
- [SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient](https://arxiv.org/abs/1609.05473 "SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient")
- [Adversarial Learning for Neural Dialogue Generation](https://arxiv.org/pdf/1701.06547.pdf "Adversarial Learning for Neural Dialogue Generation")


## My Idea

Inspired by the work in [Dual Learning for Machine Translation](https://arxiv.org/abs/1611.00179 "Dual Learning for Machine Translation")
 and [Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks](https://arxiv.org/abs/1703.10593 "Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks"), the dialog system can also use the framework in them.

 In detail, suppose that there are two agents in the dialog system, one is called **A** and the other one is called **B**. At the beginning, **A** sends a message $S_A$ to **B**, and as a response, **B** sends a message $S_B$ back to **A**, then the process is repeated until the dialog is over.

### Pretrain

I will choose RNN(precisely LSTM) as the basic unit in my dialog system. Suppose that the training data consists of messages $\{S_A\}$ from agent **A** and messages  $\{S_B\}$ from agent **B**, so I can use [SeqGAN](https://arxiv.org/abs/1609.05473 "SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient") to pretrain the TWO basic models respectively with one of them for **A** and the other one for **B**, which is pictured as following.

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/274100-96b2601aed73883d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### Dialog System

Note that pretraining just uses only one side dataset in the dialog system, it can learn the inner habits who talks with others, such as the syntactic and sentiment style, but we need more information from the other side.

If there is $T$ cycles when two agents talk with each other, and cell memory and hidden state of LSTM are notated as $s$ and $h$ respectively. Then, the flow chart is:

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/274100-1bf74fe858499a7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

In the above image, the green straight arrow is the output sentence of agent **B** and also the input of agent **A** simultaneously. The red curve is in a similar way.

So, how to update the parameter of the model during the training process? For both of the two agents, we can use Policy Gradient method. The reward comes from two main aspects one of which is the language model score(such as BLEU, CIDERr and so on), and the other aspect's intuition is how to make the current sentence more responsive.

There are much more details in math. In conclusion:
- The basic model unit is RNN;
- The framework consists mainly of [SeqGAN](https://arxiv.org/abs/1609.05473 "SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient") and [dual learning](https://arxiv.org/abs/1611.00179 "Dual Learning for Machine Translation");
- The method to update parameter is reinforcement learning.