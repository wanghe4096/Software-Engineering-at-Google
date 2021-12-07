# Testing Overview
# 测试概述
** Written by Adam Bender **
** Edited by Tom Manshreck**


Testing has always been a part of programming. In fact, the first time you wrote a computer program you almost certainly threw some sample data at it to see whether it performed as you expected. For a long time, the state of the art in software testing resembled a very similar process, largely manual and error prone. However, since the early 2000s, the software industry’s approach to testing has evolved dramatically to cope with the size and complexity of modern software systems. Central to that evolution has been the practice of developer-driven, automated testing.

测试一直是编程的一部分。事实上，当我们第一次编写计算机程序时，我们总是可以给出一些样本数据，以此观察程序的运行是否符合预期。在相当长的一段时间里，软件测试的技术水平和这一过程非常相似，大量的手动操作且容易出现错误。然而，自21世纪初以来，软件行业的测试技术已经有了巨大的发展，来应对现代软件系统的规模化和复杂性。这种发展的核心是以开发者驱动的自动化测试。


Automated testing can prevent bugs from escaping into the wild and affecting your users. The later in the development cycle a bug is caught, the more expensive it is; exponentially so in many cases.[1] However, “catching bugs” is only part of the motiva‐tion. An equally important reason why you want to test your software is to support the ability to change. Whether you’re adding new features, doing a refactoring focused on code health, or undertaking a larger redesign, automated testing can quickly catch mistakes, and this makes it possible to change software with confidence.

自动化测试可以防止bug逃逸，而影响你的用户。在软件开发周期发现BUG越晚，BUG被发现的成本越高；在许多情况下，这种成本是成指数倍增长的。 然而，“捉bug” 只是这一动机的一部分。 同样重要的原因是，你想测试你的软件是为了支持某种变化的能力。无论你是在添加新的功能，还是聚集在代码健壮性的重构上，或者进行更大规模的重新设计，自动化测试都可以快速地捕捉错误，这使得你可以放心的修改软件。 



Companies that can iterate faster can adapt more rapidly to changing technologies, market conditions, and customer tastes. If you have a robust testing practice, you needn’t fear change—you can embrace it as an essential quality of developing soft‐ ware. The more and faster you want to change your systems, the more you need a fast way to test them.
能够快速迭代的公司可以更迅速地适应不断变化的技术、市场条件和客户的偏好。如果你有一个强大的测试实践，你就不需要害怕变化--你可以把它作为开发软件的一个基本质量来接受。你越想改变你的系统，你就越需要一种快速的方法来测试它们。

The act of writing tests also improves the design of your systems. As the first clients of your code, a test can tell you much about your design choices. Is your system too tightly coupled to a database? Does the API support the required use cases? Does your system handle all of the edge cases? Writing automated tests forces you to con‐ front these issues early on in the development cycle. Doing so generally leads to more modular software that enables greater flexibility later on.
编写测试的行为也提在改善系统的设计。作为你的代码的第一个客户，测试可以告诉你很多关于设计选择。你的系统是否与数据库耦合得太紧了？API是否支持所需的用例？你的系统是否能处理所有的边界情况？编写自动化测试迫使你在开发的早期就解决这些问题。这样做通常会使更多的模块化软件，以便之后有更大的灵活性。


Much ink has been spilled about the subject of testing software, and for good reason: for such an important practice, doing it well still seems to be a mysterious craft to many. At Google, while we have come a long way, we still face difficult problems get‐ ting our processes to scale reliably across the company. In this chapter, we’ll share what we have learned to help further the conversation.

关于测试软件的问题，已经有很多人在讨论，这也是有原因的：对于这样一个重要的实践，做好它对很多人来说似乎仍然是一个神秘的技术。在谷歌，虽然我们已经走过了很长的路，但我们仍然面临着困难的问题，即如何使我们的流程在整个公司可靠地扩展。在这一章中，我们将分享我们所学到的东西，以便进一步的讨论。


## Why Do We Write Tests?
To better understand how to get the most out of testing, let’s start from the beginning. When we talk about automated testing, what are we really talking about?
The simplest test is defined by:
- A single behavior you are testing, usually a method or API that you are calling
- A specific input, some value that you pass to the API
- An observable output or behavior
- A controlled environment such as a single isolated process


## 我们为什么要写测试？
为了更好地理解如何从测试中获得最大收益，让我们从头开始。当我们谈论自动化测试时，我们真正谈论的是什么？
最简单的测试是这样定义的:
- 一个你要测试的单一行为，通常是一个你要调用的方法或API
- 一个特定的输入，你传递给API的一些值
- 一个可观察的输出或行为
- 一个受控的环境，如一个单独的隔离过程


