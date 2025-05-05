# Operating System Concepts – 筆記整理 (Chapters 1–21)

---

## 📘 Chapter 1: Introduction（導論）

### 🌟 本章重點目標

- 瞭解電腦系統組織與中斷（interrupts）的角色
- 描述多處理器系統的組成
- 說明從使用者模式轉換至核心模式（kernel mode）的過程
- 探討作業系統在不同運算環境中的應用
- 提供自由與開源作業系統的範例

### 1.1 What Operating Systems Do（作業系統的角色）

- 作業系統 = 硬體資源管理者 + 使用者介面 + 控制程式
- 使用者觀點 vs 系統觀點
- 程式 = 被動；程序 = 主動執行的單位

### 1.2 Computer-System Organization

- CPU、控制器、RAM → 共享總線
- 使用 interrupt 處理裝置請求
- 儲存結構階層（Register → Cache → RAM → SSD → Tape）

### 1.3 Computer-System Architecture

- 單處理器 vs 多處理器（SMP, NUMA）
- 叢集系統（高可用性與資源共享）

### 1.4 OS Operations

- Bootstrap 程式 → Kernel 載入 → Daemon 啟動
- Trap: 軟體產生中斷（如系統呼叫、除以 0）
- 使用 Mode Bit 區分 User/Kernel 模式

### 1.5 Resource Management

- OS 管理 Process、Memory、I/O、File 等資源
- Process 是程序，具備代碼段、資料段、堆疊、堆積

---

## 🧱 Chapter 2: Operating-System Structures（作業系統結構）

### 2.1 OS 提供的服務

- 使用者層服務：UI、檔案處理、I/O 操作、通訊、錯誤檢測
- 系統層服務：資源分配、帳務、保護與安全

### 2.2 使用者與 OS 介面

- CLI、GUI、Touch UI 的比較與應用情境

### 2.3 System Calls（系統呼叫）

- API 與 System Call 的轉換
- 常見 system call 分類（Process/File/Device/Comm 等）
- 傳參方法（Register / Stack / Memory Block）

### 2.4 System Services

- Daemon 背景服務、Compiler/Debugger 支援、File 工具等

### 2.5 Linking and Loading

- 編譯 → 連結 → 載入流程：`.c → .o → .exe → memory`
- Linux: ELF / Windows: PE / macOS: Mach-O

### 2.6 程式無法跨平台原因

- ABI、呼叫介面、執行檔格式不同
- 解決方式：Interpreter / VM / POSIX 標準

### 2.7 OS 設計與實作

- 機制 vs 策略
- 單核心、微核心、模組化核心等架構

---

## 🧠 Chapter 3: Processes（程序）

### 3.1 Process Concept

- 程式碼段、資料段、Stack/Heap
- Process States: New, Ready, Running, Waiting, Terminated
- Process Control Block (PCB) 儲存程序資訊

### 3.2 Process Scheduling

- Ready Queue / Wait Queue
- Context Switch 開銷與時機

### 3.3 Process Operations

- UNIX: `fork()`, `exec()`, `wait()`, `exit()`
- Zombie / Orphan Process 定義
- Android Process 等級：Empty → Foreground

### 3.4 Interprocess Communication (IPC)

- 動機：共享資訊、模組化、分散運算
- Shared Memory vs Message Passing

### 3.5 Shared Memory

- Producer-Consumer 模型
- 使用 Circular Buffer 進行同步

### 3.6 Message Passing

- Blocking / Non-blocking 通訊
- Zero / Bounded / Unbounded Buffer 模式

---

🔁 **總結：作業系統是以程序為核心的多工平台，提供一套抽象的資源管理與通訊機制，使複雜系統能被有效控制與使用。**

---

## 🧵 Chapter 4: Threads（執行緒）

### 4.1 Overview（概述）

- 程序是資源分配與保護的單位；**執行緒是 CPU 執行的最小單位**。
- 多執行緒允許：

  - 更快的回應（responsiveness）
  - 更有效率的資源共享（共用記憶體空間）
  - 更少 overhead（建立執行緒比建立程序便宜）
  - 更佳的多核心延展性

### 4.2 多核心程式設計（Multicore Programming）

- **Concurrency（並發）**：多個任務交錯進行（不一定同時）
- **Parallelism（平行）**：多個任務同時執行
- 挑戰：

  1. 任務分解
  2. 工作負載平衡
  3. 資料分割
  4. 相依管理
  5. 除錯測試

#### Amdahl’s Law：

- 最大加速比受限於序列部分：
  Speedup = `1 / (S + (1-S)/N)`

#### 類型：

- **Data Parallelism**：同樣運算作用於不同資料
- **Task Parallelism**：不同任務作用於相同或不同資料

### 4.3 Multithreading Models（執行緒模型）

#### User Threads vs Kernel Threads

- **User Threads**：由使用者空間函式庫實作，不被 OS 感知
- **Kernel Threads**：由作業系統核心直接管理

#### 三種映射模型：

1. **Many-to-One**：多個使用者執行緒對應一個核心執行緒
   ➤ 缺點：無法真正並行，阻塞問題
2. **One-to-One**：每個使用者執行緒對應一個核心執行緒
   ➤ 缺點：系統開銷大
3. **Many-to-Many**：多個使用者執行緒映射到較少數的核心執行緒
   ➤ 複雜但具彈性
4. **Two-Level Model**：Many-to-Many + optional binding

### 4.4 Thread Libraries（執行緒函式庫）

- **Pthreads**：POSIX 標準，C 語言用於 UNIX 系統
- **Windows Threads**：Windows 專用
- **Java Threads**：JVM 上層封裝，建構在 native thread 上

#### 執行策略：

- **Asynchronous**：父與子執行緒可並行
- **Synchronous**：父執行緒等待子執行緒完成（join）

### 4.5 Implicit Threading（隱式多執行緒）

#### 背景：

- 開發者只定義任務，執行緒由系統處理

#### 方法：

1. **Thread Pools**

   - 預先建立執行緒，避免頻繁建立與銷毀
   - 限制同時執行緒數量
   - 範例：Java `Executors.newFixedThreadPool(n)`

2. **Fork-Join 模型**

   - 遞迴拆分子任務 → 平行執行 → 合併
   - 範例：Java `ForkJoinPool`

3. **OpenMP**

   - C/C++/Fortran：使用 `#pragma omp parallel` 指令標示可平行區塊

4. **Grand Central Dispatch (GCD)**

   - Apple 系統：任務導向 queue（serial/concurrent）
   - 使用 blocks 或 closures 表示任務

5. **Intel Thread Building Blocks (TBB)**

   - C++ 平行函式庫：支援 template 並行控制流程、容器、任務圖

---

## 第五章：CPU Scheduling 筆記

### 5.1 基本概念（Basic Concepts）

- 目標：提升 CPU 使用率與系統效率。
- 核心機制：在多個 process 間切換 CPU。
- **CPU–I/O Burst Cycle**：Process 執行時交替發生 CPU burst 與 I/O burst。

  - I/O-bound：多個短 CPU burst。
  - CPU-bound：少量長 CPU burst。

- **CPU Scheduler**：選擇 ready queue 中的 process。
- **Preemptive vs Nonpreemptive**：

  - Preemptive：可中斷並切換（現代系統預設）。
  - Nonpreemptive：process 自願釋出 CPU。

- **Dispatcher**：執行 context switch。調度延遲稱為 dispatch latency。

---

### 5.2 排程評估指標（Scheduling Criteria）

- **CPU utilization**：盡可能讓 CPU 忙碌（40–90%）。
- **Throughput**：每單位時間完成的 process 數。
- **Turnaround time**：從提交到完成的總時間。
- **Waiting time**：只計算在 ready queue 的等待時間。
- **Response time**：從提交到第一次回應的時間。
- 目標：最大化 utilization/throughput，最小化 turnaround/waiting/response time。

---

### 5.3 排程演算法（Scheduling Algorithms）

#### 5.3.1 FCFS（First-Come, First-Served）

- FIFO queue，非搶佔式。
- 缺點：長 process 可能拖慢整體效率（convoy effect）。

#### 5.3.2 SJF（Shortest-Job-First）

- 執行預估 CPU burst 最短的 process。
- 可為 preemptive（Shortest Remaining Time First）或 nonpreemptive。
- 需預測下一次 CPU burst，使用 exponential averaging：

  - τₙ₊₁ = αtₙ + (1–α)τₙ

- 最佳化平均 waiting time，但難以實作精確預測。

#### 5.3.3 RR（Round Robin）

- 為 FCFS 加上 time quantum（10–100ms）。
- 適合互動式系統。
- quantum 太小：context switch overhead 高。
- quantum 太大：退化為 FCFS。
- rule of thumb：80% CPU bursts < quantum。

#### 5.3.4 Priority Scheduling

- 根據 priority 值排程，可 preemptive 或 nonpreemptive。
- 問題：低優先權 process 可能 starvation。
- 解法：aging，等待時間久則提升優先權。
- 可結合 RR（同優先權者用 RR）。

#### 5.3.5 Multilevel Queue

- 多個 queue（如 real-time, system, interactive, batch），各自有排程策略。
- queue 間排程通常為 fixed-priority preemptive 或 time-slice。

#### 5.3.6 Multilevel Feedback Queue

- 可調整 process 所在 queue（根據行為）。
- CPU burst 長 → 降級；等太久 → 升級。
- 最通用但最複雜，需要設定：

  - queue 數量
  - 每層排程演算法
  - 升/降級策略
  - 初始分配規則

---

### 5.4 Thread Scheduling

- **PCS (Process-Contention Scope)**：同一 process 內部排程（由 user thread library 控制）。
- **SCS (System-Contention Scope)**：跨 process 全系統排程（由 kernel 控制）。
- 一對一模型（Linux、Windows）：僅用 SCS。
- **Pthreads API**：

  - `pthread_attr_setscope(&attr, PTHREAD_SCOPE_SYSTEM)`
  - 可設定 scope（部分系統不支援 PROCESS）

---

## 第六章 Synchronization Tools

### ✳️ 關鍵概念

- 多個 Process 或 Thread 在共享資料時若無同步，會導致 Race Condition。
- 為確保資料一致性，需使用 Synchronization 工具解決 Critical Section 問題。

---

### 🔐 6.1 背景與 Race Condition

- 多進程/多執行緒同時修改共享變數（如 `count++` 和 `count--`）會產生錯誤結果（例如：預期 5，實際可能變 4 或 6）。
- 關鍵原因是機器語言的 Load/Store 指令順序可能交錯執行，導致資料競爭。

---

