<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "9579f42e3ff5114c58379cc9e186a828",
  "translation_date": "2025-08-29T13:52:06+00:00",
  "source_file": "4-Classification/2-Classifiers-1/README.md",
  "language_code": "ar"
}
-->
# مصنفات المأكولات 1

في هذا الدرس، ستستخدم مجموعة البيانات التي قمت بحفظها من الدرس السابق، والتي تحتوي على بيانات متوازنة ونظيفة حول المأكولات.

ستستخدم هذه المجموعة من البيانات مع مجموعة متنوعة من المصنفات للتنبؤ بـ _نوع المطبخ الوطني بناءً على مجموعة من المكونات_. أثناء القيام بذلك، ستتعلم المزيد عن بعض الطرق التي يمكن من خلالها استخدام الخوارزميات في مهام التصنيف.

## [اختبار ما قبل المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/21/)
# التحضير

بافتراض أنك أكملت [الدرس الأول](../1-Introduction/README.md)، تأكد من وجود ملف _cleaned_cuisines.csv_ في المجلد الجذر `/data` لهذه الدروس الأربعة.

## تمرين - التنبؤ بنوع المطبخ الوطني

1. أثناء العمل في مجلد _notebook.ipynb_ الخاص بهذا الدرس، قم باستيراد هذا الملف مع مكتبة Pandas:

    ```python
    import pandas as pd
    cuisines_df = pd.read_csv("../data/cleaned_cuisines.csv")
    cuisines_df.head()
    ```

    تبدو البيانات كما يلي:

|     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
| --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
| 0   | 0          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 1   | 1          | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 2   | 2          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 3   | 3          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 4   | 4          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |
  

1. الآن، قم باستيراد المزيد من المكتبات:

    ```python
    from sklearn.linear_model import LogisticRegression
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    from sklearn.svm import SVC
    import numpy as np
    ```

1. قسّم إحداثيات X و y إلى إطارين بيانات للتدريب. يمكن أن تكون `cuisine` إطار البيانات الخاص بالتسميات:

    ```python
    cuisines_label_df = cuisines_df['cuisine']
    cuisines_label_df.head()
    ```

    ستبدو كما يلي:

    ```output
    0    indian
    1    indian
    2    indian
    3    indian
    4    indian
    Name: cuisine, dtype: object
    ```

1. قم بحذف العمود `Unnamed: 0` وعمود `cuisine` باستخدام `drop()`. احفظ باقي البيانات كميزات قابلة للتدريب:

    ```python
    cuisines_feature_df = cuisines_df.drop(['Unnamed: 0', 'cuisine'], axis=1)
    cuisines_feature_df.head()
    ```

    ستبدو الميزات كما يلي:

|      | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | artemisia | artichoke |  ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood |  yam | yeast | yogurt | zucchini |
| ---: | -----: | -------: | ----: | ---------: | ----: | -----------: | ------: | -------: | --------: | --------: | ---: | ------: | ----------: | ---------: | ----------------------: | ---: | ---: | ---: | ----: | -----: | -------: |
|    0 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    1 |      1 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    2 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    3 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    4 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      1 |        0 | 0 |

الآن أنت جاهز لتدريب النموذج الخاص بك!

## اختيار المصنف

الآن بعد أن أصبحت بياناتك نظيفة وجاهزة للتدريب، عليك أن تقرر أي خوارزمية ستستخدم لهذه المهمة.

