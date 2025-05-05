# Qual-Prep Notes: Advanced OS • Computer Networks • Algorithms • Computer Architecture

## 博士班資格考四科學習筆記倉庫

## 📚 目錄結構  Directory Layout

```

.
├── os/ # Advanced Operating System 筆記 & Lab
│ ├── notes/ # Markdown 筆記（依章節）
│ ├── labs/ # xv6 / OSTEP / MIT 6.828 實作
│ └── papers/ # 重要論文精讀
├── networks/ # Computer Networks
│ ├── notes/
│ ├── labs/ # Wireshark / Mininet / QUIC
│ └── papers/
├── algorithms/ # Advanced Algorithms
│ ├── notes/
│ ├── leetcode/ # 題目 + 白板推導 + 複雜度
│ └── papers/
├── architecture/ # Advanced Computer Architecture
│ ├── notes/
│ ├── sims/ # GEM5 / RTL / Roofline
│ └── papers/
└── roadmap.md # 整體進度與里程碑

```

## 🎯 專案目標  Goals

1. **結構化整理**四門核心課程所有重點，方便快速複習與搜尋。
2. **同步實作**：每門課至少一個完整 Lab/專案，橋接理論與實務。
3. **考前衝刺**：提供 Mock Quiz、考古題索引與錯題解析。

## 🗓️ 推進節奏  Milestones

| 時程    | 目標                                                 |
| ------- | ---------------------------------------------------- |
| M1–M3   | 完成 Algorithms & OS 基礎章節筆記；刷題 100/300      |
| M4–M6   | 完成 OS Labs + Algorithms 進階筆記；Mock Quiz #1     |
| M7–M9   | Networks 筆記 + Lab、Architecture 筆記；刷題 250/300 |
| M10–M12 | 二輪精讀 & 真題衝刺；Mock Quiz #2；資格考應試        |

> **進度跟蹤**請看  [`roadmap.md`](./roadmap.md)。

## 🛠️ 使用方式  How to Use

1. **Clone repo**

   ```bash
   git clone https://github.com/yourname/qual-prep-notes.git
   ```

2. **瀏覽筆記**

   - Markdown 檔可直接在 GitHub 預覽或匯入 Obsidian。
   - 圖片／公式以本地路徑引用，離線可讀。

3. **執行實驗**

   - 進入對應 `labs/` 或 `sims/` 資料夾，依 README 操作。
   - 需要 Docker 的專案已附 `docker-compose.yml`。

## 🤝 貢獻  Contributing

歡迎 PR／Issue：

- 修正錯誤或拼字
- 新增考古題解析、Lab 改進
- 分享高品質參考論文或教學資源

Merge  前請確保：

- 使用繁體中文（必要時附英文對照）
- 通過 `markdownlint` 與 `pre-commit` 檢查

## 📖 主要參考書  Key References

- *Operating System Concepts* (10th)
- _Operating Systems: Three Easy Pieces_
- *Computer Networking: A Top‑Down Approach* (8th)
- *Introduction to Algorithms* (4th, CLRS)
- *Computer Architecture: A Quantitative Approach* (6th)

### 📚 四門科目的「教材生態」總覽

（依*實際課程採用頻率*排序；統計來自 2024‑2025  年各大學課綱／書評榜）

| 科目                               | 最常用 (核心課本)                                                                                                          | 第二常用 (替代主流)                                                                                                     | 推薦補充 / 強化                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Advanced Operating System**      | **《Operating‑System Concepts (10/e)》Silberschatz et al.** – 多數研究所仍指定，Amazon OS 類別長期前 5  名 ([Amazon][1])   | **《OSTEP: Operating Systems – Three Easy Pieces》Arpaci‑Dusseau** – 免費 PDF，被「全球數百所院校使用」([Wikipedia][2]) | **《Modern Operating Systems (4/e)》Tanenbaum** – 分散式與 MINIX 案例深入                               |
| **Computer Network**               | **《Computer Networking: A Top‑Down Approach (8/e)》Kurose & Ross** – GfG 2024  年「最佳網路書籍」榜首([GeeksforGeeks][3]) | **《Computer Networks (6/e)》Tanenbaum & Wetherall** – 同榜 Top 10  且被多門課沿用([GeeksforGeeks][3])                  | **《Computer Networks: A Systems Approach (6/e)》Peterson & Davie** – 協定實作與軟體定義網路視角        |
| **Algorithms**                     | **《Introduction to Algorithms (CLRS 4/e)》** – MIT 6.006 官方參考書籍([MIT OpenCourseWare][4])                            | **《Algorithms》Dasgupta⋯Vazirani** – GaTech CS 6515 指定教材                                                           | **《Algorithm Design》Kleinberg & Tardos** – 同課程建議閱讀                                             |
| **Advanced Computer Architecture** | **《Computer Architecture: A Quantitative Approach (H\&P 6/e)》** – 劍橋大學 ACA 課程唯一推薦([Cambridge Computer Lab][5]) | **《Computer Organization and Design (COD RISC‑V 6/e)》H\&P** – 自底向上理解 ISA→µArch                                  | **《Digital Design and Computer Architecture (Harris & Harris)》** – RTL → Pipeline 打底；FPGA lab 友好 |