When you execute a test like this, passing the input to the system and verifying the output, you will learn whether the system behaves as you expect. Taken in aggregate, hundreds or thousands of simple tests (usually called a test suite) can tell you how well your entire product conforms to its intended design and, more important, when it doesn’t.

Creating and maintaining a healthy test suite takes real effort. As a codebase grows, so too will the test suite. It will begin to face challenges like instability and slowness. A failure to address these problems will cripple a test suite. Keep in mind that tests derive their value from the trust engineers place in them. If testing becomes a pro‐ ductivity sink, constantly inducing toil and uncertainty, engineers will lose trust and begin to find workarounds. A bad test suite can be worse than no test suite at all.

当你执行这样的测试，把输入传给系统并验证输出时，你将了解到系统的行为是否符合你的期望。总的来说，成百上千的简单测试（通常称为测试套件）可以告诉你整个产品在多大程度上符合其预期的设计，更重要的是，当它不符合时。
创建和维护一个健康的测试套件需要真正的努力。随着代码库的增长，测试套件也会增长。它将开始面临诸如不稳定和缓慢的挑战。如果不能解决这些问题，测试套件将被削弱。请记住，测试的价值来自工程师对它们的信任。如果测试让生产力下降，并不断地带繁重和不确定性，工程师将失去信任并开始寻找变通。一个糟糕的测试套件可能比没有测试套件更糟糕。

In addition to empowering companies to build great products quickly, testing is becoming critical to ensuring the safety of important products and services in our lives. Software is more involved in our lives than ever before, and defects can cause more than a little annoyance: they can cost massive amounts of money, loss of prop‐ erty, or, worst of all, loss of life.[2]
At Google, we have determined that testing cannot be an afterthought. Focusing on quality and testing is part of how we do our jobs. We have learned, sometimes painfully, that failing to build quality into our products and services inevitably leads to bad outcomes. As a result, we have built testing into the heart of our engineering culture.



除了使公司能够快速构建出色的产品之外，测试对于确保我们生活中重要产品和服务的安全也变得至关重要。软件比以往任何时候都更多地涉及我们的生活，缺陷可能会引起更多的烦恼：它们可能会花费大量金钱、财产损失，或者最糟糕的是，造成生命损失。 2
在 Google，我们已经确定测试不能是事后的想法。专注于质量和测试是我们工作方式的一部分。我们有时痛苦地了解到，未能将质量融入我们的产品和服务中，不可避免地会导致糟糕的结果。因此，我们将测试融入了我们工程文化的核心。

# The Story of Google Web Server
# Google Web 服务器故事


In Google’s early days, engineer-driven testing was often assumed to be of little importance. Teams regularly relied on smart people to get the software right. A few systems ran large integration tests, but mostly it was the Wild West. One product in particular seemed to suffer the worst: it was called the Google Web Server, also known as GWS.
在谷歌的早期，工程师驱动的测试往往被认为是不重要的。团队经常依靠聪明人来使软件正确。仅有几个系统进行了大规模的集成测试，但大多数情况下都是蛮荒的西部世界。有一个产品似乎受到了最严重的影响：它被称为谷歌网络服务器，也被称为GWS。

