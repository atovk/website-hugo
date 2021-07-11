---
title: "Notes on Distributed Systems for Young Bloods - 分布式系统忠告"
date: 2020-09-05T17:16:03+08:00
keywords:
- echosence
- 技术分享
- 分布式系统忠告
- 翻译
description : "Notes on Distributed Systems for Young Bloods 分布式系统忠告"
draft: false
tags: ["Distributed"]
categories: ["Distributed"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

### 分布式系统忠告

### [Notes on Distributed Systems for Young Bloods](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/ "Permalink to Notes on Distributed Systems for Young Bloods")

January 14, 2013 08:15:00 AM PST

I’ve been thinking about the lessons distributed systems engineers learn on the job. A great deal of our instruction is through scars made by mistakes made in production traffic. These scars are useful reminders, sure, but it’d be better to have more engineers with the full count of their fingers.

我一直在思考分布式系统工程师在工作中学到的教训。我们的很多指令都是通过生产过程中的错误造成的伤疤来完成的。当然，这些伤疤是有用的提醒，但最好让更多的工程师全神贯注于他们的手指。

New systems engineers will find the [Fallacies of Distributed Computing](http://en.wikipedia.org/wiki/Fallacies_of_Distributed_Computing) and the [CAP theorem](http://codahale.com/you-cant-sacrifice-partition-tolerance/) as part of their self\-education. But these are abstract pieces without the direct, actionable advice the inexperienced engineer needs to start moving[1](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/#fn:1). It’s surprising how little context new engineers are given when they start out.

新的系统工程师将发现[分布式计算的谬误](http://en.wikipedia.org/wiki/Fallacies_of_Distributed_Computing)还有[CAP定理](http://codahale.com/you-cant-safective-partition-tolerance/)作为自我教育的一部分。但这些都是抽象的部分，没有经验丰富的工程师开始行动所需的直接、可行的建议[1](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/#fn:1)。令人惊讶的是，当新工程师开始工作的时候，他们得到的信息很少。

Below is a list of some lessons I’ve learned as a distributed systems engineer that are worth being told to a new engineer. Some are subtle, and some are surprising, but none are controversial. This list is for the new distributed systems engineer to guide their thinking about the field they are taking on. It’s not comprehensive, but it’s a good beginning.

下面是我作为一名分布式系统工程师所学到的一些经验教训，值得新工程师去学习。有些是微妙的，有些是令人惊讶的，但没有一个是有争议的。这个列表是为了让新的分布式系统工程师指导他们对所从事领域的思考。这并不全面，但这是一个良好的开端。

The worst characteristic of this list is that it focuses on technical problems with little discussion of social problems an engineer may run into. Since distributed systems require more machines and more capital, their engineers tend to work with more teams and larger organizations. The social stuff is usually the hardest part of any software developer’s job, and, perhaps, especially so with distributed systems development.

这个列表最糟糕的特点是它只关注技术问题，而很少讨论工程师可能遇到的社会问题。由于分布式系统需要更多的机器和更多的资金，他们的工程师倾向于与更多的团队和更大的组织合作。社交活动通常是任何软件开发人员工作中最困难的部分，也许，在分布式系统开发中尤其如此。

Our background, education, and experience bias us towards a technical solution even when a social solution would be more efficient, and more pleasing. Let’s try to correct for that. People are less finicky than computers, even if their interface is a little less standardized.

我们的背景、教育和经验使我们倾向于技术解决方案，即使社会解决方案更有效、更令人满意。让我们试着纠正一下。人们不像计算机那么挑剔，即使他们的界面不那么标准化。

Alright, here we go.

好吧，开始吧。

\# **Distributed systems are different because they fail often.** When asked what separates distributed systems from other fields of software engineering, the new engineer often cites latency, believing that’s what makes distributed computation hard.

\#**分布式系统是不同的，因为它们经常失败。** 当被问及是什么将分布式系统与软件工程的其他领域区分开来时，这位新工程师经常引用延迟，认为这是分布式计算困难的原因。

But they’re wrong. What sets distributed systems engineering apart is the probability of failure and, worse, the probability of partial failure. If a well\-formed mutex unlock fails with an error, we can assume the process is unstable and crash it. But the failure of a distributed mutex’s unlock must be built into the lock protocol.

但他们错了。使分布式系统工程与众不同的是失败的概率，更糟糕的是部分失效的概率。如果一个格式良好的互斥锁因错误而失败，我们可以假设进程不稳定并使其崩溃。但是分布式互斥锁的解锁失败必须建立在锁协议中。

Systems engineers that haven’t worked in distributed computation will come up with ideas like “well, it’ll just send the write to both machines” or “it’ll just keep retrying the write until it succeeds”. These engineers haven’t completely accepted (though they usually intellectually recognize) that networked systems fail more than systems that exist on only a single machine and that failures tend to be partial instead of total. One of the writes may succeed while the other fails, and so now how do we get a consistent view of the data? These partial failures are much harder to reason about.

没有在分布式计算领域工作过的系统工程师会想出这样的想法：“好吧，它只会将写操作发送到两台机器上”或者“它会一直重复写入，直到成功为止”。这些工程师还没有完全接受（尽管他们通常在智力上认识到）联网系统比只存在于一台机器上的系统更容易出故障，而且故障往往是局部的而不是全部的。其中一个写操作可能成功，而另一个写操作失败，那么现在我们如何获得一致的数据视图呢？这些局部故障很难解释。

Switches go down, garbage collection pauses make leaders “disappear”, socket writes seem to succeed but have actually failed on the other machine, a slow disk drive on one machine causes a communication protocol in the whole cluster to crawl, and so on. Reading from local memory is simply more stable than reading across a few switches.

交换机宕机，垃圾收集暂停使引导程序“消失”，套接字写入看似成功，但实际上在另一台计算机上失败，一台计算机上磁盘驱动器速度慢导致整个群集中的通信协议爬行，等等。从本地内存读取比通过几个交换机读取更稳定。

Design for failure. #

为失败而设计。#

\# **Writing robust distributed systems costs more than writing robust single\-machine systems.** Creating a robust distributed solution requires more money than a single\-machine solution because there are failures that only occur with many machines. Virtual machine and cloud technology make distributed systems engineering cheaper but not as cheap as being able to design, implement, and test on a computer you already own. And there are failure conditions that are difficult to replicate on a single machine. Whether it’s because they only occur on dataset sizes much larger than can be fit on a shared machine, or in the network conditions found in datacenters, distributed systems tend to need actual, not simulated, distribution to flush out their bugs. Simulation is, of course, very useful. #

\#**编写健壮的分布式系统要比编写健壮的单机系统花费更多的钱。** 创建一个健壮的分布式解决方案需要比单机解决方案更多的钱，因为有些故障只发生在多台机器上。虚拟机和云技术使分布式系统工程变得更便宜，但不如在已经拥有的计算机上进行设计、实现和测试那样便宜。而且有些故障情况很难在一台机器上重现。无论是因为它们只出现在比共享计算机大得多的数据集上，还是在数据中心的网络条件下，分布式系统往往需要实际的而不是模拟的分布来清除它们的错误。当然，模拟是非常有用的。#

\# **Robust, open source distributed systems are much less common than robust, single\-machine systems.** The cost of running many machines for long periods of time is a burden on open source communities. Hobbyists and dilettantes are the engines of open source software and they do not have the financial resources available to explore or fix many of the problems a distributed system will have. Hobbyists write open source code for fun in their free time and with machines they already own. It’s much harder to find open source developers who are willing to spin up, maintain, and pay for a bunch of machines.

\#**健壮的、开源的分布式系统比健壮的单机系统要少得多。** 长时间运行许多机器的成本是开源社区的负担。业余爱好者和业余爱好者是开源软件的引擎，他们没有足够的财力来探索或解决分布式系统将面临的许多问题。业余爱好者在空闲时间和他们已经拥有的机器上写开源代码是为了好玩。很难找到愿意启动、维护和购买一堆机器的开源开发人员。

Some of this slack has been taken up by engineers working for corporate entities. However, the priorities of their organization may not be in line with the priorities of your organization.

这些松懈的部分工作已经被为企业实体工作的工程师所弥补。但是，他们组织的优先级可能与您组织的优先级不一致。

While some in the open source community are aware of this problem, it’s not yet solved. This is hard. #

虽然开源社区的一些人意识到了这个问题，但它还没有得到解决。这很难。#

\# **Coordination is very hard.** Avoid coordinating machines wherever possible. This is often described as “horizontal scalability”. The real trick of horizontal scalability is independence – being able to get data to machines such that communication and consensus between those machines is kept to a minimum. Every time two machines have to agree on something, the service becomes harder to implement. Information has an upper limit to the speed it can travel, and networked communication is flakier than you think, and your idea of what constitutes consensus is probably wrong. Learning about the [Two Generals](http://en.wikipedia.org/wiki/Two_Generals%27_Problem) and [Byzantine Generals](http://en.wikipedia.org/wiki/Byzantine_Generals%27_Problem) problems is useful here. (Oh, and Paxos really is [very hard to implement](http://research.google.com/pubs/pub33002.html); that’s not grumpy old engineers thinking they know better than you.) #

\#**协调非常困难。** 尽可能避免协调机器。这通常被描述为“水平可伸缩性”。横向可伸缩性的真正诀窍在于独立性——能够将数据传输到机器上，从而将这些机器之间的通信和一致性保持在最低限度。每当两台机器必须就某件事达成一致时，服务就变得更难实现。信息的传播速度有一个上限，网络通信比你想象的要脆弱，你对什么是共识的想法可能是错误的。了解[两位将军](http://en.wikipedia.org/wiki/Two_Generals%27_Problem)和[拜占庭将军](http://en.wikipedia.org/wiki/Two_Generals%27_Problem)问题在这里很有用。（哦，而且Paxos真的[很难实现](http://research.google.com/pubs/pub33002.html)；这不是那些脾气暴躁的老工程师，他们认为自己比你更懂。）#

\# **If you can fit your problem in memory, it’s probably trivial.** To a distributed systems engineer, problems that are local to one machine are easy. Figuring out how to process data quickly is harder when the data is a few switches away instead of a few pointer dereferences away. In a distributed system, the well\-worn efficiency tricks documented since the beginning of computer science no longer apply. Plenty of literature and implementations are available for algorithms that run on a single machine because the majority of computation has been done on singular, uncoordinated machines. Significantly fewer exist for distributed systems. #

\#**如果您可以将问题存储在内存中，那么它可能是微不足道的。** 对于分布式系统工程师来说，一台机器上的本地问题很容易解决。当数据间的一些指针引用被一些交换机替代时，要想快速处理数据就比较困难了。在分布式系统中，自计算机科学诞生以来记录在案的效率技巧不再适用。在一台机器上运行的算法有大量的文献和实现，因为大多数计算都是在单一的、不协调的机器上完成的。对于分布式系统，存在的数量要少得多。#

\# **“It’s slow” is the hardest problem you’ll ever debug.** “It’s slow” might mean one or more of the number of systems involved in performing a user request is slow. It might mean one or more of the parts of a pipeline of transformations across many machines is slow. “It’s slow” is hard, in part, because the problem statement doesn’t provide many clues to the location of the flaw. Partial failures, ones that don’t show up on the graphs you usually look up, are lurking in a dark corner. And, until the degradation becomes very obvious, you won’t receive as many resources (time, money, and tooling) to solve it. [Dapper](http://research.google.com/pubs/pub36356.html) and [Zipkin](http://engineering.twitter.com/2012/06/distributed-systems-tracing-with-zipkin.html) were built for a reason. #

\#**“它很慢”是您调试过的最难的问题。** “它很慢”可能意味着执行用户请求所涉及的一个或多个系统速度较慢。这可能意味着跨越许多机器的传输管道中的一个或多个部分是缓慢的。“它很慢”很难，部分原因是问题陈述没有提供很多关于缺陷位置的线索。部分失败，那些在你通常查找的图表中没有出现的故障，正潜伏在一个黑暗的角落里。而且，在退化变得非常明显之前，您不会得到足够多的资源（时间、资金和工具）来解决它。[潇洒](http://research.google.com/pubs/pub36356.html)还有[Zipkin](http://engineering.twitter.com/2012/06/distributed-systems-tracing-with-zipkin.html)是有原因的。#

\# **Implement backpressure throughout your system.** Backpressure is the signaling of failure from a serving system to the requesting system and how the requesting system handles those failures to prevent overloading itself and the serving system. Designing for backpressure means bounding resource use during times of overload and times of system failure. This is one of the basic building blocks of creating a robust distributed system.

\#**在整个系统中实施反压。** 反压是从服务系统向请求系统发出故障信号，以及请求系统如何处理这些故障，以防止自身和服务系统过载。设计反压意味着在过载和系统故障时限制资源使用。这是创建一个健壮的分布式系统的基本构件之一。

Implementations of backpressure usually involve either dropping new messages on the floor, or shipping errors back to users (and incrementing a metric in both cases) when a resource becomes limited or failures occur. Timeouts and exponential back\-offs on connections and requests to other systems are also essential.

反压的实现通常包括在资源受限或发生故障时向用户发送新消息，或者将错误发送回用户（在这两种情况下都增加一个度量）。与其他系统的连接和请求的超时和指数后退也很重要。

Without backpressure mechanisms in place, cascading failure or unintentional message loss become likely. When a system is not able to handle the failures of another, it tends to emit failures to another system that depends on it. #

如果没有反压机制，级联故障或意外的消息丢失就很可能发生。当一个系统不能处理另一个系统的故障时，它往往会向依赖它的另一个系统发出故障。#

\# **Find ways to be partially available.** Partial availability is being able to return some results even when parts of your system is failing.

Search is an ideal case to explore here. Search systems trade\-off between how good their results are and how long they will keep a user waiting. A typical search system sets a time limit on how long it will search its documents, and, if that time limit expires before all of its documents are searched, it will return whatever results it has gathered. This makes search easier to scale in the face of intermittent slowdowns, and errors because those failures are treated the same as not being able to search all of their documents. The system allows for partial results to be returned to the user and its resilience is increased.

And consider a private messaging feature in a web application. At some point, no matter what you do, enough storage machines for private messaging will be down at the same time that your users will notice. So what kind of partial failure do we want in this system?

This takes some thought. People are generally more okay with private messaging being down for them (and maybe some other users) than they are with all users having some of their messages go missing. If the service is overloaded or one of its machines are down, failing out just a small fraction of the userbase is preferable to missing data for a larger fraction. And, on top of that choice, we probably don’t want an unrelated feature, like public image upload, to be affected just because private messaging is having a problem. How much work are we willing to do to keep those failure domains separate?

Being able to recognize these kinds of trade\-offs in partial availability is good to have in your toolbox. #

\#**找到使部分可用的方法。**部分可用性可以在系统的某些部分发生故障时返回一些结果。

在这里，搜索是一个理想的探索案例。搜索系统在搜索结果的好坏和用户等待的时间之间进行权衡。一个典型的搜索系统设置了一个搜索文档的时间限制，如果这个时间限制在搜索所有文档之前过期，它将返回它收集到的任何结果。这使得搜索更容易在面对间歇性的减速和错误时进行扩展，因为这些失败被视为无法搜索所有文档。该系统允许将部分结果返回给用户，并提高其弹性。

并考虑web应用程序中的私有消息传递功能。在某个时刻，无论你做什么，足够多的存储机器将关闭，同时你的用户会注意到。那么在这个系统中我们想要什么样的局部失效呢？

这需要一些思考。一般来说，人们对自己（也许还有其他一些用户）的私人信息服务瘫痪比对所有用户的部分信息丢失更放心。如果服务超载或其中一台机器宕机，仅仅是一小部分用户群的故障比更大一部分用户的数据丢失要好。而且，在这个选择之上，我们可能不希望一个不相关的功能，比如公共图片上传，仅仅因为私人消息出现问题而受到影响。我们愿意做多少工作来保持这些故障域的分离？

\# **Metrics are the only way to get your job done.** Exposing metrics (such as latency percentiles, increasing counters on certain actions, rates of change) is the only way to cross the gap from what you believe your system does in production and what it actually is doing. Knowing how the system’s behavior on day 20 is different from its behavior on day 15 is the difference between successful engineering and failed shamanism. Of course, metrics are necessary to understand problems and behavior, but they are not sufficient to know what to do next.

A diversion into logging. Log files are good to have, but they tend to lie. For example, it’s very common for the logging of a few error classes to take up a large proportion of a space in a log file but, in actuality, occur in a very low proportion of requests. Because logging successes is redundant in most cases (and would blow out the disk in most cases) and because engineers often guess wrong on which kinds of error classes are useful to see, log files get filled up with all sorts of odd bits and bobs. Prefer logging as if someone who has not seen the code will be reading the logs.

I’ve seen a good number of outages extended by another engineer (or myself) over\-emphasizing something odd we saw in the log without first checking it against the metrics. I’ve also seen another engineer (or myself) Sherlock\-Holmes’ing an entire set of failed behaviors from a handful of log lines. But note: a) we remember those successes because they are so very rare and b) you’re not Sherlock unless the metrics or the experiments back up the story. #

\# **指标是完成你的工作的唯一方法。** 暴露指标（如延迟百分比，增加某些动作的计数器，变化率）是跨越你相信你的系统在生产中做的事情和它实际做的事情之间的差距的唯一方法。了解系统在第20天的行为与第15天的行为有何不同，是成功的工程和失败的萨满之间的区别。当然，指标对于了解问题和行为是必要的，但它们不足以知道下一步该怎么做。

转入日志记录。日志文件是好东西，但它们往往会说谎。例如，记录一些错误类的日志在日志文件中占据了很大的空间，但实际发生在请求中的比例很低，这是非常常见的。因为记录成功率在大多数情况下是多余的（而且在大多数情况下会炸掉磁盘），而且工程师经常猜错哪种错误类是有用的，所以日志文件会被各种奇奇怪怪的零碎东西填满。更喜欢把日志记录成一个没有看过代码的人在阅读日志。

我见过不少中断是由另一个工程师（或我自己）过度强调我们在日志中看到的一些奇怪的东西，而没有先对照指标检查。我也见过另一位工程师（或我自己）从少量的日志行中找出一整套失败的行为。但是请注意：a)我们记住这些成功的案例，因为它们非常罕见；b)如果没有指标或实验支持，你就不是夏洛克(大侦探)。#