### 🔁 6.2 Critical-Section 問題

#### 三大要求：

1. **Mutual Exclusion**：同一時間只允許一個進程在 critical section。
2. **Progress**：若沒人在 critical section，則允許等待者進入，不能無限延遲。
3. **Bounded Waiting**：等待進入 critical section 的次數有上限。

---

### 📐 6.3 Peterson’s Solution（只適用於兩個 process）

```c
flag[i] = true;
turn = j;
while (flag[j] && turn == j);
```

- 使用 `flag[]` 表示是否想進入 critical section。
- 使用 `turn` 表示讓對方先進入。
- 滿足三大要求，但現代硬體可能會因為指令重排序導致失效（需 memory barrier 保護）。

---

### 🧱 6.4 硬體支援

#### Memory Barrier

- 強制讓先前的 load/store 操作完成才允許之後的。
- 解決 CPU 或編譯器對 Load/Store 的重排序問題。

#### Test-and-Set

```c
boolean test_and_set(boolean *target) {
  boolean rv = *target;
  *target = true;
  return rv;
}
```

#### Compare-and-Swap (CAS)

```c
int compare_and_swap(int *value, int expected, int new_value) {
  int temp = *value;
  if (*value == expected) *value = new_value;
  return temp;
}
```

#### Atomic Variables

- 封裝像是 `int` 的原子操作（如 `atomic increment()`）。
- 適用於簡單情境，但無法完全取代其他同步機制。

---

### 🧰 6.5 Mutex Locks

```c
acquire();  // 進入 critical section
// critical section
release();  // 離開 critical section
```

- 若鎖不可用，會「忙等待（busy wait）」，導致 CPU 資源浪費。
- 若鎖短時間持有，可考慮 spinlock，否則用其他策略（如 semaphore）。

---

### ⛳ 6.6 Semaphores

- 類型：Counting / Binary Semaphore
- 操作：

```c
wait(S);   // S--
signal(S); // S++
```

#### 用途：

- 控制資源存取數量
- 實作等待/通知機制（例如：讓 P2 的 S2 在 P1 的 S1 之後執行）

#### 實作（避免 busy wait）：

```c
wait(S):
  S.value--;
  if S < 0: add to S.queue and sleep()

signal(S):
  S.value++;
  if S <= 0: remove one from S.queue and wakeup()
```

---

### 🧭 6.7 Monitors

- 高階抽象資料型態，將共享變數與操作封裝，內建 mutual exclusion。

```c
monitor M {
  condition x;

  procedure P1() {
    ...
    x.wait();
    ...
  }

  procedure P2() {
    ...
    x.signal();
  }
}
```

#### Condition Variable

- `x.wait()`：進入等待
- `x.signal()`：喚醒一個等待中的進程（可能需搭配 `signal-and-wait` 機制）

#### Conditional Wait with Priority

```c
x.wait(c); // 以 priority c 等待
```

---

### 🔄 6.8 Liveness

#### Deadlock（死結）

- 多個 process 相互等待彼此釋放資源（例如 P0 等 P1，P1 又等 P0）。

#### Priority Inversion（優先權反轉）

- 高優先權 process 等待低優先權 process，而低優先權 process 被中優先權 process 搶佔，造成高優先權 process 無法前進。
- 解法：Priority Inheritance Protocol

---

### ✅ 6.9 評估

- 硬體指令適合構建更高階同步工具。
- 對於高性能需求，可用 CAS 打造 lock-free 資料結構。
- 根據 contention 程度選擇工具：

  - **Low contention** → spinlock
  - **Moderate contention** → mutex
  - **High contention / 多資源控制** → semaphore or monitor

---

## 第七章：同步範例（Synchronization Examples）

### 🌟 一貫性主軸

- 應用第六章介紹的同步工具於經典問題與實作（如 semaphore、mutex、monitor、condition variable）。
- 並探索 Linux、Windows、POSIX 與 Java 等實作方式與替代方案（如 OpenMP、Functional Programming）。

---

### 🔁 7.1 經典同步問題（Classic Problems of Synchronization）

#### 🔸 7.1.1 有界緩衝區問題（Bounded-Buffer Problem）

- 生產者與消費者共享 n 個 buffer：

  - `semaphore mutex = 1`：互斥存取緩衝區。
  - `semaphore empty = n`、`full = 0`：追蹤空與滿的 buffer 數。

- `producer`: `wait(empty) → wait(mutex) → add → signal(mutex) → signal(full)`
- `consumer`: `wait(full) → wait(mutex) → remove → signal(mutex) → signal(empty)`

#### 🔸 7.1.2 讀者-寫者問題（Readers–Writers Problem）

- `rw_mutex`: 控制寫入互斥。
- `mutex`: 控制 `read_count` 修改的互斥。
- 第一類問題：允許多個 reader 併發，但 writer 需獨占。
- 可能造成飢餓（starvation）：第一類可能使 writer 餓死，第二類可能使 reader 餓死。

#### 🔸 7.1.3 哲學家進餐問題（Dining-Philosophers Problem）

- 使用 semaphore 陣列 `chopstick[5]`，初始為 1。
- 哲學家：`wait(left) → wait(right) → eat → signal(left) → signal(right)`
- 可能 deadlock：若所有人先拿左邊筷子。
- 解法：

  - 最多允許四位同時進餐。
  - 必須一次拿兩支筷子（進入 critical section 再拿）。
  - 奇數哲學家先拿左筷，偶數先拿右筷（asymmetric）。

#### Monitor 解法：

- 狀態為 `{THINKING, HUNGRY, EATING}`。
- 每個哲學家用 `self[i]` condition 變數等待。
- 僅當兩邊鄰居不在吃飯時，才可進入 `EATING`。

---

### ⚙️ 7.2 核心同步機制（Synchronization within the Kernel）

#### 🔹 Windows

- 單核心：關閉中斷。
- 多核心：使用 spinlock。
- 用 Dispatcher Object（如 mutex, semaphore, event, timer）處理用戶同步。
- 狀態：signaled（可獲得）、nonsignaled（需等待）。
- Critical Section Object：user-mode lock，先用 spin，超時才進入 kernel mutex。

#### 🔹 Linux

- 支援 preemptive kernel（2.6 起）。
- 基礎同步方式：

  - `atomic_t` 原子變數操作。
  - `mutex_lock()` / `mutex_unlock()`。
  - `spinlock`：短時間鎖。
  - `semaphore`：長時間同步。

- 單核：用 disable/enable preemption 代替 spinlock。
- 所有鎖皆為 non-recursive。

---

### 🔧 7.3 POSIX 同步工具（POSIX Synchronization）

#### Mutex

- `pthread_mutex_init()`、`pthread_mutex_lock()`、`pthread_mutex_unlock()`。
- 阻塞式：若無法取得會等待。

#### Semaphore

- Named (`sem_open()`)、Unnamed (`sem_init()`)，皆用 `sem_wait()`、`sem_post()`。

#### Condition Variable

- 與 mutex 一起使用。
- `pthread_cond_wait()` 會釋放 mutex 並等待 signal。
- `pthread_cond_signal()` 喚醒等待中的某個 thread。
- 使用時需放入 while loop 檢查條件是否滿足。

---

### ☕ 7.4 Java 同步機制（Java Synchronization）

#### Monitor

- `synchronized` method/block：自動取得物件鎖。
- `wait()` / `notify()` 操作：

  - `wait()`：釋放鎖、進入 wait set。
  - `notify()`：喚醒 wait set 中任一 thread，移入 entry set。

#### ReentrantLock

- 明確控制 lock/unlock（try-finally 保證解鎖）。
- `ReentrantReadWriteLock` 支援多 reader，一 writer。

#### Semaphore

- `Semaphore(n)`，`acquire()` / `release()`。
- 用 `try-finally` 包裹確保 release。

#### Condition

- `lock.newCondition()`，搭配 `await()` / `signal()`。
- 可設計具條件識別的 wait/signal（比內建 `wait/notify` 精細）。

---

### 🧠 7.5 替代性方案（Alternative Approaches）

#### Transactional Memory

- `atomic{}` 區塊可自動保證原子性。
- 軟體（STM）或硬體（HTM）實作，解決 deadlock 與 contention。

#### OpenMP

- `#pragma omp parallel`：產生多 thread。
- `#pragma omp critical`：保護 critical section，防 race condition。

#### Functional Programming

- 例：Erlang, Scala。
- 特性：無 mutable state，自然避免 race condition 與 deadlock。

---

### ✅ 小結

- 經典同步問題提供開發與分析平台。
- 多作業系統支援多種同步機制以平衡效能與安全性。
- Java 與 POSIX 提供豐富 API 處理多執行緒協作。
- 新興方法如 Transactional Memory 與函數式語言挑戰傳統同步設計方式。

---

## 第八章：Deadlocks

### 8.1 System Model 系統模型

- 系統中資源有限，可分為多種資源型別，每型別有多個實例。
- 資源使用流程：**Request → Use → Release**
- 多執行緒請求資源若未被立即滿足則等待。若所有等待互相依賴，則會產生**死結（deadlock）**。

---

### 8.2 Deadlock in Multithreaded Applications 多執行緒死結範例

- POSIX Mutex 例子說明：

  - Thread 1：鎖住 `first_mutex` 再鎖 `second_mutex`
  - Thread 2：鎖住 `second_mutex` 再鎖 `first_mutex`
  - 若兩者交錯執行則可能陷入死結

- **Livelock 活鎖**：執行緒持續嘗試但皆失敗，導致無實際進展（如 hallway 一直讓路的例子）

---

### 8.3 Deadlock Characterization 死結條件與圖模型

#### 8.3.1 四個必要條件：

1. **Mutual exclusion**：資源為非共享。
2. **Hold and wait**：持有資源後仍等待其他資源。
3. **No preemption**：資源不能被強行回收。
4. **Circular wait**：形成一個環狀等待。

#### 8.3.2 Resource Allocation Graph 資源分配圖

- 節點為 Thread / Resource
- 邊：

  - Thread → Resource（request edge）
  - Resource → Thread（assignment edge）

- 若圖中**無環**則無死結；有環則「可能」死結，若每資源型別僅一實例，則**一定**是死結。

---

### 8.4 Methods for Handling Deadlocks 處理死結的方法

1. **Ignore**：最常見做法（如 Linux/Windows）
2. **Prevention / Avoidance**：防止系統進入死結狀態
3. **Detection & Recovery**：進入死結後偵測與恢復

---

### 8.5 Deadlock Prevention 死結預防：破壞 4 個條件之一

1. **Mutual Exclusion**：

   - 無法避免，因某些資源天生非共享

