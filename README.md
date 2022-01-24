# SystemDesign
Source @educative

## System Design Basics
- System Design Basics
- Key Characteristics of Distributed Systems
- Load Balancing
- Caching
- Data Partitioning
- Indexes
- Proxies
- Redundancy and Replication
- SQL vs. NoSQL
- Long-Polling vs WebSockets vs Server-Sent Events

## System Design Problems
- System Design Interviews: A step by step guide
- Designing a URL Shortening service like TinyURL
- Designing Pastebin
- Designing Instagram
- Designing Dropbox
- Designing Facebook Messenger
- Designing Twitter




# 第一课
## 系统设计过程中的几大步骤：

* Step 1: 需求澄清（Requirments Clarification）
* Step 3:粗略估算 (Back-of-the-envelope Estimation )
* Step 4:系统接口定义 （System Interface Definition）
*Step 4:定义数据模型 （Defining Data Model）
* Step 5:高级定义 (High Level Design)
* Step 6:细节设计（Detailed Design）
* Step 7:识别并解决瓶颈（Indentitying and Resovling Bottlenecks）

### 第一步：需求澄清 Requirements clarifications
首先要清楚要解决的问题以及问题的范围什么？

系统设计问题往往是开放性问题，它没有唯一的答案，对于求职者来说，在需求澄清这一步的表现对面试的结果是关键的。因此我们必须明确，要关注的重点在哪里。
举一个实际的例子，假设要求设计一个类似推特的系统。在进行下一步之前，我们需要问出以下问题：

这个服务的用户能发推特以及关注别人吗？

Will users of our service be able to post tweets and follow other people?

我们应该设计和展示用户的时间线吗？

Should we also design to create and display the user’s timeline?

推特包含图片和视频吗？

Will tweets contain photos and videos?

只设计后端？还是前端后端都需要设计？

Are we focusing on the backend only, or are we developing the front-end too?

用户能搜索推特吗？

Will users be able to search tweets?

需要展示热点话题吗？

Do we need to display hot trending topics?

如果有新推特，需要弹出消息吗？

Will there be any push notification for new (or important) tweets?

诸如此类的问题，将帮助我们定义出一个合适的系统。

### 第二步：粗略估算 Back-of-the-envelope estimation

即将设计系统时，先估算一下它的规模。这将有助于我们之后关注scaling, partitioning, load balancing & caching.

系统的预期规模是多大？（比如新推特的数量，推特的浏览量，每秒产生的时间线数量，等等）

What scale is expected from the system (e.g., number of new tweets, number of tweet views, number of timeline generations per sec., etc.)?

系统需要多少内存？如果用户可以推图片和视频，那需要的内存就完全不一样了。

How much storage will we need? We will have different storage requirements if users can have photos and videos in their tweets.

系统需要多大的带宽？这对于我们处理网络拥挤和服务器负载来说非常关键。

What network bandwidth usage are we expecting? This will be crucial in deciding how we will manage traffic and balance load between servers.

### 第三步：系统接口定义 System interface definition
需要定义这个系统用得着的API。这些API可以精确体现出我们希望这个系统能做什么（establish the exact contract expected from the system)，确保我们在设计时对需求的理解没有出现错误。

还是用这个推特系统来举例。 一些可能的API如下：

postTweet(user_id, tweet_data, user_location, tweet_location, timeline, …);

generateTimeline(user_id, current_time, …);

followPeople(user_id, followee_id, …);

markFavorateTweet(user_id, tweet_id, timeStamp, …);

### 第四步：定义数据模型 Defining data model
尽早定义数据模型可以理顺系统中的数据是如何在各个模块中流动的。这会指导之后的数据分区和管理设计。面试者应该能识别设计出系统所需要的各个实体，以及它们之间应该怎么互动，还有诸如数据管理（存储，分区，加密）等方面的内容。

#### 以这个类似推特的系统举例：

User: UserID, Name, Email, DoB, CreationData, LastLogin, ect.

Tweet: TweetID, Content, TweetLocation, NumberOfLikes, TimeStamp, ect.

UserFollowo: UserID1, UserID2

FavoriteTweet: UserID, TweetID, TimeStamp

基于数据模型，我们应该选择合适的数据系统。该用NoSQL像是 Cassandra ，还是MySQL呢？为了存储图片和视频，我们应该选择什么块存储方式呢？

### 第五步：高级设计 High-level design
画出系统核心模块的类图。我们需要识别出端到端的各个模块。

对于推特举例来说，我们需要不同的应用服务来完成读/写请求和负载均衡等。如果我们能假设将有更多的读拥堵（相较于写），我们就可以决定在不同的场景中使用不同的服务器。我们需要高效的数据库来存储所有的推特和支持超大量的读要求，还需要一个分布式存储系统来保存图像和视频。

### 第六步：细节设计 Detailed design
深入挖掘两到三个模块，由面试官的反馈来引导我们来决定哪些个模块需要更多更深入的讨论。我们应该有能力提出不同的方案，并且罗列它们的优势和劣势，并解释为什么我们选择其中的某一种，而不是另一种。

记住，这里没有唯一的答案。重要的是，在系统现有限制的情况下，对不同的方案进行tradeoff选择。

你需要对以下问题有所考虑：

由于我们要存储大量的数据，那么我们如何将这些数据分区到不同的数据库呢？我们应该把同一个用户的所有数据都放到一个数据库吗？这样做会造成什么样的问题呢？
如何处理活跃用户呢？比如大量发推特的用户，或者大量关注别人的用户。
既然用户的时间线是包含最近发的推特，那么我们在存储数据的时候要考虑到如何优化最近推特的浏览吗？
我们在引入缓存来加速的时候，需要引入到什么程度呢，在哪个层面引入呢？ 什么模块需要更好的负载均衡呢？
### 第七步：识别和解决瓶颈 Identifying and resolving bottlenecks
尽可能多地找到瓶颈（存在的或者潜在的），以及尽可能多的提出不同的解决方案。

系统中有可能失效的某个点呢？我们如何去消除它？
系统对数据的备份足够吗？如果某个数据库或者服务器失效了，我们如何避免对用户的影响？
我们是否有足够的服务备份，当某些服务失效的时候，不会导致整个系统的崩溃？
如何监控系统的服务表现？当关键模块失效或者性能大幅下降的时候，系统是否有警示？
