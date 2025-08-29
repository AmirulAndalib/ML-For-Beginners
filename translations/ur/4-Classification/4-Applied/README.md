<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "ad2cf19d7490247558d20a6a59650d13",
  "translation_date": "2025-08-29T13:56:25+00:00",
  "source_file": "4-Classification/4-Applied/README.md",
  "language_code": "ur"
}
-->
# کھانے کی سفارش کرنے والی ویب ایپ بنائیں

اس سبق میں، آپ ایک درجہ بندی ماڈل بنائیں گے، جو آپ نے پچھلے اسباق میں سیکھے گئے تکنیکوں اور اس سیریز میں استعمال ہونے والے مزیدار کھانے کے ڈیٹا سیٹ کے ذریعے بنایا جائے گا۔ اس کے علاوہ، آپ ایک چھوٹی ویب ایپ بنائیں گے جو محفوظ شدہ ماڈل کو استعمال کرے گی، اور Onnx کے ویب رن ٹائم کا فائدہ اٹھائے گی۔

مشین لرننگ کے سب سے مفید عملی استعمال میں سے ایک سفارش کرنے والے نظام بنانا ہے، اور آپ آج اس سمت میں پہلا قدم اٹھا سکتے ہیں!

> 🎥 اوپر دی گئی تصویر پر کلک کریں: جین لوپر کھانے کے ڈیٹا کو استعمال کرتے ہوئے ایک ویب ایپ بناتی ہیں

## [سبق سے پہلے کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/25/)

اس سبق میں آپ سیکھیں گے:

- ماڈل کیسے بنائیں اور اسے Onnx ماڈل کے طور پر محفوظ کریں
- Netron کا استعمال کرکے ماڈل کا معائنہ کیسے کریں
- اپنے ماڈل کو ویب ایپ میں انفرینس کے لیے کیسے استعمال کریں

## اپنا ماڈل بنائیں

مشین لرننگ کے عملی نظام بنانا ان ٹیکنالوجیز کو اپنے کاروباری نظاموں میں استعمال کرنے کا ایک اہم حصہ ہے۔ آپ اپنے ماڈلز کو ویب ایپلیکیشنز میں استعمال کر سکتے ہیں (اور اگر ضرورت ہو تو آف لائن سیاق و سباق میں بھی) Onnx کا استعمال کرکے۔

ایک [پچھلے سبق](../../3-Web-App/1-Web-App/README.md) میں، آپ نے UFO مشاہدات کے بارے میں ایک ریگریشن ماڈل بنایا، اسے "پکل" کیا، اور اسے Flask ایپ میں استعمال کیا۔ اگرچہ یہ فن تعمیر جاننا بہت مفید ہے، یہ ایک مکمل اسٹیک Python ایپ ہے، اور آپ کی ضروریات میں JavaScript ایپلیکیشن کا استعمال شامل ہو سکتا ہے۔

اس سبق میں، آپ انفرینس کے لیے ایک بنیادی JavaScript پر مبنی نظام بنا سکتے ہیں۔ لیکن پہلے، آپ کو ایک ماڈل تربیت دینا ہوگا اور اسے Onnx کے ساتھ استعمال کے لیے تبدیل کرنا ہوگا۔

## مشق - درجہ بندی ماڈل کی تربیت کریں

سب سے پہلے، صاف شدہ کھانے کے ڈیٹا سیٹ کا استعمال کرتے ہوئے ایک درجہ بندی ماڈل تربیت کریں۔

1. مفید لائبریریاں درآمد کریں:

    ```python
    !pip install skl2onnx
    import pandas as pd 
    ```

    آپ کو '[skl2onnx](https://onnx.ai/sklearn-onnx/)' کی ضرورت ہوگی تاکہ اپنے Scikit-learn ماڈل کو Onnx فارمیٹ میں تبدیل کر سکیں۔

1. پھر، اپنے ڈیٹا کے ساتھ کام کریں جیسا کہ آپ نے پچھلے اسباق میں کیا تھا، `read_csv()` کا استعمال کرتے ہوئے ایک CSV فائل پڑھیں:

    ```python
    data = pd.read_csv('../data/cleaned_cuisines.csv')
    data.head()
    ```

1. پہلے دو غیر ضروری کالم ہٹا دیں اور باقی ڈیٹا کو 'X' کے طور پر محفوظ کریں:

    ```python
    X = data.iloc[:,2:]
    X.head()
    ```

1. لیبلز کو 'y' کے طور پر محفوظ کریں:

    ```python
    y = data[['cuisine']]
    y.head()
    
    ```

### تربیتی عمل شروع کریں

ہم 'SVC' لائبریری استعمال کریں گے جو اچھی درستگی فراہم کرتی ہے۔