---

## 🧠 高效備考方法論

> 「**教科書 ≠ 唯一資訊來源**；真題 + Lab + Paper 三管齊下，才能在資格考中脫穎而出。」

### 1. 構建「三層知識塔」

1. **基石（概念）**

   - 逐章做 _Concept Map_（Mermaid/Obsidian）—  用 1  頁圖勾勒章節脈絡。

2. **骨架（公式 & 演算法流程）**

   - 為每個核心公式或演算法寫「輸入 → 處理 → 邊界 → 複雜度」四行摘要。

3. **肌肉（實戰 / 真題）**

   - OS：移植 xv6/DIY kernel module；CN：Wireshark & Mininet Lab；ALG：每週 5 題限時 LeetCode；ARCH：GEM5  或  Perf 分析 CPI‑stack。

### 2. 週期化學習—「4‑4‑2 模式」

| 週期      | 內容                                      |
| --------- | ----------------------------------------- |
| **4  天** | 精讀兩章＋做筆記＋小實驗                  |
| **4  天** | 翻寫重點 & 自擬出題 2‑3  題               |
| **2  天** | 刷歷屆考題 / 課堂 Quizzes，並口頭講解解法 |

### 3. 科目特定技巧

| 科目 | 破題要領                                     | 典型考法                             | 建議練習                            |
| ---- | -------------------------------------------- | ------------------------------------ | ----------------------------------- |
| OS   | 排程、虛擬記憶、檔案系統時間序圖             | 「給程式片段、問 state/中斷序列」    | Trace 系統呼叫 + 手繪 state diagram |
| CN   | 流量控制、路由、TCP 細節                     | 算 RTT、推導 throughput、畫 BGP Path | Wireshark 重現三次握手 & 擁塞窗口   |
| ALG  | DP、圖論、NP/Approx                          | 推導複雜度、證明正確性               | 白板手寫 + 模擬口試 10 min/p 題     |
| ARCH | Pipeline hazard、Cache/Memory hierarchy、ILP | 計 CPI、cache miss、coherence 狀態   | GEM5 跑 SPEC；Roofline 找瓶頸       |

### 4. 真題資源

- **Georgia Tech OMSCS Quals**：OS、CN、ALG 原題庫。
- **CMU Qual Archive**：Algorithms + Architecture 經典。
- **劍橋 ACA Past Exams**：計算型密集題、命題風格接近台灣多所大學。

### 5. 關鍵里程碑

1. **Month 6**：四科基礎章節 + 每科至少 1  次 Lab Demo。
2. **Month 12**：完成全部核心章節；第一次 3 hr 模考（60 % 以上為佳）。
3. **Month 18**：二輪精讀論文 / 前沿議題（OS–eBPF、CN–QUIC、ALG–Streaming、ARCH–Chiplet/CXL）。
4. **考前三個月**：只做真題與錯題回顧，保留每日 30 min 口述講解。

---

> **策略總結**：
>
> - 以「熱門主教材 + 次熱門備援 + 1 本進階」組成**3‑書閉環**。
> - 配合「概念圖 → 手寫推導 → 實驗 / 程式」遞進。
> - 早期打好 **Algorithm & OS 基礎**，後期疊加 **CN & ARCH**，保持交叉複習。

計劃在 1–2  年內順利通過四科資格考，正式開啟博士研究之旅！