2. **Hold and Wait**：

   - 必須一次請求所有資源，或請求新資源前釋放所有已持有者（導致低資源利用率與飢餓）

3. **No Preemption**：

   - 若資源無法立即取得，則釋放持有資源並等待全部資源可用後再一次分配（難以應用於 lock/semaphore）

4. **Circular Wait**：

   - 強制資源依照某固定順序請求，例如先鎖 `mutex1` 再鎖 `mutex2`（使用排序函數強化順序性）

---

### 8.6 Deadlock Avoidance 死結避免：Banker's Algorithm 與安全狀態

#### 8.6.1 Safe State 安全狀態

- 系統可依序分配，使所有執行緒能完成後釋放資源 → 安全序列存在

#### 8.6.2 Resource-Allocation-Graph Algorithm

- 新增 **Claim edge**（虛線）表示未來可能請求
- 若將 Claim edge 轉為 Assignment edge 導致形成環，則該請求不得執行

#### 8.6.3 Banker's Algorithm 銀行家演算法

- 執行緒先聲明最大需求量
- 檢查資源分配後是否仍處於安全狀態，否則不予分配
- 使用 `Available`, `Max`, `Allocation`, `Need` 四個資料結構實作
- 安全性與資源請求檢查以保證系統不進入死結

---

### 8.7 Deadlock Detection 死結偵測

#### 8.7.1 單一資源實例：Wait-For Graph 等待圖

- 若圖中存在 cycle 則代表死結
- Linux/BCC/Java 提供工具支援 deadlock 偵測

#### 8.7.2 多資源實例：類似 Banker's Algorithm

- 使用 `Available`, `Allocation`, `Request` 三個矩陣
- 執行 Safety Check 判斷是否能完成所有程序

#### 8.7.3 Detection Algorithm Usage

- 可定期執行或在 CPU 利用率低時執行
- 檢查頻率需與死結發生頻率與影響數量平衡

---

### 8.8 Recovery from Deadlock 死結恢復

#### 8.8.1 Process Termination

- 終止所有死結執行緒（代價高）
- 一次終止一個直到解除死結（需重複偵測）

#### 8.8.2 Resource Preemption

- 暫時回收部分資源，再重新排程執行緒
- 涉及 rollback（例如資料庫的 transaction 回復）
- 評估：選擇受影響小的「victim」執行緒中止或資源回收

---

## 第九章 Main Memory 筆記

### 🎯 章節目標

- 理解邏輯位址與實體位址的差異與轉換（MMU）
- 掌握三種記憶體分配法：first-fit、best-fit、worst-fit
- 理解 internal / external fragmentation
- 掌握 paging 架構及其轉換方式（含 TLB）
- 認識 IA-32、x86-64、ARMv8 的記憶體轉譯方式（概略）

---

### 9.1 背景（Background）

#### 📌 基本硬體架構

- CPU 僅能直接存取 register 與 main memory（無法直接存 disk）
- 加入 cache 以解決 memory latency 問題
- 使用 **base 和 limit register** 提供記憶體保護（防止 user 程式誤存 OS）

#### 📌 Address Binding 三種方式

1. **Compile Time**：絕對位址，須重新編譯
2. **Load Time**：產生 relocatable code
3. **Execution Time**：使用 MMU，執行時動態轉換（最常見）

#### 📌 Logical vs Physical Address

- **Logical Address**（程式看到） vs **Physical Address**（記憶體單元實際位址）
- 執行時轉譯由 **MMU** 處理，透過 relocation register 完成 offset 加法

#### 📌 Dynamic Loading & Linking

- **Dynamic Loading**：函式被呼叫時才載入，節省記憶體
- **Dynamic Linking**：執行時連結 shared libraries（如 libc.so），可多程式共用

---

### 9.2 連續記憶體配置（Contiguous Memory Allocation）

#### 📌 分配方式

- 記憶體分為 OS 區與使用者區
- 使用 **base + limit register** 保障記憶體邊界

#### 📌 配置策略

- **First-Fit**：從前面找到第一個可用洞
- **Best-Fit**：找最小剛好的洞（減少浪費）
- **Worst-Fit**：找最大洞（保留較大剩餘）

#### 📌 碎片化（Fragmentation）

- **External Fragmentation**：空間夠但不連續
- **Internal Fragmentation**：分配空間 > 實際需求（如差幾 bytes）

#### 📌 解決方式

- **Compaction**：搬移資料合併空洞（需動態 relocation）
- **非連續分配方式（如 Paging）**：避免碎片問題

---

### 9.3 分頁（Paging）

#### 📌 基礎方法

- 實體記憶體 → frame；邏輯記憶體 → page；兩者大小相同
- **頁表（page table）** 轉換邏輯位址為實體位址
- Logical Address = `p` (page number) + `d` (offset)
- Physical Address = page table 中對應的 frame + `d`

#### 📌 特性

- **無 External Fragmentation**
- 有可能出現 **Internal Fragmentation**（最後一頁未用滿）

#### 📌 TLB（Translation Look-Aside Buffer）

- 為加速頁表查詢，使用高速快取（TLB）
- **TLB hit** → 快速查得 frame
- **TLB miss** → 查 page table 並更新 TLB
- 使用 ASID（Address Space ID）支援多程序分離
- 命中率越高，效能越接近非分頁

#### 📌 保護機制

- 每頁設置 **權限 bit**（read/write/execute）
- **Valid–Invalid bit**：標記頁面是否為合法範圍

#### 📌 共用頁面（Shared Pages）

- 可將不變的程式碼如 libc 設為 reentrant，供多程序共用（節省記憶體）

---

### 9.4 頁表結構（Structure of the Page Table）

#### 📌 分層頁表（Hierarchical Paging）

- 解決 32-bit 或 64-bit 空間下 page table 過大問題
- **Two-level paging**：Page Table 也可分頁（例：10-10-12 分法）
- **64-bit** 通常需多層（甚至 > 4-level paging）

#### 📌 Hashed Page Table（適用 64-bit 空間）

- 使用雜湊表查找頁面，解決 sparse 空間問題
- **Clustered page table**：一筆可對應多頁（如 16 頁）

---

### 第十章 Virtual Memory 筆記

#### 10.1 Background

- 虛擬記憶體讓程式可使用比實體記憶體更大的空間。
- 虛擬與實體記憶體之間需透過映射與管理（通常由 OS 處理）。

#### 10.2 Demand Paging

- **基本概念**：頁面僅在需要時才載入（lazy loading）。
- **Free-frame list**：記錄可用的實體頁框。
- **效能考量**：page fault rate 影響整體效能；低缺頁率是目標。

#### 10.3 Copy-on-Write (COW)

- 多個程序共享同一記憶體副本，直到某個程序嘗試寫入，才複製該頁面。

#### 10.4 Page Replacement

- **基本策略**：找一頁替換以騰出空間。
- **FIFO**：先進先出，可能產生 Belady’s anomaly。
- **Optimal**：理想但不可實作，僅供理論比較。
- **LRU**：最近最少使用，接近 optimal。
- **LRU 近似法**：Clock 算法。
- **計數法**：以參考次數選擇替換頁，效果有限。
- **Page buffering**：保留被替換頁於 buffer 中，利於快速回復。
- **應用情境**：不同應用可選擇合適策略。

#### 10.5 Allocation of Frames

- **最小框數**：根據指令數與需求，避免死鎖。
- **配置算法**：

  - 固定配置（equal/proportional）
  - 可變配置（global/local）

- **NUMA 架構**：考量非一致記憶體存取時間。

#### 10.6 Thrashing

- **原因**：過多 page fault 導致 CPU 大量時間在處理記憶體。
- **工作集模型**：限制活躍頁集大小來避免 thrashing。
- **Page-fault frequency (PFF)**：以缺頁頻率作為調整依據。
- **實務中**：多數系統會使用混合策略進行動態調整。

#### 10.7 Memory Compression

- 利用壓縮演算法將部分記憶體頁壓縮存放，降低 page fault。

#### 10.8 Kernel Memory Allocation

- **Buddy system**：以 2 的倍數分配記憶體，利於合併與拆分。
- **Slab allocation**：針對相同類型物件的記憶體分配，提升效率與快取命中率。

#### 10.9 Other Considerations

- **Prepaging**：預先載入未來可能用到的頁面。
- **頁面大小**：影響內部碎片與 page fault 頻率。
- **TLB reach**：TLB 可涵蓋的記憶體範圍，影響效能。
- **Inverted page table**：減少頁表大小，以 frame 為主鍵查詢。
- **程式結構影響記憶體存取模式**。
- **I/O 鎖定**：防止進行 I/O 的頁面被置換。

#### 10.10 OS 實例

- **Linux**：使用 slab allocator、LRU 和其他機制。
- **Windows**：採用 working set、page-fault frequency 管理。
- **Solaris**：支援 page coloring 以最佳化 cache。

#### 10.11 Summary

- 虛擬記憶體結合需求載入與頁面替換策略，有效提高效率與多工。
- 必須防止 thrashing 並動態調整頁框分配。

---

## 第十一章：Mass-Storage Structure

### 11.1 Mass-Storage 概觀

- 現代主要儲存設備為 HDD（機械硬碟）與 NVM（非揮發性記憶體，如 SSD）。
- 以 Logical Block Addressing (LBA) 將邏輯地址對映至物理裝置的 sector/page。

#### 11.1.1 HDD 結構與效能

- 結構：由 platters、spindle、track、sector、arm 組成。
- 效能瓶頸：Seek Time + Rotational Latency → 影響隨機存取時間。
- 備註：可能會發生 head crash，導致資料永久損毀。

#### 11.1.2 NVM（如 SSD）

- 優點：無移動元件，無 seek/latency，低耗能。
- 缺點：較貴、容量較小、壽命有限（寫入次數有限）。
- 使用 Flash Translation Layer (FTL) 進行 logical-physical 映射。
- 需考慮：Garbage Collection、Wear Leveling、Over-Provisioning。
- 常用壽命指標：DWPD（Drive Writes Per Day）。

#### 11.1.3 Volatile RAM Drives

- 把 DRAM 虛擬成磁碟（如 `/tmp`、`/dev/ram`）。
- 非揮發性但極快，常用於暫存檔案。

#### 11.1.4 儲存裝置連接方式

- 常見：SATA, USB, NVMe（直連 PCIe 線路）。
- 資料透過主機端控制器（Host Controller）與裝置控制器（Device Controller）溝通。

#### 11.1.5 Address Mapping