\# **Use percentiles, not averages.** Percentiles (50th, 99th, 99.9th, 99.99th) are more accurate and informative than averages in the vast majority of distributed systems. Using a mean assumes that the metric under evaluation follows a bell curve but, in practice, this describes very few metrics an engineer cares about. “Average latency” is a commonly reported metric, but I’ve never once seen a distributed system whose latency followed a bell curve. If the metric doesn’t follow a bell curve, the average is meaningless and leads to incorrect decisions and understanding. Avoid the trap by talking in percentiles. Default to percentiles, and you’ll better understand how users really see your system. #

\#**使用百分位数，而不是平均值。** 在绝大多数分布式系统中，百分数（第50、99、99.9、99.99）比平均数更准确，信息量更大。使用平均值是假设被评估的度量遵循一条钟形曲线，但实际上，这描述的是工程师关心的极少数度量。"平均延迟 "是一个普遍报道的度量，但我从来没有见过一个分布式系统的延迟遵循钟形曲线。如果指标不遵循钟形曲线，那么平均数就没有意义，会导致错误的决策和理解。通过用百分数说话来避免这个陷阱。默认为百分数，你就能更好地理解用户对你的系统的真实看法。#

\# **Learn to estimate your capacity.** You’ll learn how many seconds are in a day because of this. Knowing how many machines you need to perform a task is the difference between a long\-lasting system, and one that needs to be replaced 3 months into its job. Or, worse, needs to be replaced before you finish productionizing it.

