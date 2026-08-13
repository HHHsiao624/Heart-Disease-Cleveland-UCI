# 心臟病預測分析 (Heart Disease Cleveland UCI)
 
以 UCI Cleveland 心臟病資料集為基礎，透過探索式資料分析（EDA）與三種機器學習模型（Random Forest、XGBoost、SVM），預測病患是否罹患心臟病，並比較各模型在準確率、召回率、F1-score 與 ROC-AUC 上的表現。
 
## 專案動機
 
心臟病是常見且高致死率的慢性疾病，及早透過臨床檢驗數值輔助判斷罹病風險，有助於醫療資源的分配與早期介入。本專案以結構化臨床數據出發，練習完整的機器學習分析流程：資料清理 → 探索分析 → 前處理 → 建模 → 評估比較，作為將分析能力延伸至醫療應用場景的練習。
 
## 資料集
 
- 來源：Kaggle - [Heart Disease Cleveland UCI](https://www.kaggle.com/datasets/cherngs/heart-disease-cleveland-uci)
- 樣本數：297 筆病患紀錄
- 特徵數：13 個臨床相關特徵 + 1 個目標欄位（`condition`：0 = 無心臟病, 1 = 有心臟病）
| 欄位 | 說明 |
|---|---|
| age | 年齡 |
| sex | 性別 |
| cp | 胸痛型態 |
| trestbps | 靜止血壓 |
| chol | 血清膽固醇 |
| fbs | 空腹血糖 > 120 mg/dl |
| restecg | 靜止心電圖結果 |
| thalach | 最大心率 |
| exang | 運動誘發心絞痛 |
| oldpeak | 運動相對於休息的 ST 段壓低 |
| slope | 運動高峰 ST 段斜率 |
| ca | 主要血管數目（螢光鏡檢測） |
| thal | 地中海貧血檢測結果 |
| condition | 目標變數（是否罹患心臟病） |
 
## 分析流程
 
### 1. 探索式資料分析（EDA）
- 分類變數（sex、cp、fbs、restecg、exang、slope、ca、thal）以圓餅圖檢視類別分布
- 連續變數（age、trestbps、chol、thalach、oldpeak、ca）以盒形圖檢視離群值與分布狀況
- 以熱力圖（Heatmap）檢視各特徵與目標變數之間的相關性，作為特徵篩選與模型解讀的依據
### 2. 資料前處理
- 對連續型變數（age、trestbps、chol、thalach、oldpeak、ca）進行標準化（`StandardScaler`），統一數值尺度以利模型收斂與距離計算（尤其對 SVM 影響較大）
### 3. 資料切分
- 以 `train_test_split` 切分訓練集與測試集，測試集比例 20%，`random_state=10` 確保結果可重現
### 4. 模型訓練與比較
訓練三種分類模型並統一以下指標評估：**Precision、Recall、F1-score、混淆矩陣、ROC 曲線與 AUC**
 
| 模型 | Accuracy | Precision (macro) | Recall (macro) | F1-score (macro) | ROC-AUC |
|---|---|---|---|---|---|
| Random Forest（max_depth=10, n_estimators=200） | 0.83 | 0.83 | 0.83 | 0.83 | 0.919 |
| XGBoost | 0.80 | 0.80 | 0.79 | 0.80 | 0.890 |
| SVM（linear kernel, C=1.0） | 0.87 | 0.87 | 0.87 | 0.87 | **0.941** |
 
> 在此資料集上，**SVM（線性核）表現最佳**，AUC 達 0.94；Random Forest 次之。整體而言三個模型表現相近，顯示特徵經標準化後具備良好的線性可分性，也反映資料集樣本數不大（297 筆），模型間差異未必具統計顯著性。
 
### 5. 模型解釋
- 以 `export_graphviz` 將 Random Forest 中單一決策樹視覺化，觀察模型的判斷路徑，作為向非技術背景（如臨床醫師）說明模型邏輯的輔助工具
## 使用的技術與套件
 
- **語言／環境**：Python、Google Colab
- **資料處理**：Pandas、NumPy
- **視覺化**：Matplotlib、Seaborn、pydot（決策樹視覺化）
- **機器學習**：scikit-learn（RandomForestClassifier、SVC、StandardScaler、train_test_split、classification_report、roc_curve）、XGBoost
## 專案結構
 
```
├── Heart_Disease_Cleveland_UCI.ipynb   # 主要分析程式碼
├── heart_cleveland_upload.csv          # 原始資料集（需自行下載）
└── README.md
```
 
## 如何執行
 
1. 於 Kaggle 下載 `heart_cleveland_upload.csv`
2. 於 Google Colab 開啟 `Heart_Disease_Cleveland_UCI.ipynb`，掛載雲端硬碟並設定資料路徑
3. 依序執行各區塊：前置準備 → EDA → 前處理 → 資料切分 → 模型訓練與評估
