# 🩺 FHIR R4 頭痛 Observation 資源集

[![FHIR R4](https://img.shields.io/badge/FHIR-R4-blue?style=flat-square)](https://hl7.org/fhir/R4/)
[![SNOMED CT](https://img.shields.io/badge/Terminology-SNOMED%20CT-green?style=flat-square)](https://www.snomed.org/)
[![LOINC](https://img.shields.io/badge/Terminology-LOINC-orange?style=flat-square)](https://loinc.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

> 提供民眾頭痛主訴的 FHIR R4 Observation 標準化範例、欄位定義、Value Set 編碼表，以及可直接在瀏覽器使用的互動式 JSON 產生器（純 HTML + JS，無需後端）。

---

## 📁 檔案結構

```
FHIR_headache/
├── headache_observation.html          ← 互動式 FHIR JSON 產生器（HTML + JS）
├── headache_fhir_example.json         ← FHIR Observation 完整範例
├── headache_observation_fields.csv    ← Observation 欄位定義表
├── headache_location_valueset.csv     ← 疼痛部位（Location）Value Set
├── headache_character_valueset.csv    ← 疼痛性質（Character）Value Set
└── headache-obs-1775827402561.json    ← 產生器輸出範例（民眾填寫結果）
```

---

## 🔬 FHIR Observation 結構設計

本資料集以 **FHIR R4 `Observation`** 資源記錄民眾頭痛主訴，採用三個 `component` 分別描述：

| # | Component | 觀察項目代碼 | 代碼系統 | 值類型 |
|---|-----------|------------|---------|-------|
| 1 | **疼痛程度** (Severity) | `72514-3` (Pain severity 0-10 VNR) | LOINC | `valueQuantity` (0–10) |
| 2 | **疼痛部位** (Location) | `722546004` (Location of pain) | SNOMED CT | `valueCodeableConcept` |
| 3 | **疼痛性質** (Character) | `255314001` (Pain character) | SNOMED CT | `valueCodeableConcept` |

### 主題代碼（Observation.code）

| 欄位 | 值 |
|------|----|
| `system` | `http://snomed.info/sct` |
| `code` | **`25064002`** |
| `display` | *Headache (finding)* |

### Observation.category

| 欄位 | 值 |
|------|----|
| `system` | `http://terminology.hl7.org/CodeSystem/observation-category` |
| `code` | `survey` |

---

## 📐 Component 1：疼痛程度（Severity Scale）

使用 LOINC **`72514-3`** — *Pain severity - 0-10 verbal numeric rating [Score]*

```json
{
  "code": {
    "coding": [{
      "system": "http://loinc.org",
      "code": "72514-3",
      "display": "Pain severity - 0-10 verbal numeric rating [Score]"
    }],
    "text": "疼痛程度（0–10 分）"
  },
  "valueQuantity": {
    "value": 7,
    "unit": "score",
    "system": "http://unitsofmeasure.org",
    "code": "{score}"
  }
}
```

| 數值 | 臨床意義 |
|------|--------|
| 0 | 無痛 |
| 1–3 | 輕度疼痛 |
| 4–6 | 中度疼痛 |
| 7–9 | 重度疼痛 |
| 10 | 無法忍受（最大痛） |

---

## 🗺️ Component 2：疼痛部位 Value Set（HeadachePainLocation）

- **Component 觀察代碼**：SNOMED `722546004` — *Location of pain (observable entity)*  
- **代碼系統**：SNOMED CT `http://snomed.info/sct`

| SNOMED 代碼 | 英文名稱 | 中文名稱 | 解剖說明 | 建議頭痛類型 |
|------------|---------|---------|---------|------------|
| `69536005` | Head structure | 全頭 | 整個頭部廣泛性疼痛 | 緊張型頭痛 / 發燒性頭痛 |
| `9454009` | Structure of frontal region of head | 前額部 | 前額至眉心區域 | 緊張型頭痛 / 鼻竇性頭痛 |
| `81327009` | Structure of temporal region | 顳部（雙側太陽穴） | 兩側顳骨覆蓋區域 | 緊張型頭痛 / 顳動脈炎 |
| `6757004` | Structure of right temporal region | 右顳部（右太陽穴） | 頭部右側顳骨區域 | 偏頭痛（右側發作） |
| `8170008` | Structure of left temporal region | 左顳部（左太陽穴） | 頭部左側顳骨區域 | 偏頭痛（左側發作） |
| `31065004` | Structure of occipital region | 枕部（後腦勺） | 枕骨覆蓋區域 | 緊張型頭痛 / 頸源性頭痛 |
| `14414005` | Structure of parietal region | 頂部（頭頂） | 頂骨覆蓋區域 | 緊張型頭痛 / 高顱壓頭痛 |
| `56564000` | Structure of eye region | 眼眶周圍 | 眼眶及球後區域 | 叢集性頭痛 / 眼部相關頭痛 |

---

## 🎨 Component 3：疼痛性質 Value Set（HeadachePainCharacter）

- **Component 觀察代碼**：SNOMED `255314001` — *Pain character (observable entity)*  
- **代碼系統**：SNOMED CT `http://snomed.info/sct`  
- **參考標準**：IHS（International Headache Society）ICHD-3 頭痛分類

| SNOMED 代碼 | 英文名稱 | 中文名稱 | 臨床意義 |
|------------|---------|---------|--------|
| `44695005` | Pulsating sensation | 搏動性（波動跳痛） | **偏頭痛** ICHD-3 診斷指標 |
| `55145008` | Throbbing pain | 陣發性跳痛 | 偏頭痛 / 血管性頭痛 |
| `34713003` | Pricking sensation | 刺痛性（針刺感） | 叢集性頭痛 / 原發性刺痛性頭痛 |
| `7895008` | Sharp pain | 銳痛（尖銳痛） | 叢集性頭痛 / 三叉神經痛 |
| `410430009` | Aching | 鈍痛酸痛 | **緊張型頭痛** |
| `26614003` | Pressing pain sensation | 壓迫性（重壓感） | **緊張型頭痛** ICHD-3 診斷指標 |
| `23924001` | Tight sensation | 緊縮性（箍緊感） | 緊張型頭痛 / 頸源性頭痛 |
| `36349006` | Burning sensation | 灼熱性（灼燒感） | 神經病變性頭痛 / 帶狀疱疹後神經痛 |

---

## 🖥️ headache_observation.html — 互動式產生器使用說明

### 功能特色

- **純前端**：單一 HTML 檔，無需任何後端或安裝，直接瀏覽器開啟即用
- **疼痛程度滑桿**：0–10 顏色漸變（綠→黃→紅），即時顯示數值
- **多選部位 / 性質**：每個勾選項目各自產生獨立 `component`，完整支援多部位/多性質記錄
- **JSON 語法高亮**：輸出使用藍色（key）/ 綠色（字串值）/ 橘色（數值）着色
- **✏️ 編輯模式**：可直接在黑色 textarea 修改 JSON，支援格式驗證，錯誤時標紅提示
- **📋 複製 / 💾 下載**：一鍵複製到剪貼簿或下載為 `.json` 檔
- **時區自動偵測**：`effectiveDateTime` 與 `issued` 自動填入本機時區（例：`+08:00`）

### 使用步驟

```
1. 直接用瀏覽器開啟 headache_observation.html
2. 填入病患 ID（必填）、姓名（選填）
3. 選擇症狀發生時間
4. 拖移滑桿設定疼痛程度（0–10）
5. 勾選疼痛部位（可多選，至少一個）
6. 勾選疼痛性質（可多選，至少一個）
7. 填入備註說明（選填）
8. 點擊「⚡ 產生 FHIR Observation JSON」
9. 右側顯示 JSON，可再點「✏️ 編輯」直接修改
10. 點「📋 複製」或「💾 下載」匯出結果
```

### 本地 Live Server 啟動方式

```bash
npx live-server . --open=headache_observation.html --port=5500
```

---

## 📄 headache_fhir_example.json — 範例說明

範例情境：民眾王小明，右顳部搏動性頭痛，疼痛指數 7/10

```
Patient: Patient/patient-001（王小明）
Observation.code: SNOMED 25064002（Headache finding）
Component[0]: 疼痛程度 = 7（Quantity）
Component[1]: 疼痛部位 = 右顳部（SNOMED 6757004）
Component[2]: 疼痛性質 = 搏動性（SNOMED 44695005）
```

---

## 📊 CSV 檔案說明

| 檔案 | 說明 | 欄位數 |
|------|------|-------|
| `headache_observation_fields.csv` | Observation 所有欄位路徑、必填、型態、說明 | 7 欄 |
| `headache_location_valueset.csv` | 疼痛部位 8 個 SNOMED CT 代碼 | 7 欄 |
| `headache_character_valueset.csv` | 疼痛性質 8 個 SNOMED CT 代碼 | 7 欄 |

---

## 🔗 標準規範參考

| 規範 | 說明 | 連結 |
|------|------|------|
| FHIR R4 Observation | HL7 官方資源定義 | https://hl7.org/fhir/R4/observation.html |
| SNOMED CT | 醫學術語系統 | https://www.snomed.org/ |
| LOINC 72514-3 | 疼痛數值量表代碼 | https://loinc.org/72514-3/ |
| UCUM | 臨床測量單位代碼 | https://ucum.org/ |
| ICHD-3 | 國際頭痛疾病分類第三版（IHS） | https://ichd-3.org/ |
| TW Core IG | 台灣 FHIR 核心實作指引 | https://twcore.mohw.gov.tw/ |

---

## ⚠️ 注意事項

1. **SNOMED CT 授權**：SNOMED CT 代碼依 2024-03-31 Release，正式部署前請向 SNOMED International 確認授權並驗證代碼有效性。
2. **_comment 欄位**：範例 JSON 中的 `_comment` 為說明用途，不屬於 FHIR 規範欄位，實際使用時應移除。
3. **Profile 擴充**：如需符合台灣 TW Core IG，請將 `meta.profile` 改為相對應之 TWCoreIG Profile URL。
4. **多 component**：同一個 `722546004`（Location）或 `255314001`（Character）代碼允許出現多次，分別對應不同解剖部位或疼痛性質。

---

## 📝 License

MIT License — 教學與研究用途，歡迎引用與修改。
