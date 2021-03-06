# POA Network技术分享

## 摘要

本次内容由王鲁明同学准备并分享，由龚劼同学进行整理复盘。

## 背景

### 两个问题

在现有的区块链中，有两个问题是无法忽视的：1. **区块链的性能**；2. **跨链通信问题**。

其中关于性能的问题已经是老生常谈了，在这个问题上，以太坊提出了两种解决方案，一个是plasma，一个是分片技术；

而跨链通信的问题，如果解决不了，区块链就还是像局域网一样，使得信息无法互通。而有一些项目在尝试解决这个问题，比如Polkadot、Consmos network。

### POA的项目愿景

POA这个项目明确指出上面的解决方案都需要一些时间，在他们落地之前，以太坊生态存在一个空白期。那POA这个项目不是一个要解决不可能三角问题的项目，而是从现有技术中寻找一个中间路线，并非追求技术方案的完美，而是追求一个可行性。力争成为一个对以太生态比较健康的补充。

这个项目有两个关键技术的核心：1. **公证人制度**；2. **跨链桥接**。

## 公证人证明

### 背景

POA共识机制并非一个去中心化的方案，其中的每个节点都是预选出来的，即一些有社区威望的人作为节点。该项目的技术主要借鉴的是parity，而parity本身就支持了POA共识算法。

POA这样的共识机制可以完美应用在私链上，这是因为私链中被选中的节点可以受到完全的信任。当然，类似的结论在Parity官网中也有声明。另外，现有的Kovan（以太坊的一个测试网络）就是用的这种模式。

### 具体流程

1. 选择一个真实世界的主体，作为公证人；
2. 每个公证人需要运行Parity的软件并创建一个账户；
3. 将公证人的信息写在区块链的一个智能合约之上。

当然，每个公证人的硬件设施都需要受到检测，以保证其硬件达到一定的标准。

由以上内容可知，该项目在技术上的创新型并不多，是Parity网络的一个实现，目的是将这个技术商业化，解决法律、务实的一些问题。由于挑选出的公证人须在美国有一定声望，且要与项目方签订合同，所以这些公证人若有作恶的行为，需要为此承担一定的法律责任。

## 跨链桥接

这个项目的跨链桥接，同样是照抄了Parity的一个开源的项目，叫Paritybridge。它是适用于以太主网和其他与以太兼容的主网，通过公证人制度来实现兼容主网之间的交通。

### 以图为例
![](https://i.imgur.com/o2zbB1w.jpg)
![](https://i.imgur.com/Q5BZ3ap.jpg)

如上图所示，我们将Mainnet看做以太主网，Testnet看作POA网络，将代币从他们之间来回转移是一个简单的过程：

1. 在以太主网上将代币打到一个智能合约中，智能合约发出一个事件。
2. 中间部分的桥接点侦听到这个事件，会自动调用POA上的另一个合约，然后将代币转出来到POA对应的账户上。


当我们将代币从POA上提到以太主网上，流程也是一致的。

这里需要考虑中间的桥接点是如何实施的。如果我们假设桥接点是百分百可信的，那么我们可以做如下的实践：一台电脑上运行两个节点，一个是ETH节点，一个POA节点。写一个小程序，监听在以太主网上的事件，事件发生后，调用POA主网上的智能合约，然后实现代币的转移。

### 问题

1. 满足不了原子交易。如果以太主网上的智能合约实施后，一定要保证在POA主网上的合约也能执行。若桥接点是百分百可信，则可以写一个日记，监控各种智能合约的执行，若执行失败，则再调用一次。

2. 中心化的桥接点。有些项目在追求一个去中心化的桥接点，而POA是采取了一个中间路线，他的桥接点是一个公证人。猜测是在POA主网上集成了以太坊的中序节点。这样在桥接点之上，不需要同时运行以太节点和POA节点。POA利用桥接技术已经实现了一个功能：代币的发行。即可以把以太打到以太的智能合约之上，然后代币通过POA主网发到各投币人的手上。

## 总结

这个项目的本身技术可能不算突出，但是对于以太以后的生态建设具有一定的指导性意义。

大家都知道以太坊受到EOS和其他公链的挑战，主要是攻击他的速度，这个速度问题迟迟不能解决，主要是因为不可能三角。然而计算机里有一句名言，通过增加不同的层，所有的问题都可以解决：硬盘速度有问题，那么就提出了缓存、内存，以解决硬盘速度的瓶颈。同样的方法也用在了CPU之上，CPU的速度太快，那就在CPU之上也加了一些缓存。那么在以太主网短期时间内解决不了速度问题时，可不可以通过类似的方案，在以太主网上加上一些缓存的网络来解决效率问题呢？这时就出现了POA和LOOM这样的项目。

通过对这两个项目的理解感觉到了以太坊对于EOS的回应：一个以太的主网，他最重要的特征还是他的分布性和安全性，还有一些其他的与以太主网兼容的其他网络。这些兼容的主网可以采用一些其他的共识机制。根据商业场景的不同，可以放弃一些特征，突出一些特征，比如poa采用的这种公证人制度，loom采用的dpos这种共识制度。若我在以太主网上需要一个计算密集型的应用，则可以转到一个兼容的网络上，执行完毕之后再转回到以太主网。

这样就会构成一个全新的生态，有各个不同想法的人，都可以实施他自己兼容的主链和以太互接，从用户的角度来说，也可以有自己不同的选择。所以未来以太的生态，就是以以太为龙头，有不同共识机制的网络组成一个区块链网络集群，这些集群之间和以太主网之间可以自由地调动智能合约做代币的转移。

即以太主网本身解决不了的问题，可以通过以太生态去解决，这样生命力会更加顽强，不会被EOS这样的公链所打败。

举例：想象成城市的交通系统，公路的承载是有限的（以太主网），解决方案加高架桥（侧链），或者发展其他的轨道交通，只要他们是互通的，这样就可以通过交通系统来解决交通拥堵的问题。



