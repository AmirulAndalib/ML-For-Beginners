<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "808a71076f76ae8f5458862a8edd9215",
  "translation_date": "2025-08-29T13:58:24+00:00",
  "source_file": "4-Classification/3-Classifiers-2/README.md",
  "language_code": "ur"
}
-->
# کھانے کی اقسام کی درجہ بندی 2

اس دوسرے درجہ بندی کے سبق میں، آپ عددی ڈیٹا کو درجہ بندی کرنے کے مزید طریقے دریافت کریں گے۔ آپ یہ بھی سیکھیں گے کہ ایک درجہ بندی کو دوسرے پر منتخب کرنے کے کیا اثرات ہو سکتے ہیں۔

## [لیکچر سے پہلے کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/23/)

### پیشگی شرط

ہم فرض کرتے ہیں کہ آپ نے پچھلے اسباق مکمل کر لیے ہیں اور آپ کے `data` فولڈر میں ایک صاف شدہ ڈیٹا سیٹ موجود ہے جس کا نام _cleaned_cuisines.csv_ ہے، جو اس 4-سبق والے فولڈر کی جڑ میں ہے۔

### تیاری

ہم نے آپ کے _notebook.ipynb_ فائل کو صاف شدہ ڈیٹا سیٹ کے ساتھ لوڈ کیا ہے اور اسے X اور y ڈیٹا فریمز میں تقسیم کیا ہے، جو ماڈل بنانے کے عمل کے لیے تیار ہیں۔

## درجہ بندی کا نقشہ

پہلے، آپ نے مائیکروسافٹ کے چیٹ شیٹ کا استعمال کرتے ہوئے ڈیٹا کو درجہ بندی کرنے کے مختلف اختیارات کے بارے میں سیکھا۔ Scikit-learn ایک مشابہ لیکن زیادہ تفصیلی چیٹ شیٹ پیش کرتا ہے جو آپ کے تخمینے (درجہ بندی کرنے والے) کو مزید محدود کرنے میں مدد دے سکتا ہے:

![Scikit-learn سے ML نقشہ](../../../../translated_images/map.e963a6a51349425ab107b38f6c7307eb4c0d0c7ccdd2e81a5e1919292bab9ac7.ur.png)
> ٹپ: [اس نقشے کو آن لائن دیکھیں](https://scikit-learn.org/stable/tutorial/machine_learning_map/) اور راستے پر کلک کریں تاکہ دستاویزات پڑھ سکیں۔

### منصوبہ

یہ نقشہ آپ کے ڈیٹا کی واضح سمجھ حاصل کرنے کے بعد بہت مددگار ثابت ہوتا ہے، کیونکہ آپ اس کے راستوں پر 'چل' کر فیصلہ کر سکتے ہیں:

- ہمارے پاس >50 نمونے ہیں
- ہم ایک زمرہ کی پیش گوئی کرنا چاہتے ہیں
- ہمارے پاس لیبل شدہ ڈیٹا ہے
- ہمارے پاس 100K سے کم نمونے ہیں
- ✨ ہم ایک Linear SVC منتخب کر سکتے ہیں
- اگر یہ کام نہ کرے، چونکہ ہمارے پاس عددی ڈیٹا ہے
    - ہم ✨ KNeighbors Classifier آزما سکتے ہیں 
      - اگر یہ کام نہ کرے، تو ✨ SVC اور ✨ Ensemble Classifiers آزما سکتے ہیں

یہ ایک بہت مددگار راستہ ہے۔

## مشق - ڈیٹا تقسیم کریں

اس راستے پر چلتے ہوئے، ہمیں کچھ لائبریریاں درآمد کرنے سے شروع کرنا چاہیے۔

1. مطلوبہ لائبریریاں درآمد کریں:

    ```python
    from sklearn.neighbors import KNeighborsClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.svm import SVC
    from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    import numpy as np
    ```

1. اپنے تربیتی اور ٹیسٹ ڈیٹا کو تقسیم کریں:

    ```python
    X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
    ```

## Linear SVC درجہ بندی کرنے والا

Support-Vector Clustering (SVC) مشین لرننگ تکنیکوں کے Support-Vector Machines خاندان کا حصہ ہے (نیچے ان کے بارے میں مزید جانیں)۔ اس طریقے میں، آپ لیبلز کو کلسٹر کرنے کے لیے ایک 'kernel' منتخب کر سکتے ہیں۔ 'C' پیرامیٹر 'regularization' کو ظاہر کرتا ہے جو پیرامیٹرز کے اثر کو منظم کرتا ہے۔ kernel [کئی](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC) میں سے ایک ہو سکتا ہے؛ یہاں ہم اسے 'linear' پر سیٹ کرتے ہیں تاکہ Linear SVC کا فائدہ اٹھایا جا سکے۔ Probability ڈیفالٹ طور پر 'false' ہے؛ یہاں ہم اسے 'true' پر سیٹ کرتے ہیں تاکہ probability estimates حاصل کر سکیں۔ ہم random state کو '0' پر سیٹ کرتے ہیں تاکہ ڈیٹا کو شفل کر کے probabilities حاصل کی جا سکیں۔

### مشق - Linear SVC کا اطلاق کریں

درجہ بندی کرنے والوں کی ایک array بنا کر شروع کریں۔ جیسے جیسے ہم ٹیسٹ کریں گے، آپ اس array میں بتدریج اضافہ کریں گے۔

1. Linear SVC سے شروع کریں:

    ```python
    C = 10
    # Create different classifiers.
    classifiers = {
        'Linear SVC': SVC(kernel='linear', C=C, probability=True,random_state=0)
    }
    ```

2. اپنے ماڈل کو Linear SVC کا استعمال کرتے ہوئے تربیت دیں اور رپورٹ پرنٹ کریں:

    ```python
    n_classifiers = len(classifiers)
    
    for index, (name, classifier) in enumerate(classifiers.items()):
        classifier.fit(X_train, np.ravel(y_train))
    
        y_pred = classifier.predict(X_test)
        accuracy = accuracy_score(y_test, y_pred)
        print("Accuracy (train) for %s: %0.1f%% " % (name, accuracy * 100))
        print(classification_report(y_test,y_pred))
    ```

    نتیجہ کافی اچھا ہے:

    ```output
    Accuracy (train) for Linear SVC: 78.6% 
                  precision    recall  f1-score   support
    
         chinese       0.71      0.67      0.69       242
          indian       0.88      0.86      0.87       234
        japanese       0.79      0.74      0.76       254
          korean       0.85      0.81      0.83       242
            thai       0.71      0.86      0.78       227
    
        accuracy                           0.79      1199
       macro avg       0.79      0.79      0.79      1199
    weighted avg       0.79      0.79      0.79      1199
    ```

## K-Neighbors درجہ بندی کرنے والا

K-Neighbors مشین لرننگ کے "neighbors" خاندان کا حصہ ہے، جو سپروائزڈ اور غیر سپروائزڈ لرننگ دونوں کے لیے استعمال کیا جا سکتا ہے۔ اس طریقے میں، ایک پہلے سے طے شدہ تعداد میں پوائنٹس بنائے جاتے ہیں اور ڈیٹا ان پوائنٹس کے ارد گرد جمع کیا جاتا ہے تاکہ ڈیٹا کے لیے عمومی لیبلز کی پیش گوئی کی جا سکے۔

### مشق - K-Neighbors درجہ بندی کرنے والے کا اطلاق کریں

پچھلا درجہ بندی کرنے والا اچھا تھا اور ڈیٹا کے ساتھ اچھا کام کیا، لیکن شاید ہم بہتر درستگی حاصل کر سکیں۔ K-Neighbors درجہ بندی کرنے والا آزمائیں۔

1. اپنی درجہ بندی کرنے والے array میں ایک لائن شامل کریں (Linear SVC آئٹم کے بعد ایک کاما شامل کریں):

    ```python
    'KNN classifier': KNeighborsClassifier(C),
    ```

    نتیجہ تھوڑا خراب ہے:

    ```output
    Accuracy (train) for KNN classifier: 73.8% 
                  precision    recall  f1-score   support
    
         chinese       0.64      0.67      0.66       242
          indian       0.86      0.78      0.82       234
        japanese       0.66      0.83      0.74       254
          korean       0.94      0.58      0.72       242
            thai       0.71      0.82      0.76       227
    
        accuracy                           0.74      1199
       macro avg       0.76      0.74      0.74      1199
    weighted avg       0.76      0.74      0.74      1199
    ```

    ✅ [K-Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#neighbors) کے بارے میں جانیں

## Support Vector درجہ بندی کرنے والا

Support-Vector درجہ بندی کرنے والے مشین لرننگ کے [Support-Vector Machine](https://wikipedia.org/wiki/Support-vector_machine) خاندان کا حصہ ہیں، جو درجہ بندی اور رجریشن کے کاموں کے لیے استعمال ہوتے ہیں۔ SVMs "تربیتی مثالوں کو خلا میں پوائنٹس پر نقشہ بناتے ہیں" تاکہ دو زمروں کے درمیان فاصلہ زیادہ سے زیادہ ہو۔ بعد کا ڈیٹا اس خلا میں نقشہ بنایا جاتا ہے تاکہ اس کے زمرے کی پیش گوئی کی جا سکے۔

### مشق - Support Vector درجہ بندی کرنے والے کا اطلاق کریں

Support Vector درجہ بندی کرنے والے کے ساتھ تھوڑی بہتر درستگی حاصل کرنے کی کوشش کریں۔

1. K-Neighbors آئٹم کے بعد ایک کاما شامل کریں، اور پھر یہ لائن شامل کریں:

    ```python
    'SVC': SVC(),
    ```

    نتیجہ کافی اچھا ہے!

    ```output
    Accuracy (train) for SVC: 83.2% 
                  precision    recall  f1-score   support
    
         chinese       0.79      0.74      0.76       242
          indian       0.88      0.90      0.89       234
        japanese       0.87      0.81      0.84       254
          korean       0.91      0.82      0.86       242
            thai       0.74      0.90      0.81       227
    
        accuracy                           0.83      1199
       macro avg       0.84      0.83      0.83      1199
    weighted avg       0.84      0.83      0.83      1199
    ```

    ✅ [Support-Vectors](https://scikit-learn.org/stable/modules/svm.html#svm) کے بارے میں جانیں

## Ensemble درجہ بندی کرنے والے

چاہے پچھلا ٹیسٹ کافی اچھا تھا، آئیے راستے کے آخر تک چلتے ہیں۔ آئیے کچھ 'Ensemble Classifiers' آزمائیں، خاص طور پر Random Forest اور AdaBoost:

```python
  'RFST': RandomForestClassifier(n_estimators=100),
  'ADA': AdaBoostClassifier(n_estimators=100)
```

نتیجہ بہت اچھا ہے، خاص طور پر Random Forest کے لیے:

```output
Accuracy (train) for RFST: 84.5% 
              precision    recall  f1-score   support

     chinese       0.80      0.77      0.78       242
      indian       0.89      0.92      0.90       234
    japanese       0.86      0.84      0.85       254
      korean       0.88      0.83      0.85       242
        thai       0.80      0.87      0.83       227

    accuracy                           0.84      1199
   macro avg       0.85      0.85      0.84      1199
weighted avg       0.85      0.84      0.84      1199

Accuracy (train) for ADA: 72.4% 
              precision    recall  f1-score   support

     chinese       0.64      0.49      0.56       242
      indian       0.91      0.83      0.87       234
    japanese       0.68      0.69      0.69       254
      korean       0.73      0.79      0.76       242
        thai       0.67      0.83      0.74       227

    accuracy                           0.72      1199
   macro avg       0.73      0.73      0.72      1199
weighted avg       0.73      0.72      0.72      1199
```

✅ [Ensemble Classifiers](https://scikit-learn.org/stable/modules/ensemble.html) کے بارے میں جانیں

مشین لرننگ کا یہ طریقہ "کئی بنیادی تخمینے کے پیش گوئیوں کو یکجا کرتا ہے" تاکہ ماڈل کے معیار کو بہتر بنایا جا سکے۔ ہماری مثال میں، ہم نے Random Trees اور AdaBoost استعمال کیے۔

- [Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#forest)، ایک اوسطی طریقہ، 'decision trees' کا ایک 'forest' بناتا ہے جس میں randomness شامل ہوتی ہے تاکہ overfitting سے بچا جا سکے۔ n_estimators پیرامیٹر درختوں کی تعداد پر سیٹ کیا جاتا ہے۔

- [AdaBoost](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html) ایک ڈیٹا سیٹ پر درجہ بندی کرنے والے کو فٹ کرتا ہے اور پھر اس درجہ بندی کرنے والے کی کاپیاں اسی ڈیٹا سیٹ پر فٹ کرتا ہے۔ یہ غلط طور پر درجہ بندی کیے گئے آئٹمز کے وزن پر توجہ مرکوز کرتا ہے اور اگلے درجہ بندی کرنے والے کے فٹ کو درست کرنے کے لیے ایڈجسٹ کرتا ہے۔

---

## 🚀چیلنج

ان تکنیکوں میں سے ہر ایک کے پاس پیرامیٹرز کی ایک بڑی تعداد ہے جسے آپ ایڈجسٹ کر سکتے ہیں۔ ہر ایک کے ڈیفالٹ پیرامیٹرز پر تحقیق کریں اور سوچیں کہ ان پیرامیٹرز کو ایڈجسٹ کرنے سے ماڈل کے معیار پر کیا اثر پڑے گا۔

## [لیکچر کے بعد کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/24/)

## جائزہ اور خود مطالعہ

ان اسباق میں بہت زیادہ اصطلاحات ہیں، لہذا ایک لمحہ لیں اور [اس فہرست](https://docs.microsoft.com/dotnet/machine-learning/resources/glossary?WT.mc_id=academic-77952-leestott) کا جائزہ لیں جو مفید اصطلاحات پر مشتمل ہے!

## اسائنمنٹ 

[پیرامیٹر کھیل](assignment.md)

---

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے پوری کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستگی ہو سکتی ہیں۔ اصل دستاویز، جو اس کی اصل زبان میں ہے، کو مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے لیے ہم ذمہ دار نہیں ہیں۔