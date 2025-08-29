<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "808a71076f76ae8f5458862a8edd9215",
  "translation_date": "2025-08-29T13:57:56+00:00",
  "source_file": "4-Classification/3-Classifiers-2/README.md",
  "language_code": "ar"
}
-->
# مصنفات المأكولات 2

في درس التصنيف الثاني هذا، ستستكشف المزيد من الطرق لتصنيف البيانات الرقمية. ستتعلم أيضًا العواقب المترتبة على اختيار مصنف معين بدلاً من الآخر.

## [اختبار ما قبل المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/23/)

### المتطلبات الأساسية

نفترض أنك قد أكملت الدروس السابقة ولديك مجموعة بيانات نظيفة في مجلد `data` باسم _cleaned_cuisines.csv_ في جذر هذا المجلد المكون من 4 دروس.

### التحضير

قمنا بتحميل ملف _notebook.ipynb_ الخاص بك مع مجموعة البيانات النظيفة وقمنا بتقسيمها إلى إطاري بيانات X و y، جاهزة لعملية بناء النموذج.

## خريطة التصنيف

في السابق، تعلمت عن الخيارات المختلفة المتاحة لتصنيف البيانات باستخدام ورقة الغش الخاصة بـ Microsoft. تقدم مكتبة Scikit-learn ورقة غش مشابهة ولكن أكثر تفصيلًا يمكن أن تساعدك بشكل أكبر في تضييق نطاق المصنفات (مصطلح آخر للمصنفات):