- LBA 是邏輯表示，與物理位置不一定一一對應。
- HDD 傳統用 CAV（Constant Angular Velocity）模式；CD/DVD 用 CLV。
- HDD/SSD 會內建 defect 管理與 spare sectors。

---

### 11.2 HDD 排程

- 目標：減少 seek time、增加 bandwidth。
- 排程演算法：

  - FCFS：公平但可能效率低。
  - SCAN：頭移動像電梯來回掃描。
  - C-SCAN：只朝一個方向掃描，另一方向快速返回，等待時間更平均。
  - Linux 常見排程器：Deadline（防飢餓，強調 Read），NOOP（給 SSD）、CFQ（支援 I/O 分類與預測）。

---

### 11.3 NVM 排程

- SSD 無機械延遲，通常用簡單的 FCFS。
- 特性：讀取時間穩定；寫入時間變化大（與 GC/寫放大有關）。
- 寫放大（Write Amplification）：一個寫入可能引發多次實體寫操作。

---

### 11.4 錯誤檢測與修正（ECC）

- Parity Bit：檢查奇偶性（偵錯但不修錯）。
- ECC：可糾錯，例如儲存時加入 checksum，可修復單/雙 bit 錯誤。
- Flash/HDD 都內建 ECC 機制，會自動做錯誤修復與標記壞區。

---

### 11.5 儲存裝置管理

#### 11.5.1 格式化與分割

- Low-Level Formatting：製造時建立 sector/page。
- 分割（Partitioning）：邏輯分區，可分別掛載不同檔案系統或 Swap。
- Volume：可結合多分區（RAID/ZFS）。
- File System Formatting：建立 metadata、目錄、block map 等。
- Raw I/O：不經 FS，效率高但程式需自行管理（如 DB 系統）。

#### 11.5.2 開機區域（Boot Block）

- 開機過程：NVM Firmware ➝ Boot Block ➝ Kernel ➝ OS。
- Windows 使用 MBR（主開機記錄）；Linux 使用 grub2。
- MBR 包含 Partition Table 和 bootable flag。

#### 11.5.3 壞區管理

- Sector Sparing：壞區由備援區塊替換。
- Sector Slipping：邏輯位移所有區塊來跳過壞區。
- SSD 由 Controller 自動管理壞頁。

---

### 11.6 Swap-Space 管理

#### 11.6.1 Swap 用途

- 用於補足主記憶體空間不足。
- 儲存已被置換出的 anonymous memory（如 stack/heap）。
- Linux 支援多個 swap 裝置，可用檔案或 raw partition。

#### 11.6.2 Swap 位置

- 可為 file 或 raw partition。
- raw partition 效能較好，fragmentation 較少。

#### 11.6.3 範例：Linux Swap 管理

- Swap slot 對應 page，並用 swap map 記錄映射數。
- 支援共享記憶體映射（多個 process 對應同一頁）。

---

### 11.7 儲存連接方式

#### 11.7.1 Host-Attached

- 使用 SATA、USB、FireWire 等本地端連接。

#### 11.7.2 NAS（Network-Attached Storage）

- 使用 NFS（UNIX）、CIFS（Windows）協定經由 LAN 存取。
- 可同時被多用戶端使用。

---

## 📘 第 12 章：I/O Systems（輸入輸出系統）

### 🎯 本章目標

- 探索作業系統中 I/O 子系統的結構
- 解釋 I/O 硬體的原理與複雜性
- 說明 I/O 硬體與軟體的效能議題

---

### 🧱 章節結構與重點

#### **12.1 Overview**

- I/O 是電腦的兩大主要任務之一（另一是計算）
- I/O 子系統將核心邏輯與硬體控制隔離
- I/O 發展出「標準化介面」與「多樣化設備」的雙重趨勢

---

#### **12.2 I/O Hardware**

##### 🧩 組成要素

- **Port / Bus / Controller**：裝置透過 port 或 bus 傳輸資料，controller 操作它們
- **記憶體映射 I/O（MMIO）**：控制暫存器映射進主記憶體空間，可用普通讀寫指令控制設備

##### 🔁 資料傳輸方法

- **Polling（輪詢）**：CPU 持續讀取狀態暫存器直到設備可用，效率低
- **Interrupt（中斷）**：設備就緒時主動通知 CPU，提高效率

  - 支援：優先權、中斷向量表、分層處理（FLIH/SLIH）
  - 特例：**Trap** 是程式主動觸發的中斷（如系統呼叫、page fault）

- **DMA（Direct Memory Access）**：大型傳輸由 DMA 控制器直接處理，不經 CPU

  - 可搭配 scatter-gather 支援非連續記憶體區段

---

#### **12.3 Application I/O Interface**

##### 🔧 抽象層設計

- 把設備分為標準類型（如 block/character）
- 每個設備由 **device driver** 封裝，對 kernel export 標準操作介面

##### 📦 標準介面類型

- **Block device**：支援 read/write/seek，常為磁碟

  - 可支援 raw I/O（繞過作業系統 buffering/locking）
  - 可搭配 memory-mapped I/O

- **Character device**：逐字元輸入輸出，如鍵盤/滑鼠
- **Network device（socket interface）**：用於通訊

  - select() 支援多 socket 非阻塞檢查

- **Timer/Clock**：支援時間查詢與定時器中斷

  - 使用 HPET、NTP 校正漂移

- **Nonblocking / Asynchronous I/O**：

  - Nonblocking：立刻返回，有資料則回傳
  - Asynchronous：非同步操作，透過 callback 通知

- **Vectored I/O（scatter-gather）**：一次傳多筆資料，提高效率

---

#### **12.4 Kernel I/O Subsystem**

##### ⛓ 作業系統支援功能

- **Scheduling**：根據裝置與作業優先權排程（如磁碟 I/O 排程）
- **Buffering**：緩衝區用途：

  1. 解決傳輸速率不對稱
  2. 對齊傳輸單位
  3. 實現 copy semantics（防止程式修改原始資料）

- **Caching**：快取資料，提高存取效率
- **Spooling**：佇列列印等獨佔設備作業，避免資料混淆
- **Device Reservation**：支援設備鎖定，避免 deadlock
- **Error Handling**：如 errno、SCSI 錯誤分級回報
- **I/O Protection**：

  - 限定 I/O 指令為特權操作，需透過系統呼叫進行
  - 防止惡意或錯誤程式擾亂設備

---

### 🔗 一貫性思維

- **分層抽象設計**：從設備硬體到 kernel，再到 user app，層層抽象與封裝
- **效率與保護兼顧**：

  - 效率：DMA、interrupt、buffer、cache、nonblocking I/O
  - 安全：特權指令、中斷保護、錯誤重試與管理

- **多樣性統一化**：透過 device driver 模型統一不同硬體控制介面

---

### 第十三章 File-System Interface

#### 13.1 File Concept

- 檔案是邏輯儲存單位，由 OS 抽象化處理底層儲存裝置。
- 資訊種類：程式碼、文字、影像、音訊等。
- 檔案內容結構由創建者定義，可為字元串、紀錄、位元等序列。
- UNIX/Linux 提供 `/proc` 檔案系統以檔案介面讀取系統資訊。

#### 13.1.1 File Attributes

- 屬性：名稱、識別碼、型態、位置、大小、保護權限、時間戳記等。
- 儲存於目錄結構中，依據檔案系統可能高達 KB 級容量。
- 新式檔案系統支援擴充屬性（如編碼格式、checksum）。

#### 13.1.2 File Operations

- 基本操作：

  - create：配置空間並加入目錄。
  - open：取得檔案控制代碼。
  - write/read：寫入/讀取並更新指標。
  - seek：調整指標（不一定觸發 IO）。
  - delete：釋放空間並移除目錄項。
  - truncate：清空內容但保留屬性。

- 開啟檔案後記錄於 open-file table（分為 per-process / system-wide）。
- Java 示例：`FileChannel.lock()` 支援共享/獨佔鎖定。
- 鎖定機制：

  - Mandatory（Windows）：OS 強制封鎖其他進程存取。
  - Advisory（UNIX）：靠開發者自願遵守鎖定規則。

#### 13.1.3 File Types

- 型態透過副檔名（.c, .java, .mp4 等）或 metadata 判定。
- UNIX 支援 magic number。
- macOS 有 creator attribute（指定創建程式）。

#### 13.1.4 File Structure

- OS 需支援可執行檔格式，其餘留給應用層解析。
- UNIX 檔案為純 byte stream，提供最大彈性但最低支援。

#### 13.1.5 Internal File Structure

- 磁碟以固定大小 block 為單位存取，造成 internal fragmentation。
- Logical record 可由 OS 或應用程式自行處理封裝與 unpack。

---

#### 13.2 Access Methods

- **Sequential Access**：

  - 最常見，逐一處理紀錄（如 editor / compiler）。
  - 操作為 read next / write next。

- **Direct Access**：

  - 記錄以固定長度儲存，可任意指定區塊讀寫。
  - 常用於資料庫等隨機存取系統。

- **Other Access Methods**：

  - 建立 index 以輔助搜尋（如 binary search + index block）。
  - ISAM（Indexed Sequential Access Method）透過 primary/secondary index 快速定位資料區塊。

---

#### 13.3 Directory Structure

##### 13.3.1 Single-Level Directory

- 所有檔案於同一層，名稱需唯一，無法區分使用者或主題。

##### 13.3.2 Two-Level Directory

- 每位使用者有獨立的 UFD（User File Directory），MFD 作為根目錄。
- 容許不同使用者擁有相同檔名，支援基本隔離但不利共享。

##### 13.3.3 Tree-Structured Directory

- 支援多層子目錄，擴展使用者自定分類能力。
- 檔案以絕對或相對路徑命名，如 `/user/docs/report.txt`。
- 可使用 `rm -r` 刪除整個子樹，風險較高。

##### 13.3.4 Acyclic-Graph Directory

- 支援檔案/目錄共享（例如兩人共用同一專案目錄）。
- UNIX 以 symbolic link 或 hard link 實現。
- 採 reference count 判定是否能刪除實體檔案。

##### 13.3.5 General Graph Directory

- 容許有向循環（cycle），但需防止 traversal 無限迴圈。
- 通常需 garbage collection 演算法處理無法到達的檔案。
- 多數系統避免此種設計以控制複雜性。

---

#### 13.4 Protection

##### 13.4.1 Types of Access

- 權限控制類型：read、write、execute、append、delete。
- 僅允許授權行為，避免惡意或意外破壞檔案。

---

## 第十四章：File-System Implementation

### 14.1 File-System Structure