[1]: https://www.amazon.com/Best-Sellers-Computer-Operating-Systems-Theory/zgbs/books/3863?utm_source=chatgpt.com 'Amazon Best Sellers: Best Computer Operating Systems Theory'
[2]: https://en.wikipedia.org/wiki/Remzi_Arpaci-Dusseau?utm_source=chatgpt.com 'Remzi Arpaci-Dusseau'
[3]: https://www.geeksforgeeks.org/best-computer-networks-books/ '10 Best Computer Networking Books To Learn From [2024 Updated] | GeeksforGeeks'
[4]: https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/syllabus/ 'Syllabus | Introduction to Algorithms | Electrical Engineering and Computer Science | MIT OpenCourseWare'
[5]: https://www.cl.cam.ac.uk/teaching/2425/AdComArch/ 'Department of Computer Science and Technology – Course pages 2024–25: Advanced Computer Architecture'

---

5  所美國大型研究型大學（MIT、Stanford、CMU、Berkeley、Georgia Tech）對 **Advanced OS、Computer Networks、Advanced Algorithms、Computer Architecture** 四門「研究所核心系統課」的**實際開課學期**。

> 註：各校偶有輪流或雙學期開課；下表取 **最近 2‑3  年公告**為主，如課程網站明示  term / quarter / semester 即列入。

| 學校                        | Advanced OS                                                            | Computer Networks                                                                         | Advanced Algorithms                                                        | Computer Architecture                                                                              |
| --------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **MIT**                     | 6.828 Operating‑System Engineering **Fall** ([pdos.csail.mit.edu][1])  | 6.829 Computer Networks **Fall** ([MIT OpenCourseWare][2])                                | 6.854 Advanced Algorithms **Spring** ([MIT CSAIL][3])                      | 6.823 Computer System Architecture **Fall** ([csg.csail.mit.edu][4])                               |
| **Stanford** (Quarter 制)   | CS240 Adv. Topics in OS **Spring** Q ([Stanford University][5])        | CS244 Adv. Networking **Spring** Q ([cs244.stanford.edu][6])                              | CS261 Optimization & Alg. Paradigms **Autumn** Q ([cs261.stanford.edu][7]) | EE382C / CS316 系列 (多核/互連) **Winter/Spring** Q (近年  W 2024) ([Stanford Explore Courses][8]) |
| **CMU**                     | 15‑712 Adv.& Dist. OS **Fall** ([CMU School of Computer Science][9])   | 15‑744 Graduate Computer Networks **Spring** 2024  版 ([computer-networks.github.io][10]) | _15‑651/15‑750 系列_ 多開在 **Fall** (未列出)                              | _18‑740 Computer Arch._ 傳統 **Fall**，但 18‑742 深階架構多為 **Spring**                           |
| **UC Berkeley**             | CS262A Adv. OS 通常 **Fall** (近年 2023)                               | —（專業網路課 CS268 多為 **Fall**）                                                       | CS270 Adv. Algorithms **Spring**（傳統）                                   | CS252 Graduate Computer Architecture **Spring** (歷年 S96、S24) ([EECS at UC Berkeley][11])        |
| **Georgia Tech** (Semester) | CS 6210 Adv. OS：常於 **Fall & Spring** (OMSCS 每年雙開) ([OMSCS][12]) | CS 6250 Computer Networks：近期公告 **Spring** 2024/25 ([OSCAR][13], [OSCAR][14])         | CS 8803 Adv. Alg.：多在 **Fall** (隔年開)                                  | ECE 8803 Adv. Arch.、CS 6290：常排 **Fall**                                                        |

### ⬇️ 觀察到的「大體模式」

1. **Advanced Operating Systems**

   - **Fall** 開課最常見（MIT、CMU、Berkeley、GaTech）。
   - Stanford 採 **Spring Quarter**，屬例外。
   - 理由：許多博士班希望新生第一學期就完成系統核心基礎並投入研究計畫。

2. **Computer Networks**

   - 兩種典型：

     - **Spring**→ Stanford、GaTech、CMU（近年）；
     - **Fall**→ MIT（6.829）、Berkeley（CS268）。

   - 系統/網路老師通常與 OS 老師錯開，以平衡師資。

3. **Advanced Algorithms**

   - 與學校數學／理論組課程節奏強相關：

     - MIT、Berkeley 走 **Spring**；
     - Stanford、GaTech 偏 **Fall/Autumn**。

   - 建議先修完基礎 CLRS 課（研所或高年級本科），再選高階。

