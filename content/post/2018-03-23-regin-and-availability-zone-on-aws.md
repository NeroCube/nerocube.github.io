---
layout:     post
title:      "Why you need Region and Availability Zone?"
subtitle:   " \"Everything fails all the time — build for resiliency\""
date:       2018-03-23 14:02:00
author:     "Nero"
image: "img/post-bg-os-metro.jpg"
tags:
    - AWS
    - 學習筆記
---

{{< myblockquote "colorquote info" "“學習為什麼需要 Region 與 Availability Zones”" >}}

## 前言

在學習 AWS Cross-Region Replication 與 Availability Zones 時意外的找到了 [How to build a multi-region active-active architecture on AWS](https://read.acloud.guru/why-and-how-do-we-build-a-multi-region-active-active-architecture-6d81acb7d208) 這篇優質的文章，所以在這邊進行整理。

## Region 與 Availability Zones
什麼是 Region?什麼是 Availability Zones?
- Region: 地域是指物理的數據中心的地理區域
- Availability Zones: 可用區（Zone）是指在同一地域內電力和網絡互相獨立的物理數據中心。目標是能夠保證可用區間故障相互隔離（大型災害或者大型電力故障除外），不出現故障擴散，使得用戶的業務持續在線服務。通過啟動獨立可用區內的實例，用戶可以保護應用程序不受單一位置故障的影響。
每一個區域都是完全獨立的且每個可用區也是獨立的，但區域內的可用區通過低延遲鏈接相連。下圖説明區域和可用區之間的關係。

![image](https://user-images.githubusercontent.com/8331034/192158374-52cf4443-47d5-4772-b907-ba2d9d7c637f.png)


## 為什麼需要 Region 與 Availability Zones
- 改善用戶端的延遲問題
- 災難修復
- 商務需求

## 改善用戶端的延遲問題
當用戶端距離使用者越近，理論上請求的反應時間就會越短，而用戶對於服務的體驗就會越好。
Content Cloud Networks（CDN）（如Amazon CloudFront）已成功用於加速向全球用戶端傳送內容，尤其是靜態內容（例如圖像，影片，JavaScript等）。
透過在全球部署 memcached(多線程) 或 redis(單線程) 這類分佈式緩存服務器網絡可以改善靜態文件文件的存取速度。

即使CloudFront解決了大部分內容問題，但系統使用上仍然會需要進行後端的資料操作，如果伺服器地理位置距離用戶端過遠會產生延遲，但在遊戲或金融系統中這樣的延遲會嚴重的影響到，使用者的體驗與資料的可靠程度，進而產生許多不可預期的問題，且隨著AR，VR和MR的出現，需要更加逼真的體驗，開發具有嚴格等待時間要求的系統變得越來越重要。

## 災難修復

> Netflix設計用於處理區域內全部或部分單一可用區域的故障，因為我們跨越三個區域運行，並且在兩個區域都不會丟失功能。我們正在努力擴展我們的彈性以處理部分或全部區域性停電。艾德里安科克羅夫特。[(link)](https://medium.com/netflix-techblog/a-closer-look-at-the-christmas-eve-outage-d7b409a529ee)
![區域防災能力](https://cdn-images-1.medium.com/max/1600/1*e1Xht1m5hZF5S07my4gbZg.png)

如果您的應用程序（如Netflix）由多種不同的服務組成，服務中的其中一個應用程序故障，則可以將流量轉移到正常運作的地區以影響用戶的系統使用。

## 商務需求
一些客戶可能有業務需求將數據存儲在幾百公里的不同地區。如：中國政策限制因此，這些客戶必須將數據存儲在多個地區。由於AWS目前在全球擁有18個地區，分佈在美洲，亞太地區，歐洲，中東和非洲地區，因此這種趨勢越來越普遍。

## 參考資料

[How to build a multi-region active-active architecture on AWS](https://read.acloud.guru/why-and-how-do-we-build-a-multi-region-active-active-architecture-6d81acb7d208)

[Regions and Availability Zones - Amazon](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

[AWS Glossary - Amazon](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html)