- 檔案系統分層結構：I/O control → Basic file system → File-organization module → Logical file system。
- 使用 block 為單位進行磁碟與記憶體之間的資料傳輸。
- Logical File System 管理 metadata，包括：

  - File Control Block（FCB, inode）包含擁有者、權限、內容位置等。

- 基本檔案系統透過邏輯區塊位址進行 I/O。
- 檔案組織模組管理邏輯區塊與 free-space。
- 支援多種檔案系統（如 ext4, FAT32, NTFS），以適應不同效能、穩定性需求。

### 14.2 File-System Operations

- **開啟檔案(open)**：

  - 檢查是否已在 system-wide open-file table；
  - 若無則從磁碟載入 FCB，建立對應表項；
  - 建立 per-process open-file table 條目。

- **關閉檔案(close)**：

  - 移除 process 對應的 open-file entry；
  - 若所有 process 都關閉此檔案，則將更新後的 metadata 寫回磁碟。

- 開檔後檔名可能不再儲存，只用檔案描述符 (file descriptor) 或檔案控制碼 (file handle) 來操作。

### 14.3 Directory Implementation

- **Linear List**：簡單但搜尋慢；可使用排序或 linked list 改善刪除效率。
- **Hash Table**：加速搜尋；需處理碰撞與重哈希問題。

### 14.4 Allocation Methods

- **Contiguous Allocation**：

  - 檔案需連續配置區塊；
  - 優點：順序與直接存取效率高；
  - 缺點：外部碎裂、預估大小困難、延伸困難。

- **Linked Allocation**：

  - 檔案由指標連結的分散區塊組成；
  - 優點：無外部碎裂；
  - 缺點：無法直接存取，效率差。
  - FAT（File Allocation Table）是一種特例，把指標集中於 FAT 中。

- **Indexed Allocation**：

  - 每檔案有一 index block 儲存所有區塊的指標；
  - 支援直接存取；
  - 常見變體：

    - Linked index block（連結多個 index block）
    - Multi-level index（多級 index）
    - Combined inode（UNIX 使用 direct + single/double/triple indirect）

- **Performance**：

  - Contiguous 適合小檔/隨機存取；
  - Linked 適合順序存取；
  - Indexed 適合混合存取但 metadata 成本高。

### 14.5 Free-Space Management

- **Bit Vector**：

  - 每個 block 用 1 bit 表示是否空閒；
  - 優點：尋找快速；
  - 缺點：需大量記憶體儲存 bitmap。

- **Linked List**：每個空 block 連到下一個，效能差但實作簡單。
- **Grouping**：每個 free block 儲存下一組 n 個空 block 的位址。
- **Counting**：每個 entry 儲存第一個空 block 的位址與後續連續空 block 數量。
- **Space Maps**（ZFS）：

  - 分成 metaslab，每個用 log + balanced tree 管理空間；
  - 使用類似 write-ahead log 的方式記錄 allocate/free 操作。

- **TRIM/Unallocate**：

  - 對於不能覆寫的 NVM（如 SSD），使用 TRIM/unallocate 告知可擦除區塊以避免 write cliff。

### 14.6 Efficiency and Performance

- **效率**：

  - 指標大小影響最大可支持檔案大小與 metadata 成本（32-bit 限 4GB；ZFS 用 128-bit）。
  - UNIX 預先配置 inode，有利於分散與效能。

- **效能**：

  - 使用 Page Cache 或 Unified Buffer Cache 提升 I/O 效能；
  - 避免 double caching（read/write 與 mmap 使用不同 cache）；
  - Solaris 改進：priority paging, capped process/cache memory。
  - **Write 策略**：可選 synchronous（立即寫入）或 asynchronous（先寫入 cache 再同步）。
  - **預讀/後清策略**（read-ahead / free-behind）：適用順序 I/O 以提升效能。

---

### 第十五章：File-System Internals

#### 📁 15.1 檔案系統

- 作業系統通常支援多種檔案系統（如 UFS、ZFS、tmpfs、procfs）。
- 磁碟可分為多個分割區（partitions），每個分割區可承載不同類型的檔案系統。
- Solaris 範例展示多種檔案系統（虛擬、暫存、通用等）。

#### 🔗 15.2 檔案系統掛載（Mounting）