![خريطة تعلم الآلة من Scikit-learn](../../../../translated_images/map.e963a6a51349425ab107b38f6c7307eb4c0d0c7ccdd2e81a5e1919292bab9ac7.ar.png)
> نصيحة: [قم بزيارة هذه الخريطة عبر الإنترنت](https://scikit-learn.org/stable/tutorial/machine_learning_map/) وانقر على المسار لقراءة الوثائق.

### الخطة

هذه الخريطة مفيدة جدًا بمجرد أن تكون لديك فهم واضح لبياناتك، حيث يمكنك "السير" على طول مساراتها لاتخاذ قرار:

- لدينا >50 عينة
- نريد التنبؤ بفئة
- لدينا بيانات معنونة
- لدينا أقل من 100 ألف عينة
- ✨ يمكننا اختيار Linear SVC
- إذا لم ينجح ذلك، نظرًا لأن لدينا بيانات رقمية
    - يمكننا تجربة ✨ KNeighbors Classifier 
      - إذا لم ينجح ذلك، جرب ✨ SVC و ✨ Ensemble Classifiers

هذا مسار مفيد جدًا للمتابعة.

## تمرين - تقسيم البيانات

وفقًا لهذا المسار، يجب أن نبدأ باستيراد بعض المكتبات اللازمة.

1. قم باستيراد المكتبات المطلوبة:

    ```python
    from sklearn.neighbors import KNeighborsClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.svm import SVC
    from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    import numpy as np
    ```

1. قم بتقسيم بيانات التدريب والاختبار:

    ```python
    X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
    ```

## مصنف Linear SVC

التجميع باستخدام Support-Vector (SVC) هو جزء من عائلة تقنيات تعلم الآلة Support-Vector Machines (تعرف على المزيد عنها أدناه). في هذه الطريقة، يمكنك اختيار "kernel" لتحديد كيفية تجميع العلامات. يشير المعامل 'C' إلى "التنظيم" الذي ينظم تأثير المعاملات. يمكن أن يكون kernel واحدًا من [عدة خيارات](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC)؛ هنا نضبطه على 'linear' لضمان استخدام Linear SVC. يتم ضبط الاحتمالية افتراضيًا على 'false'؛ هنا نضبطها على 'true' للحصول على تقديرات الاحتمالية. نضبط الحالة العشوائية على '0' لخلط البيانات للحصول على الاحتمالات.

### تمرين - تطبيق Linear SVC

ابدأ بإنشاء مصفوفة من المصنفات. ستضيف تدريجيًا إلى هذه المصفوفة أثناء الاختبار.

1. ابدأ بـ Linear SVC:

    ```python
    C = 10
    # Create different classifiers.
    classifiers = {
        'Linear SVC': SVC(kernel='linear', C=C, probability=True,random_state=0)
    }
    ```

2. قم بتدريب النموذج باستخدام Linear SVC واطبع التقرير:

    ```python
    n_classifiers = len(classifiers)
    
    for index, (name, classifier) in enumerate(classifiers.items()):
        classifier.fit(X_train, np.ravel(y_train))
    
        y_pred = classifier.predict(X_test)
        accuracy = accuracy_score(y_test, y_pred)
        print("Accuracy (train) for %s: %0.1f%% " % (name, accuracy * 100))
        print(classification_report(y_test,y_pred))
    ```

    النتيجة جيدة جدًا:

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

## مصنف K-Neighbors

K-Neighbors هو جزء من عائلة طرق تعلم الآلة "الجيران"، والتي يمكن استخدامها للتعلم الموجه وغير الموجه. في هذه الطريقة، يتم إنشاء عدد محدد مسبقًا من النقاط ويتم جمع البيانات حول هذه النقاط بحيث يمكن التنبؤ بالعلامات العامة للبيانات.

### تمرين - تطبيق مصنف K-Neighbors

كان المصنف السابق جيدًا وعمل بشكل جيد مع البيانات، ولكن ربما يمكننا الحصول على دقة أفضل. جرب مصنف K-Neighbors.

1. أضف سطرًا إلى مصفوفة المصنفات (أضف فاصلة بعد عنصر Linear SVC):

    ```python
    'KNN classifier': KNeighborsClassifier(C),
    ```

    النتيجة أسوأ قليلاً:

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

    ✅ تعرف على [K-Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#neighbors)

## مصنف Support Vector

مصنفات Support-Vector هي جزء من عائلة [Support-Vector Machine](https://wikipedia.org/wiki/Support-vector_machine) من طرق تعلم الآلة التي تُستخدم لمهام التصنيف والانحدار. تقوم SVMs "برسم أمثلة التدريب كنقاط في الفضاء" لزيادة المسافة بين فئتين. يتم رسم البيانات اللاحقة في هذا الفضاء بحيث يمكن التنبؤ بفئتها.

### تمرين - تطبيق مصنف Support Vector

دعنا نحاول الحصول على دقة أفضل قليلاً باستخدام مصنف Support Vector.

1. أضف فاصلة بعد عنصر K-Neighbors، ثم أضف هذا السطر:

    ```python
    'SVC': SVC(),
    ```

    النتيجة جيدة جدًا!

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

    ✅ تعرف على [Support-Vectors](https://scikit-learn.org/stable/modules/svm.html#svm)

## المصنفات التجميعية

دعنا نتبع المسار حتى النهاية، على الرغم من أن الاختبار السابق كان جيدًا جدًا. دعنا نجرب بعض المصنفات التجميعية، تحديدًا Random Forest و AdaBoost:

```python
  'RFST': RandomForestClassifier(n_estimators=100),
  'ADA': AdaBoostClassifier(n_estimators=100)
```

النتيجة جيدة جدًا، خاصة لـ Random Forest:

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

✅ تعرف على [المصنفات التجميعية](https://scikit-learn.org/stable/modules/ensemble.html)

هذه الطريقة في تعلم الآلة "تجمع توقعات عدة مصنفات أساسية" لتحسين جودة النموذج. في مثالنا، استخدمنا Random Trees و AdaBoost.

- [Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#forest)، وهي طريقة تعتمد على المتوسط، تبني "غابة" من "أشجار القرار" مع إضافة عشوائية لتجنب الإفراط في التخصيص. يتم ضبط معامل n_estimators على عدد الأشجار.

- [AdaBoost](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html) يقوم بتطبيق مصنف على مجموعة البيانات ثم تطبيق نسخ من هذا المصنف على نفس مجموعة البيانات. يركز على أوزان العناصر التي تم تصنيفها بشكل غير صحيح ويعدل التخصيص للمصنف التالي لتصحيحها.

---

## 🚀التحدي

كل واحدة من هذه التقنيات لديها عدد كبير من المعاملات التي يمكنك تعديلها. قم بالبحث عن المعاملات الافتراضية لكل تقنية وفكر في ما يعنيه تعديل هذه المعاملات لجودة النموذج.

## [اختبار ما بعد المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/24/)

## المراجعة والدراسة الذاتية

هناك الكثير من المصطلحات في هذه الدروس، لذا خذ دقيقة لمراجعة [هذه القائمة](https://docs.microsoft.com/dotnet/machine-learning/resources/glossary?WT.mc_id=academic-77952-leestott) من المصطلحات المفيدة!

## الواجب 

[لعب المعاملات](assignment.md)

---

**إخلاء المسؤولية**:  
تم ترجمة هذا المستند باستخدام خدمة الترجمة بالذكاء الاصطناعي [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو معلومات غير دقيقة. يجب اعتبار المستند الأصلي بلغته الأصلية المصدر الموثوق. للحصول على معلومات حاسمة، يُوصى بالاستعانة بترجمة بشرية احترافية. نحن غير مسؤولين عن أي سوء فهم أو تفسيرات خاطئة تنشأ عن استخدام هذه الترجمة.