Consider tweets. How many tweet ids can you fit in memory on a common machine? Well, a typical machine at the end of 2012 has 24 GB of memory, you’ll need an overhead of 4\-5 GB for the OS, another couple, at least, to handle requests, and a tweet id is 8 bytes. This is the kind of back of the envelope calculation you’ll find yourself doing. Jeff Dean’s [Numbers Everyone Should Know](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf) slide is a good expectation\-setter. #

\# **学会估算你的产能。** 你会因此知道一天有多少秒。知道你需要多少台机器来完成一个任务，是一个长期生产系统和一个在工作三个月后就需要更换的系统之间的区别。或者，更糟糕的是，在你完成生产化之前就需要更换。

考虑一下推特。在一台普通的机器上，你能在内存中容纳多少个tweet id？好吧，2012年底一台典型的机器有24GB的内存，你需要4-5GB的开销给操作系统，另外至少需要几个，来处理请求，而一个tweet id是8个字节。这就是你会发现自己要做的那种 "信封后面的计算"。Jeff Dean的[每个人都应该知道的数字](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf) 幻灯片是一个很好的期望设定者。#

\# **Feature flags are how infrastructure is rolled out.** “Feature flags” are a common way product engineers roll out new features in a system. Feature flags are typically associated with frontend A/B testing where they are used to show a new design or feature to only some of the userbase. But they are a powerful way of replacing infrastructure as well.