1. Scikit-learn سے مناسب لائبریریاں درآمد کریں:

    ```python
    from sklearn.model_selection import train_test_split
    from sklearn.svm import SVC
    from sklearn.model_selection import cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report
    ```

1. تربیتی اور ٹیسٹ سیٹ الگ کریں:

    ```python
    X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.3)
    ```

1. پچھلے سبق کی طرح ایک SVC درجہ بندی ماڈل بنائیں:

    ```python
    model = SVC(kernel='linear', C=10, probability=True,random_state=0)
    model.fit(X_train,y_train.values.ravel())
    ```

1. اب، اپنے ماڈل کو ٹیسٹ کریں، `predict()` کو کال کریں:

    ```python
    y_pred = model.predict(X_test)
    ```

1. ماڈل کے معیار کو چیک کرنے کے لیے ایک درجہ بندی رپورٹ پرنٹ کریں:

    ```python
    print(classification_report(y_test,y_pred))
    ```

    جیسا کہ ہم نے پہلے دیکھا، درستگی اچھی ہے:

    ```output
                    precision    recall  f1-score   support
    
         chinese       0.72      0.69      0.70       257
          indian       0.91      0.87      0.89       243
        japanese       0.79      0.77      0.78       239
          korean       0.83      0.79      0.81       236
            thai       0.72      0.84      0.78       224
    
        accuracy                           0.79      1199
       macro avg       0.79      0.79      0.79      1199
    weighted avg       0.79      0.79      0.79      1199
    ```

### اپنے ماڈل کو Onnx میں تبدیل کریں

یقینی بنائیں کہ تبدیلی صحیح Tensor نمبر کے ساتھ کریں۔ اس ڈیٹا سیٹ میں 380 اجزاء درج ہیں، لہذا آپ کو `FloatTensorType` میں اس نمبر کو نوٹ کرنا ہوگا:

1. 380 کے Tensor نمبر کے ساتھ تبدیل کریں۔

    ```python
    from skl2onnx import convert_sklearn
    from skl2onnx.common.data_types import FloatTensorType
    
    initial_type = [('float_input', FloatTensorType([None, 380]))]
    options = {id(model): {'nocl': True, 'zipmap': False}}
    ```