GWS is the web server responsible for serving Google Search queries and is as important to Google Search as air traffic control is to an airport. Back in 2005, as the project swelled in size and complexity, productivity had slowed dramatically. Releases were becoming buggier, and it was taking longer and longer to push them out. Team mem‐ bers had little confidence when making changes to the service, and often found out something was wrong only when features stopped working in production. (At one point, more than 80% of production pushes contained user-affecting bugs that had to be rolled back.)
GWS是负责为谷歌搜索查询提供服务的网络服务器，它对谷歌搜索的重要性就像空中交通管制对机场的重要性一样。早在2005年，随着项目的规模和复杂性的增加，生产力已经急剧下降。发布的版本bug越来越多，而且需要越来越长的时间才能推出来。团队成员在对服务进行修改时信心不足，往往在功能不能正常运行时才发现有问题。(有一次，超过80%的生产推送包含了影响用户的错误，不得不回滚）。

To address these problems, the tech lead (TL) of GWS decided to institute a policy of engineer-driven, automated testing. As part of this policy, all new code changes were required to include tests, and those tests would be run continuously. Within a year of instituting this policy, the number of emergency pushes dropped by half. This drop occurred despite the fact that the project was seeing a record number of new changes every quarter. Even in the face of unprecedented growth and change, testing brought renewed productivity and confidence to one of the most critical projects at Google. Today, GWS has tens of thousands of tests, and releases almost every day with rela‐ tively few customer-visible failures.
为了解决这些问题，GWS的技术负责人（TL）决定制定一项由工程师驱动的自动化测试策略。作为这个策略的一部分，所有新的代码变更都被要求进行测试，而且这些测试将被持续运行。在实行这一政策的一年内，紧急push的数量减少了一半。尽管该项目每个季度的变更记录都在增长，但是紧急push的数量却减少了。即使面对前所未有的增长和变化，测试也给谷歌最关键的项目之一带来了新的生产力和信心。今天，GWS有数以万计的测试，几乎每天都在发布，很少有客户可见性的失败。


The changes in GWS marked a watershed for testing culture at Google as teams in other parts of the company saw the benefits of testing and moved to adopt similar tactics.

GWS的改变标志着谷歌测试文化的一个分水岭，因为公司其他部门的团队看到了测试的好处，并采取了类似的策略。

One of the key insights the GWS experience taught us was that you can’t rely on programmer ability alone to avoid product defects. Even if each engineer writes only the occasional bug, after you have enough people working on the same project, you will be swamped by the ever-growing list of defects. Imagine a hypothetical 100-person team whose engineers are so good that they each write only a single bug a month. Collectively, this group of amazing engineers still produces five new bugs every work‐ day. Worse yet, in a complex system, fixing one bug can often cause another, as engineers adapt to known bugs and code around them.
GWS的经验告诉我们的一个重要启示是，你不能仅仅依靠程序员的能力来避免产品缺陷。即使每个工程师只是偶尔写出一些缺陷，当你有足够多的人在同一个项目上工作时，你就会被不断增长的缺陷清单所淹没。想象一下，一个假设的100人的团队，其工程师非常优秀，他们每个人每月只写一个bug。而这群了不起的工程师在每个工作晶内仍然会产生五个新的缺陷。更糟糕的是，在一个复杂的系统中，修复一个BUG往往会引起另一个BUG，因为工程师们会适应已知的错误并围绕它们编写代码。


The best teams find ways to turn the collective wisdom of its members into a benefit for the entire team. That is exactly what automated testing does. After an engineer on the team writes a test, it is added to the pool of common resources available to others. Everyone else on the team can now run the test and will benefit when it detects an issue. Contrast this with an approach based on debugging, wherein each time a bug occurs, an engineer must pay the cost of digging into it with a debugger. The cost in engineering resources is night and day and was the fundamental reason GWS was able to turn its fortunes around.

最好的团队会想办法将其成员的集体智慧转化为整个团队的利益。这正是自动化测试的作用。在团队中的一个工程师写了一个测试后，它被添加到其他人可用的公共资源池中。团队中的每个人现在都可以运行这个测试，当它检测到一个问题时，就会受益。这与基于调试的方法形成鲜明对比，在这种方法中，每次出现错误，工程师都必须付出代价，用调试器去挖掘它。工程资源的成本是夜以继日的，是GWS能够扭转其命运的根本原因。

# Testing at the Speed of Modern Development
Software systems are growing larger and ever more complex. A typical application or service at Google is made up of thousands or millions of lines of code. It uses hundreds of libraries or frameworks and must be delivered via unreliable networks to an increasing number of platforms running with an uncountable number of configurations. To make matters worse, new versions are pushed to users frequently, sometimes multiple times each day. This is a far cry from the world of shrink-wrapped software that saw updates only once or twice a year.

软件系统越来越大，越来越复杂。谷歌的一个典型的应用程序或服务是由数千或数百万行代码组成的。它使用了数百个库或框架，并且必须通过不可靠的网络交付到越来越多的平台上，这些平台上运行的配置多得难以计数。更糟糕的是，新的版本被频繁地推送给用户，有时是每天多次。这与一年只更新一两次的压缩软件世界相去甚远。

The ability for humans to manually validate every behavior in a system has been unable to keep pace with the explosion of features and platforms in most software. Imagine what it would take to manually test all of the functionality of Google Search, like finding flights, movie times, relevant images, and of course web search results (see Figure 11-1). Even if you can determine how to solve that problem, you then need to multiply that workload by every language, country, and device Google Search must support, and don’t forget to check for things like accessibility and security. Attempting to assess product quality by asking humans to manually interact with every feature just doesn’t scale. When it comes to testing, there is one clear answer: automation.

人类手动验证系统中每一个行为的能力已经无法跟上大多数软件中功能和平台爆炸的步伐。想象一下，要手动测试谷歌搜索的所有功能，比如寻找航班、电影时间、相关图片，当然还有网页搜索结果，需要多少时间（见图11-1）。即使你能确定如何解决这个问题，你也需要把这个工作量乘以谷歌搜索必须支持的每一种语言、国家和设备，而且别忘了检查可访问性和安全性等。试图通过要求人类与每个功能进行手动互动来评估产品质量是不可能的。当涉及到测试时，有一个明确的答案：自动化。

![Figure 11-1. Screenshots of two complex Google search results](../../images/chapter-11/figure 11-1.png)


# Write, Run, React
# 编写，运行，响应

     