Too many projects have failed because they went for the “big cutover” or a series of “big cutovers” that were then forced into rollbacks by bugs found too late. By using feature flags instead, you’ll gain confidence in your project and mitigate the costs of failure.

Suppose you’re going from a single database to a service that hides the details of a new storage solution. Using a feature flag, you can slowly ramp up writes to the new service in parallel to the writes to the old database to make sure its write path is correct and fast enough. After the write path is at 100% and backfilling into the service’s datastore is complete, you can use a separate feature flag to start reading from the service, without using the data in user responses, to check for performance problems. Another feature flag can be used to perform comparison checks on read of the data from the old system and the new one. And one final flag can be used to slowly ramp up the “real” reads from the new system.

By breaking up the deployment into multiple steps and affording yourself quick and partial reactions with feature flags, you make it easier to find bugs and performance problems as they occur during ramp up instead of at a “big bang” release time. If an issue occurs, you can just tamp the feature flag setting back down to a lower (perhaps, zero) setting immediately. Adjusting the rates lets you debug and experiment at different amounts of traffic knowing that any problem you hit isn’t a total disaster. With feature flags, you can also choose other migration strategies, like moving requests over on a per\-user basis, that provide better insight into the new system. And when your new service is still being prototyped, you can use flags at a low setting to have your new system consume fewer resources.