تجمع مكتبة Scikit-learn التصنيف تحت التعلم الموجه، وفي هذه الفئة ستجد العديد من الطرق للتصنيف. [التنوع](https://scikit-learn.org/stable/supervised_learning.html) قد يبدو محيرًا في البداية. تشمل الطرق التالية تقنيات التصنيف:

- النماذج الخطية
- آلات الدعم المتجهة
- الانحدار العشوائي
- أقرب الجيران
- العمليات الغاوسية
- أشجار القرار
- طرق التجميع (التصويت)
- الخوارزميات متعددة الفئات ومتعددة المخرجات

> يمكنك أيضًا استخدام [الشبكات العصبية لتصنيف البيانات](https://scikit-learn.org/stable/modules/neural_networks_supervised.html#classification)، ولكن هذا خارج نطاق هذا الدرس.

### أي مصنف تختار؟

إذن، أي مصنف يجب أن تختار؟ غالبًا ما يكون تشغيل عدة مصنفات والبحث عن نتيجة جيدة طريقة لاختبار ذلك. تقدم مكتبة Scikit-learn [مقارنة جنبًا إلى جنب](https://scikit-learn.org/stable/auto_examples/classification/plot_classifier_comparison.html) على مجموعة بيانات تم إنشاؤها، تقارن بين KNeighbors، و SVC بطريقتين، و GaussianProcessClassifier، و DecisionTreeClassifier، و RandomForestClassifier، و MLPClassifier، و AdaBoostClassifier، و GaussianNB، و QuadraticDiscrinationAnalysis، مع عرض النتائج بشكل مرئي:

![مقارنة المصنفات](../../../../translated_images/comparison.edfab56193a85e7fdecbeaa1b1f8c99e94adbf7178bed0de902090cf93d6734f.ar.png)
> الرسوم البيانية مأخوذة من توثيق Scikit-learn

> AutoML يحل هذه المشكلة بشكل أنيق عن طريق تشغيل هذه المقارنات في السحابة، مما يسمح لك باختيار أفضل خوارزمية لبياناتك. جربه [هنا](https://docs.microsoft.com/learn/modules/automate-model-selection-with-azure-automl/?WT.mc_id=academic-77952-leestott)

### نهج أفضل

نهج أفضل من التخمين العشوائي هو اتباع الأفكار الموجودة في [ورقة الغش الخاصة بـ ML](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=academic-77952-leestott) القابلة للتنزيل. هنا، نكتشف أنه بالنسبة لمشكلتنا متعددة الفئات، لدينا بعض الخيارات:

![ورقة الغش للمشاكل متعددة الفئات](../../../../translated_images/cheatsheet.07a475ea444d22234cb8907a3826df5bdd1953efec94bd18e4496f36ff60624a.ar.png)
> قسم من ورقة الغش الخاصة بخوارزميات Microsoft، يوضح خيارات التصنيف متعددة الفئات

✅ قم بتنزيل ورقة الغش هذه، واطبعها، وعلقها على حائطك!

### التفكير المنطقي

دعونا نحاول التفكير في الطرق المختلفة بناءً على القيود التي لدينا:

- **الشبكات العصبية ثقيلة جدًا**. بالنظر إلى مجموعة البيانات النظيفة ولكن الصغيرة، وحقيقة أننا نقوم بتدريب النموذج محليًا عبر دفاتر الملاحظات، فإن الشبكات العصبية ثقيلة جدًا لهذه المهمة.
- **لا يوجد مصنف ثنائي الفئات**. نحن لا نستخدم مصنفًا ثنائي الفئات، لذا فإن ذلك يستبعد one-vs-all.
- **شجرة القرار أو الانحدار اللوجستي قد يعملان**. قد تعمل شجرة القرار، أو الانحدار اللوجستي للبيانات متعددة الفئات.
- **أشجار القرار المعززة متعددة الفئات تحل مشكلة مختلفة**. شجرة القرار المعززة متعددة الفئات أكثر ملاءمة للمهام غير المعلمية، مثل المهام المصممة لإنشاء تصنيفات، لذا فهي ليست مفيدة لنا.

### استخدام Scikit-learn

سنستخدم مكتبة Scikit-learn لتحليل بياناتنا. ومع ذلك، هناك العديد من الطرق لاستخدام الانحدار اللوجستي في Scikit-learn. ألقِ نظرة على [المعلمات التي يمكن تمريرها](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression).

بشكل أساسي، هناك معلمتان مهمتان - `multi_class` و `solver` - يجب تحديدهما عند طلب Scikit-learn لتنفيذ الانحدار اللوجستي. قيمة `multi_class` تطبق سلوكًا معينًا. وقيمة `solver` تحدد الخوارزمية المستخدمة. ليس كل الحلول يمكن إقرانها مع جميع قيم `multi_class`.

وفقًا للتوثيق، في حالة التصنيف متعدد الفئات، فإن خوارزمية التدريب:

- **تستخدم مخطط one-vs-rest (OvR)**، إذا تم تعيين خيار `multi_class` إلى `ovr`
- **تستخدم خسارة الانتروبي المتقاطع**، إذا تم تعيين خيار `multi_class` إلى `multinomial`. (حاليًا، خيار `multinomial` مدعوم فقط من قبل الحلول ‘lbfgs’، ‘sag’، ‘saga’ و ‘newton-cg’).

> 🎓 "المخطط" هنا يمكن أن يكون إما 'ovr' (one-vs-rest) أو 'multinomial'. نظرًا لأن الانحدار اللوجستي مصمم لدعم التصنيف الثنائي، فإن هذه المخططات تسمح له بالتعامل بشكل أفضل مع مهام التصنيف متعددة الفئات. [المصدر](https://machinelearningmastery.com/one-vs-rest-and-one-vs-one-for-multi-class-classification/)

> 🎓 "الحل" يُعرف بأنه "الخوارزمية المستخدمة في مشكلة التحسين". [المصدر](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression).

تقدم مكتبة Scikit-learn هذا الجدول لشرح كيفية تعامل الحلول مع التحديات المختلفة التي تقدمها أنواع البيانات المختلفة:

![الحلول](../../../../translated_images/solvers.5fc648618529e627dfac29b917b3ccabda4b45ee8ed41b0acb1ce1441e8d1ef1.ar.png)

## تمرين - تقسيم البيانات

يمكننا التركيز على الانحدار اللوجستي كأول تجربة تدريبية نظرًا لأنك تعلمت عنه مؤخرًا في درس سابق.
قم بتقسيم بياناتك إلى مجموعات تدريب واختبار عن طريق استدعاء `train_test_split()`:

```python
X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
```

## تمرين - تطبيق الانحدار اللوجستي

نظرًا لأنك تستخدم حالة التصنيف متعددة الفئات، تحتاج إلى اختيار _مخطط_ للاستخدام وتحديد _الحل_ الذي ستستخدمه. استخدم LogisticRegression مع إعداد متعدد الفئات والحل **liblinear** للتدريب.

1. قم بإنشاء انحدار لوجستي مع تعيين multi_class إلى `ovr` والحل إلى `liblinear`:

    ```python
    lr = LogisticRegression(multi_class='ovr',solver='liblinear')
    model = lr.fit(X_train, np.ravel(y_train))
    
    accuracy = model.score(X_test, y_test)
    print ("Accuracy is {}".format(accuracy))
    ```

    ✅ جرب حلاً مختلفًا مثل `lbfgs`، الذي يتم تعيينه غالبًا كإعداد افتراضي
> ملاحظة، استخدم وظيفة Pandas [`ravel`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.ravel.html) لتسطيح بياناتك عند الحاجة.
الدقة جيدة بنسبة تزيد عن **80%**!

1. يمكنك رؤية هذا النموذج أثناء العمل من خلال اختبار صف واحد من البيانات (#50):

    ```python
    print(f'ingredients: {X_test.iloc[50][X_test.iloc[50]!=0].keys()}')
    print(f'cuisine: {y_test.iloc[50]}')
    ```

    يتم طباعة النتيجة:

   ```output
   ingredients: Index(['cilantro', 'onion', 'pea', 'potato', 'tomato', 'vegetable_oil'], dtype='object')
   cuisine: indian
   ```

   ✅ جرّب رقم صف مختلف وتحقق من النتائج.

1. بالتعمق أكثر، يمكنك التحقق من دقة هذا التنبؤ:

    ```python
    test= X_test.iloc[50].values.reshape(-1, 1).T
    proba = model.predict_proba(test)
    classes = model.classes_
    resultdf = pd.DataFrame(data=proba, columns=classes)
    
    topPrediction = resultdf.T.sort_values(by=[0], ascending = [False])
    topPrediction.head()
    ```

    يتم طباعة النتيجة - المطبخ الهندي هو أفضل تخمين للنموذج، مع احتمال جيد:

    |          |        0 |
    | -------: | -------: |
    |   indian | 0.715851 |
    |  chinese | 0.229475 |
    | japanese | 0.029763 |
    |   korean | 0.017277 |
    |     thai | 0.007634 |

    ✅ هل يمكنك تفسير لماذا النموذج متأكد إلى حد كبير أن هذا مطبخ هندي؟

1. احصل على مزيد من التفاصيل عن طريق طباعة تقرير التصنيف، كما فعلت في دروس الانحدار:

    ```python
    y_pred = model.predict(X_test)
    print(classification_report(y_test,y_pred))
    ```

    |              | الدقة | الاسترجاع | f1-score | الدعم |
    | ------------ | ------ | --------- | -------- | ----- |
    | chinese      | 0.73   | 0.71      | 0.72     | 229   |
    | indian       | 0.91   | 0.93      | 0.92     | 254   |
    | japanese     | 0.70   | 0.75      | 0.72     | 220   |
    | korean       | 0.86   | 0.76      | 0.81     | 242   |
    | thai         | 0.79   | 0.85      | 0.82     | 254   |
    | الدقة        | 0.80   | 1199      |          |       |
    | المتوسط الكلي | 0.80   | 0.80      | 0.80     | 1199  |
    | المتوسط الموزون | 0.80   | 0.80      | 0.80     | 1199  |

## 🚀التحدي

في هذا الدرس، استخدمت بياناتك المنظفة لبناء نموذج تعلم آلي يمكنه التنبؤ بالمطبخ الوطني بناءً على سلسلة من المكونات. خذ بعض الوقت لقراءة الخيارات العديدة التي يوفرها Scikit-learn لتصنيف البيانات. تعمق أكثر في مفهوم 'solver' لفهم ما يحدث خلف الكواليس.

## [اختبار ما بعد المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/22/)

## المراجعة والدراسة الذاتية

تعمق أكثر في الرياضيات وراء الانحدار اللوجستي في [هذا الدرس](https://people.eecs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf)
## الواجب 

[ادرس الحلول](assignment.md)

---

**إخلاء المسؤولية**:  
تم ترجمة هذا المستند باستخدام خدمة الترجمة الآلية [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو معلومات غير دقيقة. يجب اعتبار المستند الأصلي بلغته الأصلية هو المصدر الموثوق. للحصول على معلومات حساسة أو هامة، يُوصى بالاستعانة بترجمة بشرية احترافية. نحن غير مسؤولين عن أي سوء فهم أو تفسيرات خاطئة ناتجة عن استخدام هذه الترجمة.