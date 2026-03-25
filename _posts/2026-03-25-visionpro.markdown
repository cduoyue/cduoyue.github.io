---
layout: post
title:  "Vision Pro"
date:   2026-03-25 00:00:00 +0000
tags: [visionOS Design]
author: Neoren
excerpt: "Some essentials about Vision Pro"
---
**结论**

严格说，现阶段的 Vision Pro 沉浸视频还达不到“和裸眼看真实世界等价”。它已经能在很多镜头里做出很强的在场感，但 Apple 自己为 Apple Immersive Video 设计的方案——180°–230° 视场、90fps、定制投影、每片段 foveation、MV‑HEVC、空间音频——本质上是在现有带宽和显示极限下尽量保住“像在现场”，不是宣称已经到肉眼级。 ([Apple Developer][1])

**分辨率大概要多高**

这类问题最好用 **ppd（pixels per degree，角分辨率）** 来看，而不是只看“几 K”。长期常见的“视网膜级”说法大约是 60 ppd，但 2025 年的测量把高对比黑白内容的群体均值抬到了 94 ppd，红绿大约 89 ppd，黄紫大约 53 ppd，而且离开中央视野后会明显下降。Apple 在 AIV 的开发者课程里也直接给了工程口径：180° 镜头配普通 8K 相机只有约 24 ppd，AIV 的目标是 60 ppd，AIV 相机最低要到 40 ppd。按这个口径粗算，180° 沉浸视频要到 60 ppd，横向至少要 **10,800 像素/眼**；按 94 ppd 则是 **16,920 像素/眼**。若按 Apple 给出的 180°–230° AIV 视场，上限会到约 **13,800–21,620 像素/眼**。 ([Nature][2])

再看显示端，Vision Pro 当前公开规格是总共 **2300 万像素**，粗分到每眼约 **1150 万像素量级**。而 180°、60 ppd 的均匀采样需要约 **1.166 亿像素/眼**，94 ppd 需要约 **2.863 亿像素/眼**。只按像素预算看，离“全视场肉眼级”大约还差 **10 倍到 25 倍**，而且这还没算镜片畸变校正、边缘利用率和双眼重叠带来的损耗。 ([Apple][3])

**Apple 现在实际做到哪一步**

Apple 对外把 Apple Immersive Video 描述成“**180°、3D、8K**”；但在工具链里，Apple Immersive Video Utility 当前要求的交付导入格式是 **MV‑HEVC、4320×4320、90fps**，并建议使用 **HDR P3 D65 PQ**；实时流的期望格式还会降到 **3600×3600、45fps**。Apple 的开发者课程又说明，AIV 的采集端基线在 **约 7200×7200/眼、90fps**，再通过 foveation 和专门调校的 MV‑HEVC 编码压到 **4320×4320/眼** 的交付目标。现在面向 AIV 的专业机 Blackmagic URSA Cine Immersive 已公开到 **8160×7200/眼、90fps、16 档动态范围**。所以今天的 Apple 链路已经明显超过“普通 8K VR180”，但还不是“眼睛极限”，更像是“高质量、强优化的近眼交付”。 ([Apple][4])

**码率要到多少**

有个很好用的现实锚点：Northeastern 的测量抓到 Apple TV+ 沉浸视频最高质量档大约是 **4320×4320、90fps、99.81 Mbps**。把它当成今天 AIV 的现实基线，如果只按像素数等比例外推，180°、60 ppd 的 **10.8K×10.8K/眼** 大约是 4320 方形/眼的 **6.25 倍**像素量，码率会到 **约 624 Mbps**；94 ppd 的 **16.9K×16.9K/眼** 约是 **15.3 倍**，对应 **约 1.53 Gbps**。这还是偏乐观的估算，因为更高 acuity 意味着更多高频细节，实际压缩通常不会这么线性。 ([Electrical & Computer Engineering Dept.][5])

