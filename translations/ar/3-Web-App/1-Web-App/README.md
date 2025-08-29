<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "2680c691fbdb6367f350761a275e2508",
  "translation_date": "2025-08-29T13:48:01+00:00",
  "source_file": "3-Web-App/1-Web-App/README.md",
  "language_code": "ar"
}
-->
# بناء تطبيق ويب لاستخدام نموذج تعلم الآلة

في هذا الدرس، ستقوم بتدريب نموذج تعلم آلي على مجموعة بيانات غير تقليدية: _مشاهدات الأجسام الطائرة المجهولة (UFO) خلال القرن الماضي_، والمأخوذة من قاعدة بيانات NUFORC.

ستتعلم:

- كيفية "تخزين" نموذج مدرب باستخدام Pickle
- كيفية استخدام هذا النموذج في تطبيق Flask

سنواصل استخدام دفاتر Jupyter لتنظيف البيانات وتدريب النموذج، ولكن يمكنك أن تأخذ العملية خطوة إضافية من خلال استكشاف استخدام النموذج "في العالم الحقيقي"، أي في تطبيق ويب.

للقيام بذلك، تحتاج إلى بناء تطبيق ويب باستخدام Flask.

## [اختبار ما قبل المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/17/)

## بناء التطبيق

هناك عدة طرق لبناء تطبيقات ويب لاستهلاك نماذج تعلم الآلة. قد تؤثر بنية الويب الخاصة بك على الطريقة التي يتم بها تدريب النموذج. تخيل أنك تعمل في شركة حيث قامت مجموعة علوم البيانات بتدريب نموذج يريدون منك استخدامه في تطبيق.

### اعتبارات

هناك العديد من الأسئلة التي تحتاج إلى طرحها:

- **هل هو تطبيق ويب أم تطبيق جوال؟** إذا كنت تبني تطبيقًا جوالًا أو تحتاج إلى استخدام النموذج في سياق إنترنت الأشياء (IoT)، يمكنك استخدام [TensorFlow Lite](https://www.tensorflow.org/lite/) واستخدام النموذج في تطبيق Android أو iOS.
- **أين سيقيم النموذج؟** في السحابة أم محليًا؟
- **الدعم دون اتصال.** هل يجب أن يعمل التطبيق دون اتصال؟
- **ما هي التقنية المستخدمة لتدريب النموذج؟** قد تؤثر التقنية المختارة على الأدوات التي تحتاج إلى استخدامها.
    - **استخدام TensorFlow.** إذا كنت تدرب نموذجًا باستخدام TensorFlow، على سبيل المثال، فإن هذا النظام يوفر إمكانية تحويل نموذج TensorFlow لاستخدامه في تطبيق ويب باستخدام [TensorFlow.js](https://www.tensorflow.org/js/).
    - **استخدام PyTorch.** إذا كنت تبني نموذجًا باستخدام مكتبة مثل [PyTorch](https://pytorch.org/)، لديك خيار تصديره بتنسيق [ONNX](https://onnx.ai/) (Open Neural Network Exchange) لاستخدامه في تطبيقات ويب JavaScript التي يمكنها استخدام [Onnx Runtime](https://www.onnxruntime.ai/). سيتم استكشاف هذا الخيار في درس مستقبلي لنموذج مدرب باستخدام Scikit-learn.
    - **استخدام Lobe.ai أو Azure Custom Vision.** إذا كنت تستخدم نظام تعلم آلي كخدمة (ML SaaS) مثل [Lobe.ai](https://lobe.ai/) أو [Azure Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/?WT.mc_id=academic-77952-leestott) لتدريب نموذج، فإن هذا النوع من البرمجيات يوفر طرقًا لتصدير النموذج للعديد من المنصات، بما في ذلك بناء واجهة برمجية مخصصة يمكن استدعاؤها في السحابة بواسطة تطبيقك عبر الإنترنت.

لديك أيضًا فرصة لبناء تطبيق ويب كامل باستخدام Flask يمكنه تدريب النموذج نفسه في متصفح الويب. يمكن القيام بذلك أيضًا باستخدام TensorFlow.js في سياق JavaScript.

بالنسبة لأغراضنا، بما أننا عملنا مع دفاتر Jupyter المستندة إلى Python، دعنا نستكشف الخطوات التي تحتاج إلى اتخاذها لتصدير نموذج مدرب من دفتر ملاحظات إلى تنسيق يمكن قراءته بواسطة تطبيق ويب مبني باستخدام Python.

## الأدوات

لهذه المهمة، تحتاج إلى أداتين: Flask وPickle، وكلاهما يعملان على Python.

✅ ما هو [Flask](https://palletsprojects.com/p/flask/)؟ يُعرف بأنه "إطار عمل صغير" من قبل مطوريه، يوفر Flask الميزات الأساسية لإطارات عمل الويب باستخدام Python ومحرك قوالب لبناء صفحات الويب. ألقِ نظرة على [هذا الدرس](https://docs.microsoft.com/learn/modules/python-flask-build-ai-web-app?WT.mc_id=academic-77952-leestott) لتتعلم كيفية بناء تطبيقات باستخدام Flask.

✅ ما هو [Pickle](https://docs.python.org/3/library/pickle.html)؟ Pickle 🥒 هو وحدة Python تقوم بتسلسل وفك تسلسل هيكل كائن Python. عندما تقوم "بتخزين" نموذج، فإنك تقوم بتسلسل أو تسطيح هيكله لاستخدامه على الويب. كن حذرًا: Pickle ليس آمنًا بطبيعته، لذا كن حذرًا إذا طُلب منك "فك تخزين" ملف. الملف المخزن يحتوي على الامتداد `.pkl`.

## تمرين - تنظيف البيانات

في هذا الدرس، ستستخدم بيانات من 80,000 مشاهدة للأجسام الطائرة المجهولة، التي جمعها [NUFORC](https://nuforc.org) (المركز الوطني لتقارير الأجسام الطائرة المجهولة). تحتوي هذه البيانات على أوصاف مثيرة للاهتمام لمشاهدات الأجسام الطائرة المجهولة، على سبيل المثال:

- **وصف طويل كمثال.** "رجل يظهر من شعاع ضوء يضيء على حقل عشبي في الليل ويركض نحو موقف سيارات Texas Instruments".
- **وصف قصير كمثال.** "الأضواء طاردتنا".

تتضمن ورقة البيانات [ufos.csv](../../../../3-Web-App/1-Web-App/data/ufos.csv) أعمدة حول `المدينة`، `الولاية` و`الدولة` التي حدثت فيها المشاهدة، شكل الجسم الطائر (`shape`) وخطوط الطول والعرض (`latitude` و`longitude`).

في [دفتر الملاحظات](notebook.ipynb) الفارغ المرفق في هذا الدرس:

1. قم باستيراد `pandas`، `matplotlib`، و`numpy` كما فعلت في الدروس السابقة واستورد ورقة بيانات الأجسام الطائرة المجهولة. يمكنك إلقاء نظرة على عينة من مجموعة البيانات:

    ```python
    import pandas as pd
    import numpy as np
    
    ufos = pd.read_csv('./data/ufos.csv')
    ufos.head()
    ```

1. قم بتحويل بيانات الأجسام الطائرة إلى إطار بيانات صغير مع عناوين جديدة. تحقق من القيم الفريدة في حقل `Country`.

    ```python
    ufos = pd.DataFrame({'Seconds': ufos['duration (seconds)'], 'Country': ufos['country'],'Latitude': ufos['latitude'],'Longitude': ufos['longitude']})
    
    ufos.Country.unique()
    ```

1. الآن، يمكنك تقليل كمية البيانات التي نحتاج إلى التعامل معها عن طريق حذف أي قيم فارغة واستيراد المشاهدات التي تتراوح مدتها بين 1-60 ثانية فقط:

    ```python
    ufos.dropna(inplace=True)
    
    ufos = ufos[(ufos['Seconds'] >= 1) & (ufos['Seconds'] <= 60)]
    
    ufos.info()
    ```

1. استورد مكتبة `LabelEncoder` من Scikit-learn لتحويل القيم النصية للدول إلى أرقام:

    ✅ يقوم LabelEncoder بترميز البيانات أبجديًا

    ```python
    from sklearn.preprocessing import LabelEncoder
    
    ufos['Country'] = LabelEncoder().fit_transform(ufos['Country'])
    
    ufos.head()
    ```

    يجب أن تبدو بياناتك كما يلي:

    ```output
    	Seconds	Country	Latitude	Longitude
    2	20.0	3		53.200000	-2.916667
    3	20.0	4		28.978333	-96.645833
    14	30.0	4		35.823889	-80.253611
    23	60.0	4		45.582778	-122.352222
    24	3.0		3		51.783333	-0.783333
    ```

## تمرين - بناء النموذج

الآن يمكنك الاستعداد لتدريب نموذج عن طريق تقسيم البيانات إلى مجموعة تدريب واختبار.

1. اختر ثلاث ميزات تريد التدريب عليها كمتجه X، وسيكون المتجه y هو `Country`. تريد أن تكون قادرًا على إدخال `Seconds`، `Latitude` و`Longitude` والحصول على معرف الدولة كإخراج.

    ```python
    from sklearn.model_selection import train_test_split
    
    Selected_features = ['Seconds','Latitude','Longitude']
    
    X = ufos[Selected_features]
    y = ufos['Country']
    
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
    ```

1. قم بتدريب النموذج باستخدام الانحدار اللوجستي:

    ```python
    from sklearn.metrics import accuracy_score, classification_report
    from sklearn.linear_model import LogisticRegression
    model = LogisticRegression()
    model.fit(X_train, y_train)
    predictions = model.predict(X_test)
    
    print(classification_report(y_test, predictions))
    print('Predicted labels: ', predictions)
    print('Accuracy: ', accuracy_score(y_test, predictions))
    ```

الدقة ليست سيئة **(حوالي 95%)**، وهذا ليس مفاجئًا، حيث أن `Country` و`Latitude/Longitude` مترابطان.

النموذج الذي أنشأته ليس ثوريًا جدًا حيث يجب أن تكون قادرًا على استنتاج `Country` من `Latitude` و`Longitude`، ولكنه تمرين جيد لمحاولة التدريب على بيانات خام قمت بتنظيفها وتصديرها، ثم استخدام هذا النموذج في تطبيق ويب.

## تمرين - "تخزين" النموذج

الآن، حان الوقت لتخزين النموذج! يمكنك القيام بذلك في بضع أسطر من التعليمات البرمجية. بمجرد تخزينه، قم بتحميل النموذج المخزن واختبره مقابل مصفوفة بيانات عينة تحتوي على قيم للثواني، خط العرض وخط الطول:

```python
import pickle
model_filename = 'ufo-model.pkl'
pickle.dump(model, open(model_filename,'wb'))

model = pickle.load(open('ufo-model.pkl','rb'))
print(model.predict([[50,44,-12]]))
```

يُرجع النموذج **'3'**، وهو رمز الدولة للمملكة المتحدة. مذهل! 👽

## تمرين - بناء تطبيق Flask

الآن يمكنك بناء تطبيق Flask لاستدعاء النموذج وإرجاع نتائج مشابهة، ولكن بطريقة أكثر جاذبية بصريًا.

1. ابدأ بإنشاء مجلد يسمى **web-app** بجانب ملف _notebook.ipynb_ حيث يوجد ملف _ufo-model.pkl_.

1. في هذا المجلد، قم بإنشاء ثلاثة مجلدات أخرى: **static**، مع مجلد **css** بداخله، و**templates**. يجب أن يكون لديك الآن الملفات والمجلدات التالية:

    ```output
    web-app/
      static/
        css/
      templates/
    notebook.ipynb
    ufo-model.pkl
    ```

    ✅ ارجع إلى مجلد الحل لرؤية التطبيق النهائي

1. أول ملف تقوم بإنشائه في مجلد _web-app_ هو ملف **requirements.txt**. مثل _package.json_ في تطبيق JavaScript، يسرد هذا الملف التبعيات المطلوبة للتطبيق. في **requirements.txt** أضف الأسطر:

    ```text
    scikit-learn
    pandas
    numpy
    flask
    ```

1. الآن، قم بتشغيل هذا الملف عن طريق الانتقال إلى _web-app_:

    ```bash
    cd web-app
    ```

1. في الطرفية الخاصة بك، اكتب `pip install` لتثبيت المكتبات المدرجة في _requirements.txt_:

    ```bash
    pip install -r requirements.txt
    ```

1. الآن، أنت جاهز لإنشاء ثلاثة ملفات أخرى لإنهاء التطبيق:

    1. أنشئ ملف **app.py** في الجذر.
    2. أنشئ ملف **index.html** في مجلد _templates_.
    3. أنشئ ملف **styles.css** في مجلد _static/css_.

1. قم ببناء ملف _styles.css_ ببعض الأنماط:

    ```css
    body {
    	width: 100%;
    	height: 100%;
    	font-family: 'Helvetica';
    	background: black;
    	color: #fff;
    	text-align: center;
    	letter-spacing: 1.4px;
    	font-size: 30px;
    }
    
    input {
    	min-width: 150px;
    }
    
    .grid {
    	width: 300px;
    	border: 1px solid #2d2d2d;
    	display: grid;
    	justify-content: center;
    	margin: 20px auto;
    }
    
    .box {
    	color: #fff;
    	background: #2d2d2d;
    	padding: 12px;
    	display: inline-block;
    }
    ```

1. بعد ذلك، قم ببناء ملف _index.html_:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8">
        <title>🛸 UFO Appearance Prediction! 👽</title>
        <link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
      </head>
    
      <body>
        <div class="grid">
    
          <div class="box">
    
            <p>According to the number of seconds, latitude and longitude, which country is likely to have reported seeing a UFO?</p>
    
            <form action="{{ url_for('predict')}}" method="post">
              <input type="number" name="seconds" placeholder="Seconds" required="required" min="0" max="60" />
              <input type="text" name="latitude" placeholder="Latitude" required="required" />
              <input type="text" name="longitude" placeholder="Longitude" required="required" />
              <button type="submit" class="btn">Predict country where the UFO is seen</button>
            </form>
    
            <p>{{ prediction_text }}</p>
    
          </div>
    
        </div>
    
      </body>
    </html>
    ```

    ألقِ نظرة على القوالب في هذا الملف. لاحظ صياغة "mustache" حول المتغيرات التي سيتم توفيرها بواسطة التطبيق، مثل نص التنبؤ: `{{}}`. هناك أيضًا نموذج يقوم بإرسال تنبؤ إلى مسار `/predict`.

    أخيرًا، أنت جاهز لبناء ملف Python الذي يدير استهلاك النموذج وعرض التنبؤات:

1. في `app.py` أضف:

    ```python
    import numpy as np
    from flask import Flask, request, render_template
    import pickle
    
    app = Flask(__name__)
    
    model = pickle.load(open("./ufo-model.pkl", "rb"))
    
    
    @app.route("/")
    def home():
        return render_template("index.html")
    
    
    @app.route("/predict", methods=["POST"])
    def predict():
    
        int_features = [int(x) for x in request.form.values()]
        final_features = [np.array(int_features)]
        prediction = model.predict(final_features)
    
        output = prediction[0]
    
        countries = ["Australia", "Canada", "Germany", "UK", "US"]
    
        return render_template(
            "index.html", prediction_text="Likely country: {}".format(countries[output])
        )
    
    
    if __name__ == "__main__":
        app.run(debug=True)
    ```

    > 💡 نصيحة: عند إضافة [`debug=True`](https://www.askpython.com/python-modules/flask/flask-debug-mode) أثناء تشغيل تطبيق الويب باستخدام Flask، سيتم عكس أي تغييرات تجريها على التطبيق فورًا دون الحاجة إلى إعادة تشغيل الخادم. احذر! لا تقم بتمكين هذا الوضع في تطبيق الإنتاج.

إذا قمت بتشغيل `python app.py` أو `python3 app.py` - يبدأ خادم الويب الخاص بك محليًا، ويمكنك ملء نموذج قصير للحصول على إجابة لسؤالك الملح حول أماكن مشاهدة الأجسام الطائرة المجهولة!

قبل القيام بذلك، ألقِ نظرة على أجزاء `app.py`:

1. أولاً، يتم تحميل التبعيات وبدء التطبيق.
1. ثم يتم استيراد النموذج.
1. ثم يتم عرض index.html على المسار الرئيسي.

على مسار `/predict`، تحدث عدة أشياء عند إرسال النموذج:

1. يتم جمع متغيرات النموذج وتحويلها إلى مصفوفة numpy. ثم يتم إرسالها إلى النموذج ويتم إرجاع تنبؤ.
2. يتم إعادة عرض الدول التي نريد عرضها كنصوص قابلة للقراءة من رمز الدولة المتوقع، ويتم إرسال تلك القيمة مرة أخرى إلى index.html ليتم عرضها في القالب.

استخدام النموذج بهذه الطريقة، مع Flask ونموذج مخزن، هو أمر بسيط نسبيًا. أصعب شيء هو فهم شكل البيانات التي يجب إرسالها إلى النموذج للحصول على تنبؤ. يعتمد ذلك كله على كيفية تدريب النموذج. يحتوي هذا النموذج على ثلاث نقاط بيانات يجب إدخالها للحصول على تنبؤ.

في بيئة احترافية، يمكنك أن ترى كيف أن التواصل الجيد ضروري بين الأشخاص الذين يدربون النموذج وأولئك الذين يستهلكونه في تطبيق ويب أو جوال. في حالتنا، الشخص الوحيد هو أنت!

---

## 🚀 تحدي

بدلاً من العمل في دفتر ملاحظات واستيراد النموذج إلى تطبيق Flask، يمكنك تدريب النموذج مباشرة داخل تطبيق Flask! حاول تحويل كود Python الخاص بك في دفتر الملاحظات، ربما بعد تنظيف البيانات، لتدريب النموذج من داخل التطبيق على مسار يسمى `train`. ما هي إيجابيات وسلبيات متابعة هذه الطريقة؟

## [اختبار ما بعد المحاضرة](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/18/)

## المراجعة والدراسة الذاتية

هناك العديد من الطرق لبناء تطبيق ويب لاستهلاك نماذج تعلم الآلة. قم بعمل قائمة بالطرق التي يمكنك بها استخدام JavaScript أو Python لبناء تطبيق ويب للاستفادة من تعلم الآلة. فكر في البنية: هل يجب أن يبقى النموذج في التطبيق أم يعيش في السحابة؟ إذا كان الخيار الأخير، كيف ستصل إليه؟ ارسم نموذجًا معماريًا لحل تعلم الآلة المطبق على الويب.

## الواجب

[جرب نموذجًا مختلفًا](assignment.md)

---

**إخلاء المسؤولية**:  
تم ترجمة هذا المستند باستخدام خدمة الترجمة بالذكاء الاصطناعي [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو معلومات غير دقيقة. يجب اعتبار المستند الأصلي بلغته الأصلية المصدر الرسمي. للحصول على معلومات حاسمة، يُوصى بالاستعانة بترجمة بشرية احترافية. نحن غير مسؤولين عن أي سوء فهم أو تفسيرات خاطئة ناتجة عن استخدام هذه الترجمة.