Now, feature flags sound like a terrible mess of conditionals to a classically trained developer or a new engineer with well\-intentioned training. And the use of feature flags means accepting that having multiple versions of infrastructure and data is a norm, not an rarity. This is a deep lesson. What works well for single\-machine systems sometimes falters in the face of distributed problems.

Feature flags are best understood as a trade\-off, trading local complexity (in the code, in one system) for global simplicity and resilience. #

\# **功能标志是基础设施推出的方式。** "功能标志 "是产品工程师在系统中推出新功能的一种常见方式。功能标志通常与前端A/B测试相关联，它们被用来只向部分用户群展示新的设计或功能。但它们也是替换基础架构的一种有力方式。

太多的项目都失败了，因为他们去做 "大切入 "或一系列的 "大切入"，然后因为发现的bug太晚而被迫回滚。通过使用功能标志来代替，你将获得对项目的信心，并减轻失败的代价。

假设你要从一个单一的数据库到一个隐藏新存储解决方案细节的服务。使用一个功能标志，你可以慢慢地加大对新服务的写入，与对旧数据库的写入并行，以确保其写入路径正确且足够快。在写入路径达到100%，并完成对服务的数据存储的回填后，您可以使用一个单独的特征标志开始从服务中读取数据，而不使用用户响应中的数据，以检查性能问题。另一个特征标志可以用来对从旧系统和新系统读取的数据进行对比检查。而最后一个标志可以用来慢慢加大新系统的 "真实 "读取量。

