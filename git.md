## 1 Trunk Based

Trunk Based只有一个master主干，每个人都在本地写新代码，达到可提交程度的时候，就往master合并。如下图：
关键点：

1. 这种模式在本地和 master 之间不存在一个缓冲的区域，所以从本地 commit 到 master 时，需要确保经过了本地测试可用。

2. 过程足够简单，节省项目一致性成本。

3. 对项目或产品计划要求不高。有基本的WBS，任务计划，里程碑计划就行了。而且由于计划复杂度要求不高，修改计划的成本也就比较低。这点最本质的说法，就是产品只有一个 release线。

4. ABC三个模块同时开发时，一旦决定只上线AB模块，由于都在 master，线上版本其实也包含C模块的部分。当然可以也有办法去回避这个问题，比如 Feature Toggle，功能开关。
最佳实践：

1. 对项目和定义清晰的产品非常友好。不适合走 IPD 过程而形成的 VRM 产品线IPD和VRM。不太适合探索型产品，原因是探索型产品可能需要做各种Feature，最终有些留下有些不留，在TrunkBased模式下，没有Feature分支，剔除Feature不太方便。

  2. 对持续集成，每日构建，每日冒烟非常友好。

  3. 为了减少本地代码到 master 之间的时差，发挥持续集成和每日构建的价值，在项目计划的WBS 时，任务颗粒应该尽量小一些。因为颗粒越小，自测越迅速，提交越快，冲突越小。我们通常以人时为单位做 WBS，原则上不允许一个任务超过8人时。通常在2到6左右。
## 2 Git Flow

Git Flow 在本地和 master 之间引入多个分支，以做缓冲，集成和测试，保证 master 尽量干净。在这个方案下，分为以下几条分支：Master主分支，Release发布分支，Develop集成分支，Feature开发分支，Hot Fixes线上bug分支。由于这个方案的复杂度较高，受到了很多批判，主要的点是说 long lived branch considered harmful 。

主要过程如下：

2.1 初始环境。在 master 上打tag-0.1。

2.2 初始环境。从 master 拉出 develop 分支。

2.3 开发新功能。在 develop 分支上拉出 feature 分支，可能会有多个。

2.4 提交新功能。将完成单元测试的 feature 合并回 develop。

2.5 集成测试。在 develop 上可能存在多个 feature 集成测试的 bug，直接在 develop  上改。

2.6 准备发布。当develop达到发布计划的要求时，就从develop提交到release。

2.7 发布测试。在release上的bug，直接在release上改，改完merge回develop。

2.8 部署上线。从release提交到master，并打上新tag-0.2，并从master部署上线。

2.9 线上bug。如果不是紧急bug，还是可以按常规feature去做。如果是紧急bug，就从master拉出hotfixes，在hotfixes改和测，然后提交到master，merge到develop。
关键点：

1. 引入了 Feature 分支。新特性完全在分支上开发，避免了对 master 的污染，并且多个新特性同时开发，不会相互影响。

2. Feature 分支的长期存在，带来很多麻烦。Feature 的颗粒，需要反复考虑。如果颗粒较大，比如到模块级，将导致 Feature 分支长期存在，带来的问题就是合并时的大量冲突，以及无法在 feature 完全完成前尽早集成。如果颗粒较小，又无法做到不污染主要分支。

3. 对项目或产品计划要求较高 。产品计划里要做好各个 Feature 的编号和管理，这样才能更好的管理 develop 和 release 分支，使发布内容和过程可视度更高。比如有时 Feature 即使做完了也暂时不提交到 Develop。

4. 持续集成的思路，在这个方案下是无效的。

5. 需要团队的每个人，都理解每一个提交点合并点的意义。应该从谁提交到谁，从谁 merge 到谁。而团队的能力总是分层的，总会有人不太理解，这时就会造成麻烦。
最佳实践：

1. 适合比较复杂的产品线。

2. 需要投入相对较大的成本维护各分支。也许需要有专人维护。

3. 需要配合维护比较完善的产品发布计划。

总之，我觉得如果产品线复杂度比较高，也愿意花成本维护一套比较重的版本方案，Git Flow 还是比较规范的。
## 3 GitHub Flow

GitHub Flow 是对 Git Flow 的简化：摒弃了纷繁复杂的多条分支，只保留 Master 和 Feature 。并且建议 Feature 的颗粒在一个用户故事上，故事完成时，就往 Master 合并。其实就是在解决long lived branch 引发的问题。过程如下：
## 4 Aone Flow

Aone Flow 采取了另外一个思路。只存在一个 Master 分支，当要开发时，就拉出新的 Feature 分支，可以同时存在多个 Feature，当达到发布计划时，就把需要合并的多条 Feature 分支合并起来，通过后再往 Master 上合并，并且tag下来。

作者：Seymoure
链接：https://www.jianshu.com/p/01301c0d8c1e
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