1. onx بنائیں اور اسے **model.onnx** کے طور پر فائل میں محفوظ کریں:

    ```python
    onx = convert_sklearn(model, initial_types=initial_type, options=options)
    with open("./model.onnx", "wb") as f:
        f.write(onx.SerializeToString())
    ```

    > نوٹ کریں، آپ اپنے تبدیلی اسکرپٹ میں [اختیارات](https://onnx.ai/sklearn-onnx/parameterized.html) پاس کر سکتے ہیں۔ اس معاملے میں، ہم نے 'nocl' کو True اور 'zipmap' کو False پاس کیا۔ چونکہ یہ ایک درجہ بندی ماڈل ہے، آپ کے پاس ZipMap کو ہٹانے کا اختیار ہے جو لغات کی فہرست تیار کرتا ہے (ضروری نہیں)۔ `nocl` ماڈل میں کلاس معلومات شامل ہونے کا حوالہ دیتا ہے۔ اپنے ماڈل کا سائز کم کرنے کے لیے `nocl` کو 'True' پر سیٹ کریں۔

پورا نوٹ بک چلانے سے اب ایک Onnx ماڈل بنے گا اور اسے اس فولڈر میں محفوظ کیا جائے گا۔

## اپنے ماڈل کو دیکھیں

Onnx ماڈلز Visual Studio کوڈ میں بہت زیادہ نظر نہیں آتے، لیکن ایک بہت اچھا مفت سافٹ ویئر ہے جسے بہت سے محققین ماڈل کو دیکھنے کے لیے استعمال کرتے ہیں تاکہ یہ یقینی بنایا جا سکے کہ یہ صحیح طریقے سے بنایا گیا ہے۔ [Netron](https://github.com/lutzroeder/Netron) ڈاؤن لوڈ کریں اور اپنی model.onnx فائل کھولیں۔ آپ اپنے سادہ ماڈل کو دیکھ سکتے ہیں، جس میں اس کے 380 ان پٹس اور درجہ بندی کنندہ درج ہیں:

![Netron visual](../../../../translated_images/netron.a05f39410211915e0f95e2c0e8b88f41e7d13d725faf660188f3802ba5c9e831.ur.png)

Netron ماڈلز کو دیکھنے کے لیے ایک مددگار ٹول ہے۔

اب آپ اس زبردست ماڈل کو ایک ویب ایپ میں استعمال کرنے کے لیے تیار ہیں۔ آئیے ایک ایپ بنائیں جو آپ کے فریج میں موجود اجزاء کو دیکھ کر یہ معلوم کرنے میں مدد کرے کہ آپ کے ماڈل کے مطابق کون سا کھانا پکایا جا سکتا ہے۔

## سفارش کرنے والی ویب ایپ بنائیں

آپ اپنے ماڈل کو براہ راست ایک ویب ایپ میں استعمال کر سکتے ہیں۔ یہ فن تعمیر آپ کو اسے مقامی طور پر اور یہاں تک کہ آف لائن بھی چلانے کی اجازت دیتا ہے اگر ضرورت ہو۔ اس فولڈر میں جہاں آپ نے اپنی `model.onnx` فائل محفوظ کی ہے، ایک `index.html` فائل بنائیں۔

1. اس فائل _index.html_ میں، درج ذیل مارک اپ شامل کریں:

    ```html
    <!DOCTYPE html>
    <html>
        <header>
            <title>Cuisine Matcher</title>
        </header>
        <body>
            ...
        </body>
    </html>
    ```

1. اب، `body` ٹیگز کے اندر کام کرتے ہوئے، کچھ اجزاء کو ظاہر کرنے کے لیے چیک باکسز کی ایک فہرست شامل کریں:

    ```html
    <h1>Check your refrigerator. What can you create?</h1>
            <div id="wrapper">
                <div class="boxCont">
                    <input type="checkbox" value="4" class="checkbox">
                    <label>apple</label>
                </div>
            
                <div class="boxCont">
                    <input type="checkbox" value="247" class="checkbox">
                    <label>pear</label>
                </div>
            
                <div class="boxCont">
                    <input type="checkbox" value="77" class="checkbox">
                    <label>cherry</label>
                </div>
    
                <div class="boxCont">
                    <input type="checkbox" value="126" class="checkbox">
                    <label>fenugreek</label>
                </div>
    
                <div class="boxCont">
                    <input type="checkbox" value="302" class="checkbox">
                    <label>sake</label>
                </div>
    
                <div class="boxCont">
                    <input type="checkbox" value="327" class="checkbox">
                    <label>soy sauce</label>
                </div>
    
                <div class="boxCont">
                    <input type="checkbox" value="112" class="checkbox">
                    <label>cumin</label>
                </div>
            </div>
            <div style="padding-top:10px">
                <button onClick="startInference()">What kind of cuisine can you make?</button>
            </div> 
    ```

    نوٹ کریں کہ ہر چیک باکس کو ایک ویلیو دی گئی ہے۔ یہ اس انڈیکس کی عکاسی کرتا ہے جہاں اجزاء ڈیٹا سیٹ کے مطابق پایا جاتا ہے۔ مثال کے طور پر، ایپل اس الفبائی فہرست میں پانچویں کالم پر قابض ہے، لہذا اس کی ویلیو '4' ہے کیونکہ ہم 0 سے گنتی شروع کرتے ہیں۔ آپ [اجزاء اسپریڈشیٹ](../../../../4-Classification/data/ingredient_indexes.csv) سے کسی دیے گئے جزو کے انڈیکس کو دریافت کر سکتے ہیں۔

    اپنے index.html فائل میں کام جاری رکھتے ہوئے، ایک اسکرپٹ بلاک شامل کریں جہاں ماڈل کو آخری بند ہونے والے `</div>` کے بعد کال کیا جائے۔

1. سب سے پہلے، [Onnx Runtime](https://www.onnxruntime.ai/) درآمد کریں:

    ```html
    <script src="https://cdn.jsdelivr.net/npm/onnxruntime-web@1.9.0/dist/ort.min.js"></script> 
    ```

    > Onnx Runtime آپ کے Onnx ماڈلز کو وسیع ہارڈویئر پلیٹ فارمز پر چلانے کے قابل بناتا ہے، جس میں اصلاحات اور استعمال کے لیے ایک API شامل ہے۔

1. ایک بار Runtime جگہ پر ہو، آپ اسے کال کر سکتے ہیں:

    ```html
    <script>
        const ingredients = Array(380).fill(0);
        
        const checks = [...document.querySelectorAll('.checkbox')];
        
        checks.forEach(check => {
            check.addEventListener('change', function() {
                // toggle the state of the ingredient
                // based on the checkbox's value (1 or 0)
                ingredients[check.value] = check.checked ? 1 : 0;
            });
        });

        function testCheckboxes() {
            // validate if at least one checkbox is checked
            return checks.some(check => check.checked);
        }

        async function startInference() {

            let atLeastOneChecked = testCheckboxes()

            if (!atLeastOneChecked) {
                alert('Please select at least one ingredient.');
                return;
            }
            try {
                // create a new session and load the model.
                
                const session = await ort.InferenceSession.create('./model.onnx');

                const input = new ort.Tensor(new Float32Array(ingredients), [1, 380]);
                const feeds = { float_input: input };

                // feed inputs and run
                const results = await session.run(feeds);

                // read from results
                alert('You can enjoy ' + results.label.data[0] + ' cuisine today!')

            } catch (e) {
                console.log(`failed to inference ONNX model`);
                console.error(e);
            }
        }
               
    </script>
    ```

اس کوڈ میں، کئی چیزیں ہو رہی ہیں:

1. آپ نے 380 ممکنہ ویلیوز (1 یا 0) کی ایک صف بنائی ہے جو انفرینس کے لیے ماڈل کو بھیجی جائے گی، اس پر منحصر ہے کہ آیا کوئی جزو چیک باکس چیک کیا گیا ہے۔
2. آپ نے چیک باکسز کی ایک صف بنائی اور ایک طریقہ بنایا تاکہ یہ معلوم کیا جا سکے کہ آیا وہ چیک کیے گئے ہیں، ایک `init` فنکشن میں جو ایپلیکیشن شروع ہونے پر کال کیا جاتا ہے۔ جب کوئی چیک باکس چیک کیا جاتا ہے، تو `ingredients` صف کو منتخب کردہ جزو کی عکاسی کرنے کے لیے تبدیل کیا جاتا ہے۔
3. آپ نے ایک `testCheckboxes` فنکشن بنایا جو چیک کرتا ہے کہ آیا کوئی چیک باکس چیک کیا گیا ہے۔
4. آپ نے `startInference` فنکشن استعمال کیا جب بٹن دبایا جاتا ہے اور، اگر کوئی چیک باکس چیک کیا جاتا ہے، تو آپ انفرینس شروع کرتے ہیں۔
5. انفرینس روٹین میں شامل ہیں:
   1. ماڈل کو غیر متزامن طور پر لوڈ کرنے کا سیٹ اپ
   2. ماڈل کو بھیجنے کے لیے ایک Tensor ساخت بنانا
   3. 'feeds' بنانا جو اس `float_input` ان پٹ کی عکاسی کرتا ہے جو آپ نے اپنے ماڈل کو تربیت دیتے وقت بنایا تھا (آپ Netron کا استعمال کرکے اس نام کی تصدیق کر سکتے ہیں)
   4. ان 'feeds' کو ماڈل میں بھیجنا اور جواب کا انتظار کرنا

## اپنی ایپلیکیشن کو ٹیسٹ کریں

Visual Studio Code میں اس فولڈر میں ایک ٹرمینل سیشن کھولیں جہاں آپ کی index.html فائل موجود ہے۔ یقینی بنائیں کہ آپ نے [http-server](https://www.npmjs.com/package/http-server) کو عالمی طور پر انسٹال کیا ہے، اور پرامپٹ پر `http-server` ٹائپ کریں۔ ایک localhost کھلنا چاہیے اور آپ اپنی ویب ایپ دیکھ سکتے ہیں۔ مختلف اجزاء کی بنیاد پر کون سا کھانا تجویز کیا جاتا ہے چیک کریں:

![ingredient web app](../../../../translated_images/web-app.4c76450cabe20036f8ec6d5e05ccc0c1c064f0d8f2fe3304d3bcc0198f7dc139.ur.png)

مبارک ہو، آپ نے چند فیلڈز کے ساتھ ایک 'سفارش' ویب ایپ بنائی ہے۔ اس نظام کو بنانے کے لیے کچھ وقت نکالیں!

## 🚀چیلنج

آپ کی ویب ایپ بہت کم سے کم ہے، لہذا اسے [ingredient_indexes](../../../../4-Classification/data/ingredient_indexes.csv) ڈیٹا سے اجزاء اور ان کے انڈیکس کا استعمال کرتے ہوئے بناتے رہیں۔ کون سے ذائقے کے امتزاج ایک دیے گئے قومی کھانے کو بنانے کے لیے کام کرتے ہیں؟

## [سبق کے بعد کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/26/)

## جائزہ اور خود مطالعہ

جبکہ اس سبق نے کھانے کے اجزاء کے لیے سفارش کرنے والے نظام بنانے کی افادیت کو صرف چھوا، مشین لرننگ ایپلیکیشنز کے اس علاقے میں مثالوں کی بہتات ہے۔ مزید پڑھیں کہ یہ نظام کیسے بنائے جاتے ہیں:

- https://www.sciencedirect.com/topics/computer-science/recommendation-engine
- https://www.technologyreview.com/2014/08/25/171547/the-ultimate-challenge-for-recommendation-engines/
- https://www.technologyreview.com/2015/03/23/168831/everything-is-a-recommendation/

## اسائنمنٹ 

[ایک نیا سفارش کنندہ بنائیں](assignment.md)

---

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے پوری کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستگی ہو سکتی ہیں۔ اصل دستاویز کو اس کی اصل زبان میں مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے لیے ہم ذمہ دار نہیں ہیں۔