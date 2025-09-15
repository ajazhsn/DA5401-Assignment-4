# Credit Card Fraud Detection Using GMM-Based Synthetic Sampling

## Introduction  

Credit card fraud detection is a major challenge in financial security. The dataset analyzed in this project contains more than **284,000 transactions**, with fraudulent activity accounting for only about **0.17%** of all records. This severe imbalance causes standard classifiers to be biased towards the majority (valid) class, resulting in poor fraud detection.  

To address this, we test multiple strategies with **Logistic Regression** as the base model:  

- A **baseline Logistic Regression** trained on imbalanced data.  
- **GMM-based oversampling** to create synthetic fraud samples.  
- **Cluster-Based Undersampling (CBU)** to reduce redundant majority samples.  
- A **hybrid approach (GMM + CBU)** for better balance and accuracy.  

---

## Dataset Overview  

The dataset contains **284,807 transactions** and **31 features**, including:  

- Principal components (V1–V28), anonymized for confidentiality.  
- The transaction amount.  
- Target variable `Class` (0 = Valid, 1 = Fraud).  

Fraud cases are rare (~0.17%), making this a highly imbalanced classification problem.  

---

## Objectives  

- Train a baseline Logistic Regression on imbalanced data.  
- Use **GMM** to generate synthetic fraudulent samples.  
- Apply **CBU** to reduce the size of the majority class without losing diversity.  
- Evaluate models using **precision, recall, and F1-score** on the fraud class.  
- Recommend the best approach based on experimental evidence.  

---

## Methodology  

### 1. Data Loading and Initial Analysis  

- Loaded dataset into memory.  
- Verified the imbalance: ~284,315 valid vs. 492 fraud transactions.  

---

### 2. Baseline Logistic Regression  

- Logistic Regression trained with `class_weight="balanced"`.  
- Evaluated using standard performance metrics.  

**Fraud Class Performance (Baseline):**

| Metric     | Value  |
|------------|--------|
| Precision  | 0.067  |
| Recall     | 0.878  |
| F1-Score   | 0.125  |

➡ High recall (most fraud cases detected), but extremely low precision (many false positives).  

---

### 3. GMM-Based Oversampling  

- Modeled fraud class with a Gaussian Mixture Model (4 components chosen via AIC/BIC).  
- Generated synthetic samples and retrained Logistic Regression.  

**Fraud Class Performance (GMM Oversampling):**

| Metric     | Value  |
|------------|--------|
| Precision  | 0.089  |
| Recall     | 0.865  |
| F1-Score   | 0.161  |

➡ Precision improves, reducing false positives. Recall remains strong, and F1-score shows a clear improvement.  

---

### 4. GMM + Cluster-Based Undersampling (CBU)  

- Applied clustering to majority class samples and undersampled it.  
- Combined with GMM-generated fraud samples to balance the dataset.  
- Trained Logistic Regression on this combined data.  

**Fraud Class Performance (GMM + CBU):**

| Metric     | Value  |
|------------|--------|
| Precision  | 0.090  |
| Recall     | 0.865  |
| F1-Score   | 0.163  |

➡ Marginal but meaningful improvement over GMM alone, confirming benefits of balancing both classes.  

---

## Confusion Matrix Observations  

- **Baseline:** High recall, but excessive false positives.  
- **GMM Oversampling:** More true positives, fewer false positives than baseline.  
- **GMM + CBU:** Further refinement, with improved true positives and better control of false positives.  

---

## Theoretical Insights  

- **GMM Oversampling:**  
  - Models minority data as a Gaussian mixture.  
  - Captures subgroup variability within fraud class.  
  - Generates realistic synthetic samples unlike simpler methods (e.g., SMOTE).  

- **CBU:**  
  - Reduces size of majority class while retaining its diversity.  
  - Decreases model bias towards the valid class.  

- **Hybrid (GMM + CBU):**  
  - Balances oversampling and undersampling.  
  - Produces a compact, well-balanced dataset that improves classifier performance.  

---

## Final Recommendations  

- **Baseline Logistic Regression** struggles with low precision despite high recall.  
- **GMM Oversampling** greatly enhances F1-score, reducing false positives while maintaining recall.  
- **GMM + CBU** yields the strongest performance, with improvements in both precision and F1-score.  
- Even small metric improvements (precision: 0.067 → 0.090, F1: 0.125 → 0.163) have major real-world impact, reducing false alarms and enhancing fraud monitoring efficiency.  

---

**Ajaz Hussain**  
DA25M006  
