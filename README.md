## Group Report

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
| 3              | Chance        | l1          | RMSprop   | None                       | 0.3          | 0.7073   | 0.4754   | 0.3398 | 0.7909    |
| 4              | Christophe    | l2          | RMSprop   | val\_loss, patience=5      | 0.3 & 0.2    | 0.6753   | 0.4675   | 0.3516 | 0.6977    |
| 5              | Lesly         | l2          | AdamW      | val\_accuracy, patience=8 | 0.4 & 0.3    | 0.6951   | 0.4536   | 0.3242 | 0.7545    |

---

### Conclusions

* Models with RMSprop optimizer (Chance & Christophe) generally outperformed others, suggesting better handling of adaptive learning rates.
* Dropout rates around 0.3 helped maintain decent recall and avoid overfitting.
* EarlyStopping improved generalization, but when absent (e.g., Chance’s model), higher accuracy was achieved, hinting at possible underfitting in other models.
* The l1\_l2 combination in Michael’s model had the highest precision but suffered in recall, highlighting the need for better class balance.
* Lesly’s heavier dropout rates improved recall slightly but hurt accuracy.