- 檔案系統使用前需掛載（mount）至目錄結構中的某一位置（稱為掛載點）。
- 掛載時需驗證設備是否含有效的檔案系統。
- UNIX 系統可在任意空目錄掛載，Windows 系統則使用磁碟代號（如 `D:\`）。
- 掛載的文件系統可覆蓋原本目錄內容（被遮蔽）。

#### 🧩 15.3 分割與掛載

- 分割區可為 raw（未格式化）或 cooked（含檔案系統）。
- bootable 分割區需含有開機載入器（bootstrap loader），可從中載入核心（kernel）。
- 開機時掛載根目錄分割區，其餘檔案系統可自動或手動掛載。

#### 👥 15.4 檔案共享

- 多使用者作業系統需支援共享與保護。
- 利用檔案屬性中的擁有者（owner）與群組（group）進行存取控制。
- 跨系統共享需注意 UID/GID 是否一致。

#### 🧱 15.5 虛擬檔案系統（VFS）

- 抽象層：讓 OS 可支援多種檔案系統，提供統一接口。
- 使用 vnode 表示檔案或目錄，可唯一標識網路中的檔案。
- Linux VFS 四種核心結構：

  - `inode`：檔案
  - `file`：開啟的檔案
  - `superblock`：整體檔案系統
  - `dentry`：目錄項目

- 每種物件都有函數指標表，實作如 `open()`, `read()`, `write()`。

#### 🌐 15.6 遠端檔案系統（Remote File Systems）

- DFS：允許跨機器共享檔案。
- NFS：常見的網路檔案系統。可掛載遠端資料夾至本地系統。
- 認證問題：如使用 UID 對應，存在 spoofing 風險。
- Windows 使用 CIFS 與 Active Directory（LDAP + Kerberos）管理認證。

#### ⚠️ 15.6.3 錯誤模式（Failure Modes）

- 本地錯誤：磁碟損壞、metadata 損壞等。
- 遠端錯誤：網路斷線、伺服器當機需支援延遲操作（delay operations）。
- NFS v3 採無狀態（stateless），但 v4 引入狀態支援安全性與性能。

#### 🔄 15.7 一致性語意（Consistency Semantics）

- **UNIX 語意**：檔案寫入即時對其他使用者可見。
- **Session 語意**（如 Andrew）：寫入在關閉檔案後才對其他人可見。
- **不可變共享檔案語意**：檔案宣告共享後即無法修改。

#### 📡 15.8 NFS（Network File System）

- 客戶端-伺服器架構。透過 mount protocol 與 NFS protocol 提供服務。
- **Mount protocol**：建立連線、檢查 export list。
- **NFS protocol**：支援檔案查詢、讀寫、屬性操作等。
- 特性：

  - 無狀態（stateless）：不保留用戶端資訊，每次請求需包含完整資料。
  - 檔案操作需 idempotent（重複操作效果相同）。
  - 支援 cascading mount（多層掛載）。
  - 使用快取提升效能（attributes 與 blocks cache）。

#### 📌 15.9 小結

- 檔案系統支援多類型與遠端掛載。
- VFS 提供抽象化與統一操作介面。
- 遠端共享需處理安全、一致性與錯誤恢復。
- NFS 是重要案例，展示無狀態 DFS 的實作與限制。

---

## 📘 Chapter 16：Security（安全性）

### 🎯 核心目標

- 分析電腦系統可能面臨的**安全威脅與攻擊類型**
- 介紹**密碼學**作為核心安全工具
- 探討**身份驗證與防護機制**
- 提供**操作系統中實作安全防禦的多元方法**

---

### 🔐 16.1 The Security Problem（安全性問題）

- **安全性**：系統在各種情況下維持資源正確使用的能力。
- **資源**：資料、程式、CPU、記憶體、磁碟、網路等。
- **威脅來源**：惡意攻擊者、競爭對手、內部人員失誤。
- **目標**：讓安全事件成為極少數的例外，而非常態。

---

### 💣 16.2 Program Threats（程式威脅）

- **Malware**（惡意軟體）：如病毒、蠕蟲、木馬。
- **Code Injection**（程式注入）：如 buffer overflow，允許執行攻擊者控制的程式。
- **Viruses/Worms**：透過複製與自我傳播破壞系統。

---

### 🌐 16.3 System and Network Threats（系統與網路威脅）

- **攻擊類型**：

  - 攔截/修改網路資料（sniffing, spoofing）
  - **DDoS（分散式阻斷服務）攻擊**
  - **Port Scanning**：掃描開放端口找漏洞。

---

### 🔐 16.4 Cryptography as a Security Tool（密碼學作為安全工具）

- **Encryption（加密）**：使用金鑰保護資料。
- **Hashing**：不可逆地將資料轉為摘要，用於完整性檢查。
- **TLS（傳輸層安全協定）**：網路通信加密範例。

---

### 👤 16.5 User Authentication（使用者認證）

- **Passwords**：最常見但易受攻擊。
- **漏洞**：重複使用、猜測、暴力破解。
- **強化機制**：

  - 雜湊儲存、鹽值(salt)
  - OTP（一次性密碼）
  - **生物特徵驗證（biometrics）**：如指紋、臉部辨識。

---

### 🛡️ 16.6 Implementing Security Defenses（實作防禦機制）

- **Security Policy（政策）**：定義什麼是允許與禁止。
- **Vulnerability Assessment（漏洞評估）**
- **Intrusion Prevention（入侵預防系統）**
- **Virus Protection（病毒防護）**
- **Logging & Auditing（日誌與審計）**
- **Firewall（防火牆）**：防止未授權存取。
- **其他防禦**：如應用白名單、限制使用者權限。

---

### 🖥️ 16.7 Case Study: Windows 10

- **BitLocker**：全磁碟加密。
- **Windows Defender**：內建防毒與入侵防禦。
- **User Account Control (UAC)**：限制未授權操作。
- **SmartScreen**：過濾惡意應用與網站。

---

### 📌 16.8 Summary（總結）

- 完善的電腦安全需要**多層防禦**。
- 沒有單一方法可以完全防範攻擊，需整合密碼學、使用者驗證、系統保護與偵測機制。
- 強調**預防 > 偵測 > 回應**的循環。

---

### 🧠 一貫性思維整理

| 構面         | 思維重點                                  |
| ------------ | ----------------------------------------- |
| 威脅識別     | 攻擊者類型、程式漏洞、網路威脅            |
| 預防機制     | 加密、認證、防火牆、惡意軟體掃描          |
| 使用者行為   | 密碼強度與管理、操作系統權限控制          |
| 系統實作     | 定期更新、防火牆規則、資安政策制定與落實  |
| 機制整合     | TLS、hash、身份驗證、多因素驗證、生物辨識 |
| 實際案例學習 | Windows 10 實作的完整防禦機制提供實用範例 |

---

### 第十七章 Protection

#### 17.1 保護的目標

- 保護系統資源避免未授權、惡意或錯誤使用。
- 增加系統可靠性，早期偵測錯誤、防止子系統間污染。
- 保護機制（mechanism）與政策（policy）應分離，以提升彈性。

#### 17.2 保護原則

- **最小權限原則**（Least Privilege）：只給予完成任務所需最小權限。
- **區隔原則**（Compartmentalization）：子系統相互隔離，避免權限擴散。
- **防禦深度**：多層防禦，降低單點攻擊成功率。
- **審計追蹤**：系統記錄所有異常存取，便於追查。

#### 17.3 Protection Rings

- 不同環境下分為不同「環」(Ring)：Ring 0 為 kernel，Ring 3 為 user。
- ARM TrustZone 為一種更高等級的安全執行環境（EL3），可儲存加密金鑰。
- 特殊指令如 syscall、SMC 用來從低權限環切換到高權限環。

#### 17.4 保護網域（Domain of Protection）

- 保護單位可為「使用者、行程、函式」等。
- 每個 Domain 是一組存取權限 `<object, rights-set>`。
- 支援動態切換 domain，實現最小必要權限。
- 範例：

  - UNIX 利用 `setuid` 執行 root 權限的特定程式。
  - Android 為每個應用程式分配唯一 UID/GID 實現隔離。

#### 17.5 存取矩陣（Access Matrix）

- 橫列代表 domain，直欄代表 object，交叉點定義權限。
- 支援三種特殊權限管理：

  - `copy`: 可複製權限。
  - `owner`: 可新增/移除欄位權限。
  - `control`: 可變更行的權限（domain 對 domain）。

#### 17.6 實作方式

- **Global Table**：簡單但效率差。
- **Access List for Objects**：物件導向，每個物件列出擁有者和權限。
- **Capability List for Domains**：domain 導向，每個 domain 擁有存取 token。
- **Lock–Key 機制**：物件有 lock，domain 有 key，key-lock 相符才能存取。

#### 17.7 權限撤銷（Revocation）

- 對 capability 系統的撤銷較困難，可使用：

  - reacquisition、back-pointer、indirection、key-based 等機制。

- 可區分立即 vs 延遲、部分 vs 全部、永久 vs 暫時撤銷。

#### 17.8 角色為本的存取控制（RBAC）

- Solaris 10 起引入：權限分配給角色，使用者根據需要切換角色。
- 符合最小權限原則，減少使用 root 權限的風險。

#### 17.9 強制存取控制（MAC）

- 比 DAC 更嚴格，限制 root 用戶權限。
- MAC 透過標籤與政策檢查 subject 是否能對 object 執行操作。
- SELinux、TrustedBSD、Windows MIC 等皆實作 MAC。

#### 17.10 現代 capability 系統

- **Linux Capabilities**：將 root 權限細分為多個位元，可逐項移除。
- **Darwin Entitlements**：以 XML 宣告權限，嵌入 code signature，防止偽造。

#### 17.11 其他保護增強方式

- **System Integrity Protection (SIP)**：macOS 限制即使 root 也不能修改系統檔案。
- **System Call Filtering**：如 SECCOMP-BPF，針對 syscall 加上條件檢查。
- **Sandboxing**：隔離應用程式行為，例如 iOS/macOS 的 Seatbelt。
- **Code Signing**：程式必須被簽章驗證，否則無法執行。

---

## 第十八章：Virtual Machines（虛擬機）

### 🎯 章節目標

- 探討虛擬機的歷史與效益
- 比較不同類型的虛擬機技術
- 說明虛擬化實作方式
- 分析支援虛擬化的硬體特徵
- 簡介虛擬化的前沿研究

---

### 18.1 概述

- 虛擬化的核心概念是將一台實體機器抽象成多個「虛擬執行環境」
- VMM（Virtual Machine Manager，也稱 Hypervisor）管理虛擬機，提供硬體抽象接口給 guest OS
- 系統模型分為非虛擬（單核心/單 Kernel）與虛擬（多 Kernel/多 VM）

---

### 18.2 歷史

- 1972 年 IBM VM/370 系統首次引入虛擬化（使用 minidisks 虛擬多台系統）
- 定義虛擬化三原則：**Fidelity（忠實）、Performance（效能）、Safety（安全）**
- 1990 年代末，x86 處理器逐漸支援虛擬化（如 VMware、Xen）

---

### 18.3 效益與特色

- **隔離性佳**：guest OS 不易互相影響，亦難破壞 host
- **支援快照與遷移**：支援 suspend/resume、clone、live migration
- **系統開發友善**：開發者可在 VM 中實驗 OS，而不破壞主系統
- **集中管理與資源最佳化**：一台實體主機可承載多 VM
- **支援應用封裝**：預設環境+App 的 VM 映像可部署至不同主機

---

### 18.4 虛擬化基礎技術

- **Trap-and-Emulate**（陷阱與模擬）
- **Binary Translation**（二進位轉譯）
- **Hardware Assistance**（硬體支援，如 VT-x / AMD-V）
- **Nested Page Tables**（巢狀頁表）與 **VCPU 模擬**

---

### 18.5 虛擬機的類型

- **Type 0**：硬體層級（如 IBM LPAR），Firmware 控制
- **Type 1**：直接於硬體上執行，如 VMware ESX、Xen
- **Type 2**：一般應用程式級，如 VirtualBox、VMware Workstation
- **Paravirtualization**：需修改 guest OS，如早期 Xen
- **Programming Environment Virtualization**：JVM, .NET 類平台
- **Emulation**：跨 CPU 架構模擬（指令轉譯）
- **Application Containment**：像是 Linux containers、Solaris Zones，不完全虛擬化

---

### 18.6 與作業系統的互動

- **CPU 排程**：如何公平分配實體 CPU 給虛擬 CPU
- **記憶體管理**：ballooning、page deduplication、double paging
- **I/O 處理**：專用 vs 共用裝置；使用虛擬裝置與 VT-d / interrupt remapping 改善效能

---

## 📘 第十九章：網路與分散式系統

### 🎯 章節目標

- 說明分散式系統的優點
- 概述連接分散式系統的網路結構
- 定義當前常見分散式系統的角色與類型
- 探討分散式檔案系統設計相關問題

---

### 1️⃣ 分散式系統的優勢

- **資源共享**：可跨站點共享資料庫、檔案、印表機等。
- **運算加速**：將大型計算拆分至多個站點加速執行（如 MapReduce）。
- **可靠性**：部分節點失效不影響整體服務，需設計冗餘與恢復機制。

---

### 2️⃣ 網路結構與類型

- **LAN（區域網路）**：高速度、低錯誤，涵蓋小範圍（如辦公室）。
- **WAN（廣域網路）**：連接遠距站點，依賴路由器與 ISP，速度較慢但範圍廣。

---

### 3️⃣ 通訊與命名機制

- **DNS 名稱解析**：使用分層域名，如 `eric.cs.yale.edu` → IP 位址。
- **OSI 七層模型 vs TCP/IP 四層模型**：

  - OSI：實體、資料鏈結、網路、傳輸、會話、表示、應用
  - TCP/IP：網路、傳輸、應用（具體協議如 TCP、UDP、HTTP）

---

### 4️⃣ 通訊協議：UDP vs TCP

- **UDP**：

  - 無連線、快速但不可靠
  - 適用於影音串流、即時傳輸

- **TCP**：

  - 有連線、可靠、序列控制與重傳
  - 適用於 Web、Email、FTP

---

### 5️⃣ 網路作業系統 vs 分散式作業系統

- **Network OS**：

  - 使用者需手動登入與傳輸（SSH、FTP、SFTP）
  - 無統一介面

- **Distributed OS**：

  - 遠端資源與本地資源透明使用
  - 支援資料/計算/程序移轉（如 RPC、進程遷移）

---

### 6️⃣ 設計挑戰

- **容錯性（Robustness）**：須能偵測失敗、重組架構、恢復節點
- **透明性（Transparency）**：資源位置與使用者移動對使用者應無感
- **可擴展性（Scalability）**：應能隨用戶/設備增加而維持效能

---

## 🧠 一貫性的總體思維：

本章重點在於 **以 Linux 系統作為實例，統整並實作操作系統的核心概念**，涵蓋過去章節所學（如 process、memory、filesystem、I/O、scheduling 等），從設計哲學到實際實作，強調模組化、開放性與兼容性。

---

## 🧱 章節架構與筆記整理：

### 🔹 20.1 Linux History

- 起源於 1991 年 Linus Torvalds 的 80386 核心。
- 開放原始碼、快速社群協作、逐步加入 Unix 功能。
- 隨版本演進加入：

  - 1.0：TCP/IP, socket, SCSI, paging
  - 2.0：多架構支援、SMP、memory-mapped I/O、內核 threads
  - 2.6：preemptive kernel、O(1) scheduler、journaling FS
  - 3.x, 4.x：演進式版本號，CFS 調度器、mobile 改善

### 🔹 20.1.2 Linux System 組成

- Linux kernel：完全由 Linux 社群開發。
- 工具鏈來自 GNU、BSD、X Window 等。
- Distributions（發行版）：

  - 包含 kernel + 安裝/升級工具 + 應用程式（browser、editor）
  - RedHat, Debian, Ubuntu, Slackware 等。

- 授權：GNU GPL v2，必須開放原始碼才能合法發佈。

---

### 🔹 20.2 Design Principles 設計原則

- 類似傳統 monolithic UNIX：multiuser、preemptive、POSIX 相容。
- 強調：

  - 效率（early-stage 資源受限）
  - 標準化（POSIX, pthread, real-time）

- 三大構件：

  1. Kernel：控制記憶體、程序等 OS 抽象
  2. System libraries（libc）：系統呼叫的使用者介面
  3. System utilities：管理任務與守護程序（daemon）

---

### 🔹 20.3 Kernel Modules 核心模組

- Linux 支援 **模組動態載入與卸除（loadable modules）**。
- 優點：免重新編譯整個 kernel；便於維護與開發。
- 模組支援元件：

  1. Module manager：載入記憶體 + symbol 連結處理
  2. Loader/unloader：使用者層的載入指令
  3. Driver registration：登錄設備、filesystem、protocol
  4. Conflict resolution：避免硬體資源競爭（I/O port, IRQ）

---

### 🔹 20.4 Process Management 程序管理

- fork() + exec()：分開 process 建立與程式執行。
- process 結構三層：

  1. **Identity**（PID, UID, Namespace）
  2. **Environment**（env vars, args）
  3. **Context**（scheduling, memory, file table, signal handler）

- Linux 不區分 thread 與 process → 都稱為 task

  - 使用 `clone()` 決定共享哪些資源（FS, VM, signal, files）

---

### 🔹 20.5 Scheduling 調度機制

- Time-sharing 與 Real-time 兩套機制：

  - **CFS (Completely Fair Scheduler)**：

    - 不用固定 time slice → 每個 thread 得到比例分配。
    - 使用 `target latency` 與 `minimum granularity` 控制效率與公平。

  - Real-time：支援 POSIX 的 FCFS 與 RR，soft real-time。

#### Kernel Synchronization 核心同步：

- 舊版 kernel 非 preemptive。
- 2.6 起改為 preemptive → 提供 `preempt_disable()` 控制。
- 同步工具：

  - spinlock：短期鎖（SMP）
  - semaphore：長期鎖
  - Top-half / Bottom-half 模型：中斷處理可延後

---

### 🔹 20.6 Memory Management 記憶體管理

- 分為兩部份：

  1. **實體記憶體管理（Physical memory）**

     - 區分為 ZONE_DMA / DMA32 / NORMAL / HIGHMEM
     - 使用 buddy system 管理 page 分配與合併。

  2. **虛擬記憶體管理（Virtual memory）**

     - 每個 process 擁有獨立的 VM context
     - 使用 demand paging、lazy loading，並支援 memory-mapped files。

---

## ✅ 整體總結：

| 範疇         | Linux 特性總結                                               |
| ------------ | ------------------------------------------------------------ |
| 核心型態     | Monolithic，支援模組動態載入                                 |
| 多工與多線程 | 使用 `clone()` 實作輕量化 threads；無明確區分 process/thread |
| 排程機制     | 採 CFS 公平排程器，改善互動性與公平性                        |
| 記憶體管理   | 支援多 zone 實體記憶體與需求式虛擬記憶體                     |
| 標準與兼容   | 遵循 POSIX, GPL，與 GNU 工具鏈合作密切                       |

---

## 📘 第 21 章：Windows 10（Chapter 21: Windows 10）

### 🧭 一貫性主軸

本章主軸在於說明：**Windows 10 作為一個現代多用戶、多平台、以服務為導向的作業系統，如何整合傳統與新興技術，兼顧效能、安全、相容性與擴充性**。它是 NT 系列的最新發展，融合桌機、行動與雲端設備需求。

---

### 📂 章節結構與重點概念

#### 21.1 歷史沿革（History）

- ⏳ 從 NT 到 Windows 10 的發展歷程

  - Windows NT → 2000 → XP → Vista → 7 → 8/8.1 → 10

- 🚫 Vista 的失敗教訓：相容性與介面改變過大
- 🛠 Windows 10 引入 WaaS（Windows-as-a-Service）模型
- 🌐 支援多平台：Desktop、IoT、Mobile、Xbox、Mixed Reality
- 🐧 引入 Windows Subsystem for Linux（WSL）
- 📦 新包裝應用模式（UWP）與 Desktop Bridge 相容傳統 Win32

#### 21.2 設計原則（Design Principles）

1. **安全性（Security）**

   - ACL、Integrity Level、BitLocker、DEP、ASLR、CFG、ACG
   - 支援硬體安全功能如 CET、shadow stack、Device Guard
   - VTL/VSM 模式支援資料與執行隔離
   - 使用 Code Integrity、數位簽章、App Whitelisting

2. **可靠性（Reliability）**

   - 記憶體診斷與自動錯誤報告
   - 自動回報與遙測（telemetry）收集生態圈資料
   - 移除不穩定的 kernel-mode 元件到 user-mode（如 font renderer）

3. **應用程式相容性（Compatibility）**

   - Shim Engine、WoW32/WoW64/WoWA64、Pico Provider + LxCore
   - 支援 Linux ELF binary、Virtual Machine for legacy

4. **效能（Performance）**

   - 核心優化：Pushlock、Lock-free 結構、NUMA 支援
   - Processor Group 支援多達 640 個邏輯處理器
   - GPU 加速（DirectCompute）、OpenCL、HMP 核心分配
   - UMS（User-Mode Scheduling）支援用戶端排程器

5. **擴充性（Extensibility）**

   - 模組化 Kernel、Loadable drivers、Pico Provider
   - ALPC、DCOM、WMI、WinRM 等 RPC 通訊擴充機制

6. **可攜性（Portability）**

   - HAL + HAL Extension 分離硬體平台相依性
   - 支援多架構（IA32, AMD64, ARM, ARM64）

7. **國際化（International Support）**

   - NLS API、UNICODE、MUI 多語介面支援

8. **能源效率（Energy Efficiency）**

   - Core Parking、Dynamic Tick、PLM、DAM、Connected Standby
   - UWP 強制 suspend idle 應用，節省電力

9. **動態裝置支援（Dynamic Device Support）**

   - Plug-and-Play、自動驅動載入、動態增減 CPU 與記憶體（雲端虛擬環境支援）

---

### 🧱 架構概覽（見 Figure 21.1）

- **User Mode**

  - 子系統 DLL（如 ntdll.dll）、應用程式、服務

- **Kernel Mode**

  - Executive（核心服務：I/O、Memory、Security...）
  - Kernel（Dispatcher、Scheduler...）
  - HAL（硬體抽象層）

- **Hypervisor 層（VSM 啟用時）**

  - Hyper-V + Secure Kernel（Credential Guard, Device Guard, TPM 相關功能）

---

## 📘 附錄 A：影響深遠的作業系統（Influential Operating Systems）

### 🎯 章節目的標的

- 認識歷史上重要的作業系統
- 理解系統特性如何從大型主機遷移到桌面與行動裝置
- 建立歷史與現代系統的連結

---

### 🧱 核心架構概念整理（按演化順序）

| 系統                  | 時間      | 特點                                                   | 傳承與貢獻                              |
| --------------------- | --------- | ------------------------------------------------------ | --------------------------------------- |
| **Atlas (英國)**      | 1950s–60s | 最早實作虛擬記憶體（paging）與高速快取（core）         | 頁面置換、記憶體階層                    |
| **XDS-940**           | 1960s     | Time-sharing、page relocation、process control tree    | UNIX 的記憶體架構靈感                   |
| **THE 系統 (荷蘭)**   | 1960s     | 第一個層次式作業系統 + 靜態 process pool               | 引入 semaphore、死鎖避免                |
| **RC 4000**           | 1969      | 作業系統核心 (kernel) 與簡化 message-passing 機制      | kernel 設計與 microkernel 概念起源      |
| **CTSS (MIT)**        | 1961      | 最早的 time-sharing 實作                               | MULTICS 的前身                          |
| **MULTICS**           | 1965–1970 | Segmented paging、protection rings、shared file system | UNIX 概念與 macOS/iOS 架構起點          |
| **OS/360 (IBM)**      | 1960s     | 統一支援所有 IBM 機型的作業系統                        | 複雜但具有歷史意義，發展出 JCL 與 MVS   |
| **TOPS-20**           | 1970s     | DEC 時代代表，支援虛擬記憶體與互動式命令               | 帶動 UNIX-like 使用習慣                 |
| **CP/M → MS-DOS**     | 1970s–80s | 8-bit/16-bit PC 的核心作業系統                         | 奠定個人電腦使用基礎                    |
| **Mac OS vs Windows** | 1980s+    | GUI 對決，逐步引入保護模式、context switch             | 桌面系統現代化之路                      |
| **Mach**              | 1980s     | Microkernel，支援多種作業系統 interface                | macOS/iOS 的 XNU kernel 基礎            |
| **Hydra / CAP**       | 研究系統  | Capability-based 保護模型                              | 權限放大與 abstract object 保護機制設計 |

---

### 🔄 一貫性主軸：Feature Migration 思維

> 許多作業系統設計特色從大型主機 → 小型機 → 桌機 → 手機逐步遷移與普及

| 初期高階功能             | 後來普及平台            |
| ------------------------ | ----------------------- |
| 分頁記憶體（paging）     | 桌面系統、手機          |
| 分時系統（time sharing） | 多使用者作業系統        |
| 多工與同步機制           | 作業系統內部進程管理    |
| 虛擬化與容器             | 雲端、DevOps 與開發環境 |

---

## 📘 第 21 章 Windows 系統架構與設計

### 章節結構總覽

1. **歷史演進**（Windows XP → 10）
2. **設計原則**
3. **系統組件**
4. **終端服務與快速使用者切換**
5. **檔案系統：NTFS**
6. **網路功能**
7. **程式設計介面（Programmer Interface）**
8. **總結與練習題**

---

### 🎯 核心觀念整理

#### 1. 設計原則（Design Principles）

- **安全性（Security）**：包含權限控管、身分驗證與漏洞防護。
- **可靠性（Reliability）**：透過系統還原與容錯機制提高穩定性。
- **效能（Performance）**：排程與記憶體管理演算法提升效率。
- **可擴充性（Extensibility）**：模組化架構，方便新增功能。
- **國際支援與節能（i18n & Power Efficiency）**：多語系、筆電與行動裝置優化。

#### 2. 系統組件（System Components）

- **Hyper-V**：提供虛擬化支援。
- **Secure Kernel**：安全性的核心防線。
- **HAL（硬體抽象層）**：隔離硬體差異。
- **Executive**：提供物件管理、記憶體、程序、I/O 控制。

#### 3. NTFS 檔案系統

- 提供 **交易日誌（journaling）**、**檔案壓縮**、**掛載點與符號連結**、**變更日誌** 等高階功能。

#### 4. 程式設計介面（Win32 API）

- 支援存取核心物件、跨程序共享、訊息傳遞（Windows Messaging）、記憶體管理等。

---

### 🔁 一貫性與思維總結

| 面向         | Linux（第 20 章）        | Windows（第 21 章）          |
| ------------ | ------------------------ | ---------------------------- |
| 設計原則     | 開放原始碼，模組彈性     | 安全、相容性與可靠性強調     |
| 檔案系統     | ext3/ext4 + VFS          | NTFS + journaling + VSS      |
| 記憶體管理   | 分頁 + 虛擬記憶體管理    | 整合式 VM + 安全區隔         |
| 網路         | POSIX sockets 支援廣     | 含 Active Directory、域管理  |
| 程式介面     | syscalls、POSIX API      | Win32 API 支援多樣作業       |
| 多使用者支援 | fork/exec 模型，具延展性 | Terminal Services 與快速切換 |

---

## 📚 附錄 A — 有影響力的作業系統回顧

### 重點系統與貢獻

- **CTSS / MULTICS**：最早的多工與分時系統，影響 UNIX。
- **IBM OS/360**：企業級作業系統典範，支援多任務。
- **CP/M → MS-DOS**：個人電腦作業系統的演化。
- **Mach**：微核心架構的實驗基礎。
- **Hydra & CAP**：能力型安全模型的先驅。

🧠 **一貫性啟示**：很多系統的特色（模組化、虛擬記憶體、能力控制、安全模型）逐步融合進 Windows 和 Unix 類系統。

---

## 🖥️ 附錄 B — Windows 深度解析（對照第 21 章）

### 補充技術細節

- **B.3 Executive**：負責 I/O 管理員、物件管理員、記憶體與程序管理。
- **B.5 檔案系統延伸**：Volume Management、Fault Tolerance、Shadow Copy。
- **B.6 網路架構**：支援 SMB、Active Directory、網域控管。
- **B.7 程式設計介面細節**：物件導向核心物件管理與訊息式 IPC 架構。

🧩 **與主章對照補充**：附錄 B 強化了對系統內部物件設計與 Win32 API 使用方式的具體技術解說。

---

## 📘 第 21 章：Windows 10

### 一、背景與架構

- 微軟於 2015 年發表 Windows 10，核心架構源自 Windows NT。
- 採用混合式核心（Hybrid Kernel）：兼具微核心與巨核心設計優點。
- 支援多用戶、保護記憶體、虛擬記憶體、多執行緒、多處理器等現代作業系統功能。

### 二、系統元件

1. **Executive**

   - 包含管理記憶體（Memory Manager）、程序控制（Process Manager）、IO 管理（I/O Manager）、安全性機制、安全審核與登錄等。

2. **Kernel**

   - 低階核心功能，如排程、同步、異常處理、中斷管理。

3. **HAL（硬體抽象層）**

   - 提供統一硬體接口，使得 Windows 可移植於不同硬體平台。

4. **Device Drivers**

   - 模組化設計，實現即插即用與電源管理。

5. **User-Mode Subsystems**

   - 包含 Win32、POSIX、OS/2 子系統。

6. **環境子系統（Environment Subsystems）**

   - 為使用者提供 API 呼叫，橋接 kernel 與應用層。

### 三、物件導向設計

- 一切皆為物件（Object）概念，包含程序、執行緒、檔案、設備等。
- 透過統一的物件管理機制管理資源與存取權限。

### 四、核心功能

1. **程序與執行緒管理**

   - 支援 user-mode thread & kernel-mode thread，執行緒排程由 dispatcher 管理。

2. **記憶體管理**

   - 採用 demand paging 與 working set，支援 section objects 共享記憶體。

3. **I/O 系統**

   - 所有裝置以 I/O 請求封包（IRP）為基礎處理。
   - 支援異步 I/O、Direct Memory Access。

4. **檔案系統**

   - NTFS：提供 metadata、journaling、安全性與壓縮等。

5. **安全性模型**

   - 每個物件與使用者帳號/群組對應存取控制清單（ACL）。

---

## 📁 Appendix A：A Linux Source Code Walkthrough

### 主軸：解釋 Linux 核心原始碼中主要模組，強化實作層次理解

1. **初始化過程**（boot + init/main.c）

   - 使用 start_kernel 啟動記憶體管理與驅動程序註冊。

2. **系統呼叫處理**

   - 系統呼叫表 `sys_call_table` 負責分派系統呼叫。

3. **排程器**

   - Linux 使用 CFS（Completely Fair Scheduler）或舊版 O(1) 排程器。

4. **檔案系統**

   - struct `file`, `inode`, `dentry` 是檔案抽象的核心結構。

5. **中斷處理與裝置驅動**

   - `request_irq()` 註冊中斷，驅動以模組方式插入。

6. **記憶體管理**

   - 使用頁式管理、分配方式：buddy system、slab/slub allocator。

7. **程序與執行緒**

   - 使用 `task_struct` 表示 process，`fork()`/`execve()` 管理程序。

---

## 📁 Appendix B：BSD UNIX 概念導覽（FreeBSD）

### B.1 設計原則

- 一致性、可攜性、可組合性。
- 裝置與檔案概念統一（everything is a file）。
- 使用者介面與程式設計接口分離（kernel / user-space interface）。

### B.2 系統架構

- kernel 提供檔案系統、記憶體管理、排程、裝置管理、signal 等功能。
- Shell、命令與程式透過 system call 操作 kernel。

### B.3 使用者介面與 shell

- 支援 C shell、Bourne shell、Korn shell。
- 提供 shell script、pipes、重導向、job control 等機制。

### B.4 系統呼叫分類

- 檔案操作：open, read, write, close, stat
- 程序控制：fork, exec, wait, exit
- 訊號處理：signal, kill, sigaction
- 資訊查詢與控制：getpid, getuid, gettimeofday

---

## 📁 Appendix C：BSD UNIX 歷史與實作（FreeBSD 為例）

### C.1 UNIX 發展歷程

- UNIX 始於 1969，Ken Thompson 與 Dennis Ritchie。
- 1978 發佈 Version 7 成為後續 UNIX 分支基礎。
- System V（商業導向，AT\&T） vs BSD（研究導向，Berkeley）

### C.2 FreeBSD 發展

- 源自 4.3BSD-Lite，1993 年起獨立發展。
- 注重性能、安全性與開源性，支援 x86、Alpha 等架構。

### C.3 程式設計接口

- 系統呼叫：分為檔案、程序、訊號、資訊操作等。
- 運行模型：fork → exec → wait。
- 標準 I/O、shell script、pipes、filters 實現 UNIX 的 composability。

### C.4 使用者介面

- 目錄結構（如 /bin, /dev, /usr 等）
- 常見命令（ls, cp, mv, cat, grep, diff, etc.）
- Shell 語法與 script 控制流程（if, for, while）

### C.5 程序管理

- 透過 PCB（Process Control Block）、text/data/stack 段、user 結構與 kernel stack 實現。
- fork / vfork 行為差異、execve 不產生新程序但替換記憶體內容。
- vfork + copy-on-write 增進效率。
- CPU scheduling 為 priority + round robin 機制。

---

## 📘 第 21 章：The Linux System

### 🌐 主軸概念

Linux 是一個模組化、類 Unix 的作業系統，支援多使用者、多任務，源自 MINIX，遵循 POSIX 規範，並具有開放原始碼特性，廣泛用於伺服器與嵌入式系統。

### 📂 架構概覽

- **Kernel Mode / User Mode**：提供明確的權限邊界
- **核心模組化（Modular Kernel）**：以 loadable kernel modules 支援可動態載入的驅動程式與擴充功能
- **系統呼叫介面（Syscall Interface）**：用戶空間與核心互動的橋梁

### 🔧 核心子系統（Subsystems）

- **Process Scheduler**：Completely Fair Scheduler (CFS)，以 Red-Black Tree 維護執行緒優先順序
- **Memory Management**：採用分頁與虛擬記憶體，整合 slab 分配器
- **Virtual File System (VFS)**：抽象層支援多種檔案系統（如 ext4, FAT, ISO9660）
- **Networking**：遵循 TCP/IP 協定，支援網路 socket、IP routing、firewall（Netfilter）

### 🧩 Device Drivers

- 為 kernel modules 的一環，可熱插拔
- 使用 `struct file_operations` 處理 open/read/write/ioctl

### 🔐 安全機制

- DAC (Discretionary Access Control)
- MAC (Mandatory Access Control, e.g., SELinux)
- Capabilities（權限拆解）

---

## 📘 附錄 A：BSD UNIX

### 🌐 主軸概念

BSD UNIX 源於加州大學柏克萊分校，是現代 UNIX 的主要變體，對虛擬記憶體與 TCP/IP 網路支援有關鍵貢獻。

### 📂 關鍵特色

- 支援多使用者、多任務
- Memory Paging + Virtual Memory
- Sockets-based 通訊模型
- 豐富的程式庫與工具鏈

---

## 📘 附錄 B：The FreeBSD 系統

### 🌐 主軸概念

FreeBSD 是從 4.4BSD 演化而來的開放原始碼作業系統，擁有先進的網路與檔案系統支援，並廣泛用於伺服器與安全設備。

### 🧱 架構

- Mbuf networking stack
- GEOM storage framework
- Jails（輕量虛擬化）

---

## 📘 附錄 C：Windows 10 作業系統

### 🌐 主軸概念

Windows 10 採用 NT 架構，具備高模組性與支援 GUI 的能力，是企業與個人端常用平台。

### 📂 子系統與元件

- **Executive services**：支援記憶體管理、I/O、物件管理
- **Kernel**：Thread scheduling, interrupt handling
- **User-mode subsystems**：Win32、POSIX、.NET runtime
- **File system**：NTFS, 支援 journaling 與 ACL
- **Registry**：集中設定倉庫

---

## 📘 附錄 D：The Mach 系統

### 🌐 主軸概念

Mach 是一個微核心作業系統，發展於 Carnegie Mellon，支援多處理器與分散式系統。與 UNIX 相容，支援多作業系統介面。

### 🔧 核心設計

- 微核心（microkernel）架構，最小化核心責任
- IPC（Interprocess Communication）與虛擬記憶體整合
- 任務（task）、執行緒（thread）、通訊埠（port）為主要抽象
- 支援遠端訊息傳送、訊息代理（proxy ports）

### 💡 Mach 的創新

- Memory Object 與 IPC 整合為核心通訊模型
- 可在不同作業系統上模擬（如 BSD、DOS）
- Network Message Server 處理分散式 port 傳遞

---

## 🔁 一貫性概念統整

| 面向            | 核心概念                                                                               |
| --------------- | -------------------------------------------------------------------------------------- |
| ✳ 設計理念      | 簡化核心責任（Mach, Linux），模組化（Linux, BSD），彈性擴展性（FreeBSD, Windows）      |
| 🔄 虛擬記憶體   | 分頁、記憶體對映、copy-on-write（Linux, BSD, Mach）                                    |
| 📩 IPC 模型     | Mach：Port + Message；Linux/BSD：pipe, socket；Windows：Named Pipe, Message Queue      |
| 👥 多執行緒支援 | Mach (kernel thread)、Linux (CFS)、Windows (preemptive scheduler with priority levels) |
| 🔐 安全控制     | Linux (SELinux), Windows (ACL), BSD (MAC + jails)                                      |

---
