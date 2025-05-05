
# Operating System Concepts – 筆記整理 (Chapters 1–3)

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