通过将部署分为多个步骤，并通过功能标志让自己快速和部分地做出反应，你可以更容易地在升级过程中发现错误和性能问题，而不是在 "大爆炸 "的发布时间。如果出现问题，你可以立即将功能标志设置夯实到一个较低的（也许是零）设置。调整速率可以让你在不同的流量下进行调试和实验，知道你遇到的任何问题都不会是一场灾难。有了功能标志，你还可以选择其他迁移策略，比如在每个用户的基础上移动请求，以便更好地了解新系统。当你的新服务还在原型阶段时，你可以使用低设置的标志来让你的新系统消耗更少的资源。

现在，对于一个受过经典训练的开发人员或一个受过良好培训的新工程师来说，功能标志听起来像是一团糟糕的条件。而使用功能标志意味着接受拥有多个版本的基础架构和数据是一种常态，而不是稀奇。这是一个深刻的教训。对于单台机器系统来说行之有效的东西，在面对分布式问题时，有时会摇摇欲坠。

特征标志最好被理解为一种交易，用局部的复杂性（在代码中，在一个系统中）换取全局的简单性和弹性。#

\# **Choose id spaces wisely.** The space of ids you choose for your system will shape your system.

The more ids required to get to a piece of data, the more options you have in partitioning the data. The fewer ids required to get a piece of data, the easier it is to consume your system’s output.

Consider version 1 of the Twitter API. All operations to get, create, and delete tweets were done with respect to a single numeric id for each tweet. The tweet id is a simple 64\-bit number that is not connected to any other piece of data. As the number of tweets goes up, it becomes clear that creating user tweet timelines and the timeline of other user’s subscriptions may be efficiently constructed if all of the tweets by the same user were stored on the same machine.

But the public API requires every tweet be addressable by just the tweet id. To partition tweets by user, a lookup service would have to be constructed. One that knows what user owns which tweet id. Doable, if necessary, but with a non\-trivial cost.

An alternative API could have required the user id in any tweet look up and, initially, simply used the tweet id for storage until user\-partitioned storage came online. Another alternative would have included the user id in the tweet id itself at the cost of tweet ids no longer being k\-sortable and numeric.