4. **Computer Architecture**

   - **Fall**：MIT 6.823、Stanford EE382 系列（部分 Winter）。
   - **Spring**：Berkeley CS252、CMU 18‑742。
   - 通常與硬體系研討會排錯時段，並避免與 OS 同教授撞期。

### 🗺️ 如何排自己的修課順序？

| 年度 / 學期     | 推薦組合 (大多數學校可行)                   | 說明                                                                              |
| --------------- | ------------------------------------------- | --------------------------------------------------------------------------------- |
| **1A (Fall)**   | **Advanced OS + Architecture**              | 奠定系統＋硬體視角，實驗室也常在此時招 RA。                                       |
| **1B (Spring)** | **Computer Networks + Advanced Algorithms** | 演算法為之後研究方法論基底；網路課通常有 project，可與 OS/Arch 貫通做分散式實驗。 |
| **2A (Fall)**   | 專題／選修（分散式、存儲、ML Systems…）     | 若校內 Algorithms 在秋季，可以此時補修。                                          |
| **2B (Spring)** | 補足未修或準備資格考                        | 鎖定弱項回鍋；進入 Qual 考前衝刺期。                                              |

> **結論**：沒有「全美統一」的固定次序，但**Fall：OS + Arch、Spring：Networks + Algorithms** 是最常見的搭配。
> 規劃時以 **自己學校的近期課表** 為準，先卡早關閉的課 (多半是系統類 Fall only)；確保第一年內至少修畢 OS 或 Algorithms，其概念在後續研究最常被要求。祝自己選課順利！

[1]: https://pdos.csail.mit.edu/6.828/2018/overview.html?utm_source=chatgpt.com '6.828 / Fall 2018 - MIT PDOS'
[2]: https://ocw.mit.edu/courses/6-829-computer-networks-fall-2002/?utm_source=chatgpt.com 'Computer Networks | Electrical Engineering and Computer Science'
[3]: https://people.csail.mit.edu/moitra/854.html?utm_source=chatgpt.com '6.854/18.415 Advanced Algorithms, Spring 2016 - People | MIT CSAIL'
[4]: https://csg.csail.mit.edu/6.5900/?utm_source=chatgpt.com '6.5900 [6.823] Computer System Architecture - Fall 2024 - MIT'
[5]: https://web.stanford.edu/class/cs240/old/sp2022/?utm_source=chatgpt.com 'Stanford CS240: Advanced Topics in Operating Systems'
[6]: https://cs244.stanford.edu/?utm_source=chatgpt.com 'CS244: Advanced Topics in Networking, Spring 2025'
[7]: https://cs261.stanford.edu/?utm_source=chatgpt.com 'CS 261: Optimization and Algorithmic Paradigms'
[8]: https://explorecourses.stanford.edu/search?q=ee382c&utm_source=chatgpt.com 'EE 382C - Explore Courses - Stanford University'
[9]: https://www.cs.cmu.edu/~15712/?utm_source=chatgpt.com '15-712 Advanced and Distributed Operating Systems, Fall 2024'
[10]: https://computer-networks.github.io/15-744-sp24/?utm_source=chatgpt.com 'Graduate Computer Networks'
[11]: https://www2.eecs.berkeley.edu/Courses/CS252/?utm_source=chatgpt.com 'Course: CS252 | EECS at UC Berkeley'
[12]: https://omscs.gatech.edu/cs-6210-advanced-operating-systems?utm_source=chatgpt.com 'CS 6210: Advanced Operating Systems - OMSCS - Georgia Tech'
[13]: https://oscar.gatech.edu/bprod/bwckctlg.p_disp_listcrse?crse_in=6250&schd_in=%25&subj_in=CS&term_in=202402&utm_source=chatgpt.com 'Class Schedule Listing - OSCAR - Georgia Tech'
[14]: https://oscar.gatech.edu/bprod/bwckctlg.p_disp_listcrse?crse_in=6250&schd_in=%25&subj_in=CS&term_in=202502&utm_source=chatgpt.com 'Class Schedule Listing - OSCAR - Georgia Tech'

---

## 📝 授權  License

本倉庫文字內容採 **CC BY‑SA 4.0**；程式碼與 Lab 範例採 **MIT License**。詳見 [`LICENSE`](./LICENSE)。

---

> _Keep learning, keep iterating — 準備周全，自信迎戰博士班資格考！_
