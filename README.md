## Group Report

---
## Git Log
![image](https://github.com/user-attachments/assets/5cdb88cf-0f8b-489d-96b4-f0345bc9eb05)

---

## Model Evaluation & Interpretation

---

### **Michael**

**Model Overview:**
My model used a **combined `l1_l2` regularizer**, the **Adam optimizer**, **EarlyStopping** on `val_accuracy` (patience=10), and **low dropout (0.2)**. The goal was to balance regularization and training stability.

**Performance Recap:**

* **Accuracy:** 0.6860
* **F1 Score:** 0.4213
* **Recall:** 0.2930 *(Low)*
* **Precision:** 0.7500 *(High)*

**Interpretation:**

* The **F1 Score** is low due to poor recall, despite strong precision. This suggests my model was good at predicting positive cases correctly when it did make positive predictions but missed many actual positive cases (false negatives).
* A **recall of 0.2930** is too low for tasks where capturing true positives is critical (e.g., medical or fraud detection).
* The **precision of 0.7500** was the **highest** among all models, indicating very few false positives.

**Comparison with Teammates:**

* **Worse than Chance**: Chance’s model had better **accuracy (0.7073)**, **F1 (0.4754)**, and **recall (0.3398)**, likely because **RMSprop** better adapted to the learning rate and Chance didn’t use EarlyStopping.
* **Worse than Christophe in recall and F1**: Christophe’s dual-dropout strategy and `val_loss` EarlyStopping improved **recall (0.3516)** and **F1 (0.4675)** — my model sacrificed these for precision.

**Why My Model Underperformed:**

1. **EarlyStopping on `val_accuracy`** may have halted training too early, especially if accuracy plateaued before recall improved.
2. **Low dropout (0.2)** might not have been enough to prevent overfitting, causing poor generalization on unseen positives.

---

### **Dean**

**Model Overview:**
I used **l2 regularization**, **SGD optimizer**, and **EarlyStopping** on `val_loss` (patience=5), without dropout. My intention was to keep the model simple and robust.

**Performance Recap:**

* **Accuracy:** 0.6829
* **F1 Score:** 0.4378
* **Recall:** 0.3164
* **Precision:** 0.7105

**Interpretation:**

* Moderate across the board. **F1 Score and Recall** are better than Michael’s but lower than Chance and Christophe’s.
* **Precision (0.7105)** was solid, but not the highest.
* **Recall (0.3164)** shows the model is slightly better at catching true positives than Michael’s.

**Comparison with Teammates:**

* **Worse than Chance:** Chance had better **accuracy, F1, recall, and precision**—likely due to RMSprop’s adaptive learning vs my manually-tuned SGD.
* **Better than Michael**: My model achieved better recall and F1, indicating a more balanced tradeoff between false positives and false negatives.

**Why My Model Underperformed:**

1. **Lack of dropout** may have caused overfitting, even with regularization.
2. **SGD** needed extensive tuning; a poorly tuned learning rate likely slowed convergence and reduced performance compared to RMSprop or Adam.

---

### **Chance**

**Model Overview:**
I applied **l1 regularization**, **RMSprop**, no EarlyStopping, and a **0.3 dropout**. My goal was aggressive feature pruning with adaptive learning and uninterrupted training.

**Performance Recap:**

* **Accuracy:** 0.7073 *(Highest)*
* **F1 Score:** 0.4754 *(Highest)*
* **Recall:** 0.3398
* **Precision:** 0.7121

**Interpretation:**

* **F1 Score and Accuracy** lead the group, suggesting my model found the best trade-off between precision and recall.
* **Precision (0.7121)** and **recall (0.3398)** are both strong, contributing to the highest F1.
* **Not using EarlyStopping** may have allowed the model to fully converge, explaining the higher performance.

**Comparison with Teammates:**

* **Better than Michael & Dean** in F1, recall, and accuracy due to better optimizer (RMSprop) and dropout for regularization.
* Slightly better than **Christophe and Lesly** in accuracy and F1, despite similar optimizers, due to more effective regularization (l1 over l2) and no early halting.

**Why My Model Performed Best:**

1. **No EarlyStopping** allowed more training epochs, enabling convergence that others didn’t reach.
2. **RMSprop + l1** promoted generalization and adaptive learning, enhancing both recall and precision.

---

### **Christophe**

**Model Overview:**
I used **l2 regularization**, **RMSprop**, **EarlyStopping** on `val_loss`, and **two dropout layers (0.3 & 0.2)**. I aimed for stable generalization.

**Performance Recap:**

* **Accuracy:** 0.6753
* **F1 Score:** 0.4675
* **Recall:** 0.3516
* **Precision:** 0.6977

**Interpretation:**

* Best **recall** among all models, showing my model was best at identifying positives.
* **F1 score (0.4675)** was close to Chance’s, indicating a strong balance.
* Slightly lower **precision (0.6977)**, suggesting a few more false positives compared to Michael or Lesly.

**Comparison with Teammates:**

* **Better recall than all**: even Chance (0.3516 vs 0.3398), proving the dropout layers helped reduce overfitting.
* **Slightly worse accuracy and precision** than Chance, indicating some trade-off.

**Why My Model Was Strong:**

1. **Dual dropout** helped prevent overfitting and boosted recall.
2. **EarlyStopping on `val_loss`** targeted generalization better than accuracy-based stopping.

---

### **Lesly**

**Model Overview:**
I used **l2 regularization**, **AdamW optimizer**, **EarlyStopping** on `val_accuracy`, and aggressive dropout (**0.4 & 0.3**). I focused on reducing overfitting at all costs.

**Performance Recap:**

* **Accuracy:** 0.6951
* **F1 Score:** 0.4536
* **Recall:** 0.3242
* **Precision:** 0.7545 *(Highest)*

**Interpretation:**

* **Highest precision** in the group suggests very few false positives.
* **Moderate recall** (0.3242) means some positives were missed, but not as badly as Michael.
* **F1 Score** is good, though not as high as Chance or Christophe, due to slightly weaker recall.

**Comparison with Teammates:**

* **Higher precision than everyone**, even Michael, thanks to aggressive dropout and AdamW’s stability.
* Slightly **lower recall and F1** than Chance and Christophe, likely due to over-regularization.

**Why My Model Balanced Tradeoffs:**

1. **High dropout** drastically reduced overfitting and improved precision.
2. **AdamW** helped manage weight decay, supporting generalization even without high recall.

---

## Overall Takeaways

* **Best Recall**: Christophe (0.3516) — Dual dropout + val\_loss EarlyStopping
* **Best Precision**: Lesly (0.7545) — Aggressive dropout + AdamW
* **Best F1 / Accuracy**: Chance — Balanced model with RMSprop and no EarlyStopping
* **Most Conservative Model**: Michael — Prioritized precision but at the cost of low recall
* **Most Classic Approach**: Dean — SGD + l2 worked reasonably but lacked adaptability

**Key Insight:** The models that used **RMSprop** and carefully tuned dropout performed best in balancing recall and precision. **EarlyStopping** is helpful, but only when it targets meaningful loss functions, and not too aggressively.

---

### Insights from Experiments & Challenges Faced

1. **Michael:**

   * Combining l1\_l2 regularization with Adam and low dropout aimed to balance overfitting and model stability. However, limited recall (0.2930) suggests the model struggled with true positive rates despite high precision (0.7500).
   * **Challenge:** EarlyStopping on `val_accuracy` might have stopped the model early, leading to lower recall.

2. **Dean:**

   * Pure l2 regularization with SGD optimizer was intended for a simpler, more classic approach. The absence of dropout might have caused overfitting but steady training.
   * **Challenge:** Achieving the right learning rate with SGD was tricky, which might explain the slightly lower precision.

3. **Chance:**

   * Chance’s model, despite no EarlyStopping, performed the best in terms of accuracy (0.7073).
   * **Insight:** The combination of l1 regularizer and RMSprop seems to boost precision (0.7909) and F1 Score (0.4754).
   * **Challenge:** Potential overfitting without EarlyStopping, but still achieved the best accuracy.

4. **Christophe:**

   * Use of two dropout rates and EarlyStopping on `val_loss` balanced stability and generalization.
   * **Insight:** Solid recall (0.3516) and F1 score (0.4675) hint that dropout and RMSprop worked well together.
   * **Challenge:** Managing two dropout rates required careful tuning to avoid performance dips.

5. **Lesly:**

   * Aggressive dropout (0.4 & 0.3) with no EarlyStopping.
   * **Insight:** Lower accuracy but solid recall (0.3672) suggests robustness against overfitting.
   * **Challenge:** No EarlyStopping might have led to model degradation over time.

---

### Summary Table

| Train Instance | Engineer Name | Regularizer | Optimizer | Early Stopping             | Dropout Rate | Accuracy | F1 Score | Recall | Precision |
| -------------- | ------------- | ----------- | --------- | -------------------------- | ------------ | -------- | -------- | ------ | --------- |
| 1              | Michael       | l1\_l2      | Adam      | val\_accuracy, patience=10 | 0.2          | 0.6860   | 0.4213   | 0.2930 | 0.7500    |
| 2              | Dean          | l2          | SGD       | val\_loss, patience=5      | None         | 0.6829   | 0.4378   | 0.3164 | 0.7105    |
| 3              | Chance        | l1          | RMSprop   | None                       | 0.3          | 0.7073   | 0.4754   | 0.3398 | 0.7121    |
| 4              | Christophe    | l2          | RMSprop   | val\_loss, patience=5      | 0.3 & 0.2    | 0.6753   | 0.4675   | 0.3516 | 0.6977    |
| 5              | Lesly         | l2          | AdamW      | val\_accuracy, patience=8 | 0.4 & 0.3    | 0.6951   | 0.4536   | 0.3242 | 0.7545    |

---

### Conclusions

* Models with RMSprop optimizer (Chance & Christophe) generally outperformed others, suggesting better handling of adaptive learning rates.
* Dropout rates around 0.3 helped maintain decent recall and avoid overfitting.
* EarlyStopping improved generalization, but when absent (e.g., Chance’s model), higher accuracy was achieved, hinting at possible underfitting in other models.
* The l1\_l2 combination in Michael’s model had the highest precision but suffered in recall, highlighting the need for better class balance.
* Lesly’s heavier dropout rates improved recall slightly but hurt accuracy.