Watch out for what kind of information you encode in your ids, explicitly and implicitly. Clients may use the structure of your ids to de\-anonymize private data, crawl your system in unexpected ways (auto\-incrementing ids are a typical sore point), or perform a [host of other attacks](https://www.owasp.org/index.php/Top_10_2010-A4-Insecure_Direct_Object_References). #

\# **明智地选择id含义。** 你为系统选择的id含义将塑造你的系统。

获取一段数据所需的id越多，你在分割数据时的选择就越多。获取一段数据所需的id越少，你的系统的输出就越容易被消耗。

考虑一下Twitter API的第1版。所有获取、创建和删除推文的操作都是针对每条推文的一个数字id来完成的。tweet id是一个简单的64位数字，与任何其他数据无关。随着推文数量的增加，很明显，如果同一用户的所有推文都存储在同一台机器上，那么创建用户的推文时间轴和其他用户订阅的时间轴可能会被高效地构建。

但公共API要求每条推文都只需通过推文id即可寻址。为了按用户来划分tweet，必须构建一个查找服务。一个知道哪个用户拥有哪个tweet id的服务。如果有必要的话，是可以做到的，但成本不高。

一个替代的API可以在任何tweet查询中要求用户id，并且，最初，简单地使用tweet id进行存储，直到用户/分区存储上线。另一个替代方案是将用户id包含在tweet id本身中，代价是tweet id不再是k/sortable和数字的。

注意你在你的id中编码了什么样的信息，显性的和隐性的。客户端可能会使用你的id的结构去匿名化私人数据，以意想不到的方式抓取你的系统（自动递增的id是一个典型的痛点），或者执行[一系列其他攻击](https://www.owasp.org/index.php/Top_10_2010-A4-Insecure_Direct_Object_References)。#

\# **Exploit data\-locality.** The closer the processing and caching of your data is kept to its persistent storage, the more efficient your processing, and the easier it will be to keep your caching consistent and fast. Networks have more failures and more latency than pointer dereferences and `fread(3)`.

Of course, data\-locality means being nearby in space, but it also means nearby in time. If multiple users are making the same expensive request at nearly the same time, perhaps their requests can be joined into one. If multiple instances of requests for the same kind of data are made near to one another, they could be joined into one larger request. Doing so often affords lower communication overheard and easier fault management. #

\# **利用数据的位置性。** 数据的处理和缓存越接近它的持久性存储，你的处理效率就越高，也就越容易保持缓存的一致性和快速性。网络比指针取消引用和`fread(3)`有更多的故障和更多的延迟。

当然，数据/位置性意味着在空间上靠近，但也意味着在时间上靠近。如果多个用户几乎在同一时间发出同一个昂贵的请求，也许他们的请求可以合并成一个。如果对同一种数据的多个请求实例彼此靠近，那么可以将它们合并成一个更大的请求。这样做通常可以降低通信的杂音和更容易的故障管理。#

\# **Writing cached data back to persistent storage is bad.** This happens in more systems than you’d think. Especially ones originally designed by people less experienced in distributed systems. Many systems you’ll inherit will have this flaw. If the implementers talk about “Russian\-doll caching”, you have a large chance of hitting highly visible bugs. This entry could have been left out of the list, but I have a special hate in my heart for it. A common presentation of this flaw is user information (e.g. screennames, emails, and hashed passwords) mysteriously reverting to a previous value. #

\# **将缓存数据写回持久化存储是糟糕的。** 这种情况发生在比你想象的更多的系统中。尤其是最初由在分布式系统方面经验不足的人设计的。你将继承的许多系统都会有这个缺陷。如果实现者谈论的是 “俄罗斯套娃缓存"，你就有很大的机会碰到高度可见的bug。这个条目本来可以不列入列表，但我心里特别恨它。这个缺陷常见的表现形式是用户信息（如网名、邮箱和密码哈希）神秘地恢复到以前的值。#

\# **Computers can do more than you think they can.** In the field today, there’s plenty of misinformation about what a machine is capable of from practitioners that do not have a great deal of experience.

At the end of 2012, a light web server had 6 or more processors, 24 GB of memory and more disk space than you can use. A relatively complex [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) application in a modern language runtime on a single machine is trivially capable of doing thousands of requests per second within a few hundred milliseconds. And that’s a deep lower bound. In terms of operational ability, hundreds of requests per second per machine is not something to brag about in most cases.

Greater performance is not hard to come by, especially if you are willing to profile your application and introduce efficiencies based on your measurements. #

\# **计算机能做的事情比你想象的要多。** 在今天的领域里，没有太多经验的实践者对一台机器的能力有很多误解。

在2012年底，一台轻量级的Web服务器有6个以上的处理器，24GB的内存，磁盘空间比你能用的多。一个相对复杂的[CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete)应用在现代语言运行时的单机上，在几百毫秒内，每秒可以做上千个请求，这是微不足道的。而这是一个很深的下限。就运行能力而言，在大多数情况下，每台机器每秒发出数百个请求并不是什么值得吹嘘的事情。

更高的性能并不难实现，特别是如果你愿意根据你的测量结果对你的应用进行剖析并改进效率。#

\# **Use the CAP theorem to critique systems.** The CAP theorem isn’t something you can build a system out of. It’s not a theorem you can take as a first principle and derive a working system from. It’s much too general in its purview, and the space of possible solutions too broad.

However, it is well\-suited for critiquing a distributed system design, and understanding what trade\-offs need to be made. Taking a system design and iterating through the constraints CAP puts on its subsystems will leave you with a better design at the end. For homework, apply the CAP theorem’s constraints to a real world implementation of Russian\-doll caching.

One last note: Out of C, A, and P, you [can’t choose CA](http://codahale.com/you-cant-sacrifice-partition-tolerance/). #

\# **利用CAP定理来批判系统。** CAP定理不是你可以用来构建系统的的东西。它不是一个你可以把它作为第一原则并从中推导出一个工作系统的定理。它的范围过于笼统，可能的解决方案空间也太广了。

但是，它非常适合对分布式系统设计进行评判，并了解需要做出哪些权衡。将一个系统设计，通过CAP对其子系统的迭代进行约束，最终会让你得到一个更好的设计。作业家庭作业，将CAP定理的约束条件应用到现实世界的俄罗斯套娃缓存实现中。

最后说明一下：在C、A、P中，你[不能选择CA](http://codahale.com/you-cant-sacrifice-partition-tolerance/)。#

\# **Extract services.** “Service” here means “a distributed system that incorporates higher\-level logic than a storage system and typically has a request\-response style API”. Be on the lookout for code changes that would be easier to do if the code existed in a separate service instead of in your system.

An extracted service provides the benefits of encapsulation typically associated with creating libraries. However, extracting out a service improves on creating libraries by allowing for changes to be deployed faster and easier than upgrading the libraries in its client systems. (Of course, if the extracted service is hard to deploy, the client systems are the ones that become easier to deploy.) This ease is owed to the fewer code and operational dependencies in the smaller, extracted service and the strict boundary it creates makes it harder to “take shortcuts” that a library allows for. These shortcuts almost always make it harder to migrate the internals or the client systems to new versions.

The coordination costs of using a service is also much lower than a shared library when there are multiple client systems. Upgrading a library, even with no API changes needed, requires coordinating deploys of each client system. This gets harder when data corruption is possible if the deploys are performed out of order (and it’s harder to predict that it will happen). Upgrading a library also has a higher social coordination cost than deploying a service if the client systems have different maintainers. Getting others aware of and willing to upgrade is surprisingly difficult because their priorities may not align with yours.

The canonical service use case is to hide a storage layer that will be undergoing changes. The extracted service has an API that is more convenient, and reduced in surface area compared to the storage layer it fronts. By extracting a service, the client systems don’t have to know about the complexities of the slow migration to a new storage system or format and only the new service has to be evaluated for bugs that will certainly be found with the new storage layout.

There are a great deal of operational and social issues to consider when doing this. I cannot do them justice here. Another article will have to be written. #

*Much love to my reviewers [Bill de hÓra](https://twitter.com/dehora), [Coda Hale](https://twitter.com/coda), [JD Maturen](https://twitter.com/jdmaturen), [Micaela McDonald](https://twitter.com/nora), and [Ted Nyman](https://twitter.com/tnm). Your insight and care was invaluable.*

**Update** (2016\-08\-15): I’ve added permalinks for each section and cleaned up some text in the sections on coordination, data\-locality, feature flags, and backpressure.

\# **拆分服务。** "服务 "在这里指的是一个分布式系统，它包含了比存储系统更高层次的逻辑，通常有一个请求/响应式的API"。要注意代码的变化，如果代码存在于一个单独的服务中，而不是在你的系统中，就会更容易做到。

拆分出来的服务提供了通常与创建库相关的封装的好处。然而，拆分出来的服务在创建库的基础上进行了改进，允许比升级其客户端系统中的库更快、更容易地部署更改。(当然，如果拆分出来的服务很难部署，那么客户端系统才会变得更容易部署)。这种轻松归功于较小的、拆分的服务中较少的代码和操作依赖性，而且它所建立的严格边界使得库所允许的 "走捷径 "变得更加困难。这些捷径几乎总是使内部或客户端系统更难迁移到新版本。

当有多个客户端系统时，使用服务的协调成本也比共享库低很多。升级一个库，即使不需要更改API，也需要协调每个客户端系统的部署。如果不按顺序进行部署，就有可能发生数据损坏（而且更难预测会发生），这就更难了。如果客户端系统有不同的维护者，升级一个库的社会协调成本也比部署一个服务高。让其他人意识到并愿意升级是出乎意料的困难，因为他们的优先级可能与你的不一致。

规范的服务用例是隐藏一个将进行更改的存储层。拆分的服务有一个更方便的API，与它前面的存储层相比，表面积减少了。通过拆分服务，客户系统不必知道缓慢迁移到新的存储系统或格式的复杂情况，只需要评估新的服务是否有bug，而新的存储布局一定会有bug。

在做这件事的时候，有大量的运营和社会问题需要考虑。我在这里无法对它们进行公正的评价。还得再写一篇文章。#

*非常感谢我的审稿人[Bill de hÓra](https://twitter.com/dehora)、[Coda Hale](https://twitter.com/coda)、[JD Maturen](https://twitter.com/jdmaturen)、[Micaela McDonald](https://twitter.com/nora)和[Ted Nyman](https://twitter.com/tnm)。您的洞察力和关怀是无价的*。

**更新**（2016/08/15）。我为每一节添加了链接，并清理了关于协调、数据/地点性、特征标志和反压的部分文字。

---

1.  Of course, Rotem\-Gal\-Oz’s [take on the fallacies](http://www.rgoarchitects.com/Files/fallacies.pdf) is very good. [↩︎](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/#fnref:1)