如果看未压缩量就更夸张：180°、60 ppd、双眼、90fps、10-bit 4:4:4，大约是 **630 Gbps**；94 ppd 则约 **1.55 Tbps**。哪怕退到 10-bit 4:2:0，也还是 **315 Gbps** 和 **773 Gbps** 这个量级。也就是说，真正“肉眼级”的消费者交付，若没有眼动跟踪驱动的 **foveated transport / foveated coding**，基本不现实。 ([arXiv][6])

**色彩上能不能到“像真实世界”**

色彩比分辨率更难，因为“像真实世界”不只是 gamut。Vision Pro 当前公开规格是 **92% DCI‑P3**，并支持 **Dolby Vision、HDR10、HLG** 播放；Apple 对 AIV 交付的推荐则是 **HDR P3 D65 PQ**。与此同时，ITU‑R BT.2100 的 HDR 体系用的是 **PQ/HLG**、BT.2020 的系统色度学，并定义了 **10-bit 和 12-bit** 表示。换句话说，今天的 Vision Pro / AIV 已经处在“高端消费级 HDR 宽色域视频”这一级，但从 Apple 公开规格并不能证明它已经达到真实世界那种完整的色域、亮度、黑位和色容积；再加上人眼本身的色彩分辨率在不同色轴上也不一样，所以“色彩像真实世界”比“中心区域够清楚”还难。 ([Apple][3])

**整条链路最关键的提升**

关键不是只把“8K 提到 16K”，而是整条链路一起补：采集端要继续往 **每眼 10K–20K、90–120fps、更高动态范围、更低噪声、更严格双目同步和标定** 走；几何表示要继续避免传统 lat‑long / 普通 VR180 的重采样损失，Apple 现在走的是带镜头与投影元数据的定制路径；编码端要更依赖 **eye‑tracked foveation** 和专门调校的 multiview codec；网络分发要准备好 **数百 Mbps 到 Gbps** 的稳定吞吐；显示/光学要有更高的有效 ppd、更大的色容积、更低 flare；最后还要解决当前 HMD 普遍存在的 **固定焦面** 问题，也就是 vergence‑accommodation conflict。更根本地说，即便分辨率和色彩都拉满，**双目立体视频本身也没有真实世界那种完整的平移视差**，所以要做到“严格等价”，终点其实更像 **varifocal / multifocal、light field，或者更密集的 multiview / volumetric**，而不只是更高码率的 stereo video。 ([blackmagicdesign.com][7])

**一句话收尾**

所以答案是：**能逼近，但现在还远没到“肉眼看真实世界”的级别。** 以 Apple 当前公开链路看，AIV 已经把“在场感”做得非常强；但要到“分辨率、色彩、深度线索都和真实世界无差别”，今天的捕获、带宽、显示和光学都还不够，而且两路立体视频这个表示方式本身也不是最终形态。 ([Apple Developer][1])

需要的话，我下一条把这些数字按 **60 / 94 ppd、180° / 230°、10-bit 4:2:0 / 4:4:4** 展成一张对照表。

[1]: https://developer.apple.com/videos/play/meet-with-apple/222/?time=573 "https://developer.apple.com/videos/play/meet-with-apple/222/?time=573"
[2]: https://www.nature.com/articles/s41467-025-64679-2 "Resolution limit of the eye — how many pixels can we see? | Nature Communications"
[3]: https://www.apple.com/apple-vision-pro/specs/ "Apple Vision Pro - Technical Specifications - Apple"
[4]: https://www.apple.com/apple-vision-pro/ "Apple Vision Pro - Apple"
[5]: https://ece.northeastern.edu/fac-ece/dkoutsonikolas/publications/poster_mobicom24.pdf "A First Look at Apple's Stereoscopic Video and its Potential in Live Video Streaming for XR Headsets"
[6]: https://arxiv.org/html/2410.06068v1 "Resolution limit of the eye: how many pixels can we see?"
[7]: https://www.blackmagicdesign.com/products/blackmagicursacineimmersive/techspecs "Blackmagic URSA Cine Immersive – Tech Specs | Blackmagic Design"



































