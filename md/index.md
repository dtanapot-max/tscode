# แผนผังโครงสร้างระบบ (Codebase Map & Structural Index)

เอกสารนี้บอกตำแหน่งและหน้าที่ของแต่ละส่วนในสถาปัตยกรรม 4 เสาหลัก (4 Decoupled Pillars)

## 1. ผังไฟล์เดี่ยว (index.html)
```text
index.html
├── <head>
│    ├── <style id="app-style">        [Design Tokens & Layout CSS]
│    ├── <script id="logger-source">   [1. Observability: window.Logger]
│    └── <script id="fs-source">       [2. Storage Engine: window.FS (OPFS)]
└── <body>
     ├── <div id="root">               [Viewport Container]
     └── <script id="app-source">
          ├── [1] UTILITIES            [DOM Helpers & Thai Typography Engine]
          ├── [2] STATE STORE          [3. State Domain: WorkspaceStateStore (1 Doc = 1 File)]
          ├── [3] UI CONTROLLER        [4. Presentation: WorkspaceUI]
          └── [4] BOOTSTRAP            [System Initialization]
```

## 2. หน้าที่ของแต่ละส่วน
- `window.Logger`: จัดการ Telemetry, Circular Buffer 250 รายการ และปุ่ม ⧉ AI Output
- `window.FS`: I/O Filesystem จัดการ OPFS (1 เอกสาร = 1 ไฟล์ดิสก์จริง) และ LocalStorage Fallback
- `WorkspaceStateStore`: คุม State ในหน่วยความจำด้วย Pub/Sub พร้อม Debounced File Write 400ms
- `WorkspaceUI`: จัดการ DOM แบบ Mount-Once, Event Delegation และ Thai IME Guard