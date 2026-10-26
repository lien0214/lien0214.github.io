---
title: "基於圖論的 RAG 防禦"
description: "免訓練的黑箱防禦方法，透過圖的不對稱性偵測檢索增強生成（RAG）中的投毒段落。"
date: 2026-03-01
period: "2026 年 3 月 – 7 月"
team: "AIS Lab · 3 人團隊 · 共同第一作者"
category: "Research"
status: "ACL ARR（2026 年 10 月）審稿中"
stack: ["RAG", "AI Safety", "圖論", "黑箱防禦", "免訓練"]
---

## 問題

檢索增強生成（RAG）會信任檢索器回傳的內容。只要攻擊者能在語料庫中放入段落，就能操控模型的回答，這就是 RAG 投毒。

## 我們的方法

一個不需訓練、也不需存取模型內部的防禦：把檢索到的段落視為一張圖，透過圖的不對稱性偵測投毒。因為免訓練又是黑箱，可以直接放在既有的 RAG pipeline 前面，不必更動模型。

這是與 [AIS Lab](https://ntuaislab.github.io/) 合作、為期四個月的專題，我是共同第一作者。論文目前在 ACL Rolling Review（ARR）2026 年 10 月 cycle 審稿中。
