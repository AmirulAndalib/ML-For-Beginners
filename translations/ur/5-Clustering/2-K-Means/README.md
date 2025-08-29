<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "085d571097d201810720df4cd379f8c2",
  "translation_date": "2025-08-29T13:26:30+00:00",
  "source_file": "5-Clustering/2-K-Means/README.md",
  "language_code": "ur"
}
-->
# کے-مینز کلسٹرنگ

## [پری لیکچر کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/29/)

اس سبق میں، آپ سیکھیں گے کہ کس طرح Scikit-learn اور نائجیریا کے موسیقی کے ڈیٹا سیٹ کا استعمال کرتے ہوئے کلسٹرز بنائے جائیں۔ ہم کے-مینز کلسٹرنگ کی بنیادی باتوں کا احاطہ کریں گے۔ یاد رکھیں کہ، جیسا کہ آپ نے پچھلے سبق میں سیکھا، کلسٹرز کے ساتھ کام کرنے کے کئی طریقے ہیں اور آپ کے ڈیٹا پر منحصر ہے کہ آپ کون سا طریقہ استعمال کرتے ہیں۔ ہم کے-مینز آزمائیں گے کیونکہ یہ سب سے عام کلسٹرنگ تکنیک ہے۔ آئیے شروع کرتے ہیں!

اصطلاحات جن کے بارے میں آپ سیکھیں گے:

- سلویٹ اسکورنگ
- ایلبو طریقہ
- انرشیا
- ویریئنس

## تعارف

[کے-مینز کلسٹرنگ](https://wikipedia.org/wiki/K-means_clustering) سگنل پروسیسنگ کے شعبے سے ماخوذ ایک طریقہ ہے۔ یہ ڈیٹا کے گروپس کو 'k' کلسٹرز میں تقسیم کرنے اور ان کا حصہ بنانے کے لیے استعمال ہوتا ہے، مشاہدات کی ایک سیریز کے ذریعے۔ ہر مشاہدہ ایک دیے گئے ڈیٹا پوائنٹ کو اس کے قریب ترین 'mean' یا کلسٹر کے مرکز کے قریب گروپ کرنے کے لیے کام کرتا ہے۔

کلسٹرز کو [ورونائی ڈایاگرامز](https://wikipedia.org/wiki/Voronoi_diagram) کے طور پر تصور کیا جا سکتا ہے، جن میں ایک نقطہ (یا 'seed') اور اس سے متعلقہ علاقہ شامل ہوتا ہے۔

![ورونائی ڈایاگرام](../../../../translated_images/voronoi.1dc1613fb0439b9564615eca8df47a4bcd1ce06217e7e72325d2406ef2180795.ur.png)

> انفوگرافک از [Jen Looper](https://twitter.com/jenlooper)

کے-مینز کلسٹرنگ کا عمل [تین مراحل میں انجام دیا جاتا ہے](https://scikit-learn.org/stable/modules/clustering.html#k-means):

1. الگورتھم ڈیٹا سیٹ سے k-نمبر کے مرکز پوائنٹس کا انتخاب کرتا ہے۔ اس کے بعد یہ لوپ کرتا ہے:
    1. ہر نمونے کو قریب ترین سینٹروئڈ کے ساتھ تفویض کرتا ہے۔
    2. پچھلے سینٹروئڈز کے ساتھ تفویض کردہ تمام نمونوں کی اوسط قدر لے کر نئے سینٹروئڈز بناتا ہے۔
    3. پھر، نئے اور پرانے سینٹروئڈز کے درمیان فرق کا حساب لگاتا ہے اور اس وقت تک دہراتا ہے جب تک کہ سینٹروئڈز مستحکم نہ ہو جائیں۔

کے-مینز کے استعمال میں ایک کمی یہ ہے کہ آپ کو 'k' یعنی سینٹروئڈز کی تعداد قائم کرنے کی ضرورت ہوگی۔ خوش قسمتی سے، 'ایلبو طریقہ' 'k' کے لیے ایک اچھا ابتدائی قدر کا اندازہ لگانے میں مدد کرتا ہے۔ آپ اسے ابھی آزمائیں گے۔

## پیشگی شرط

آپ اس سبق کے [_notebook.ipynb_](https://github.com/microsoft/ML-For-Beginners/blob/main/5-Clustering/2-K-Means/notebook.ipynb) فائل میں کام کریں گے، جس میں ڈیٹا درآمد اور ابتدائی صفائی شامل ہے جو آپ نے پچھلے سبق میں کی تھی۔

## مشق - تیاری

گانے کے ڈیٹا پر دوبارہ نظر ڈالیں۔

1. ہر کالم کے لیے `boxplot()` کو کال کرتے ہوئے ایک باکس پلاٹ بنائیں:

    ```python
    plt.figure(figsize=(20,20), dpi=200)
    
    plt.subplot(4,3,1)
    sns.boxplot(x = 'popularity', data = df)
    
    plt.subplot(4,3,2)
    sns.boxplot(x = 'acousticness', data = df)
    
    plt.subplot(4,3,3)
    sns.boxplot(x = 'energy', data = df)
    
    plt.subplot(4,3,4)
    sns.boxplot(x = 'instrumentalness', data = df)
    
    plt.subplot(4,3,5)
    sns.boxplot(x = 'liveness', data = df)
    
    plt.subplot(4,3,6)
    sns.boxplot(x = 'loudness', data = df)
    
    plt.subplot(4,3,7)
    sns.boxplot(x = 'speechiness', data = df)
    
    plt.subplot(4,3,8)
    sns.boxplot(x = 'tempo', data = df)
    
    plt.subplot(4,3,9)
    sns.boxplot(x = 'time_signature', data = df)
    
    plt.subplot(4,3,10)
    sns.boxplot(x = 'danceability', data = df)
    
    plt.subplot(4,3,11)
    sns.boxplot(x = 'length', data = df)
    
    plt.subplot(4,3,12)
    sns.boxplot(x = 'release_date', data = df)
    ```

    یہ ڈیٹا تھوڑا شور والا ہے: ہر کالم کو باکس پلاٹ کے طور پر دیکھ کر، آپ آؤٹ لائرز دیکھ سکتے ہیں۔

    ![آؤٹ لائرز](../../../../translated_images/boxplots.8228c29dabd0f29227dd38624231a175f411f1d8d4d7c012cb770e00e4fdf8b6.ur.png)

آپ ڈیٹا سیٹ کے ذریعے جا سکتے ہیں اور ان آؤٹ لائرز کو ہٹا سکتے ہیں، لیکن اس سے ڈیٹا کافی کم ہو جائے گا۔

1. فی الحال، ان کالموں کا انتخاب کریں جنہیں آپ اپنے کلسٹرنگ مشق کے لیے استعمال کریں گے۔ وہ کالم منتخب کریں جن کی حدود ایک جیسی ہوں اور `artist_top_genre` کالم کو عددی ڈیٹا کے طور پر انکوڈ کریں:

    ```python
    from sklearn.preprocessing import LabelEncoder
    le = LabelEncoder()
    
    X = df.loc[:, ('artist_top_genre','popularity','danceability','acousticness','loudness','energy')]
    
    y = df['artist_top_genre']
    
    X['artist_top_genre'] = le.fit_transform(X['artist_top_genre'])
    
    y = le.transform(y)
    ```

1. اب آپ کو یہ فیصلہ کرنا ہوگا کہ کتنے کلسٹرز کو ہدف بنانا ہے۔ آپ جانتے ہیں کہ ڈیٹا سیٹ سے ہم نے 3 گانے کی انواع نکالی ہیں، تو آئیے 3 آزمائیں:

    ```python
    from sklearn.cluster import KMeans
    
    nclusters = 3 
    seed = 0
    
    km = KMeans(n_clusters=nclusters, random_state=seed)
    km.fit(X)
    
    # Predict the cluster for each data point
    
    y_cluster_kmeans = km.predict(X)
    y_cluster_kmeans
    ```

آپ ایک صف دیکھتے ہیں جو ڈیٹا فریم کی ہر قطار کے لیے پیش گوئی شدہ کلسٹرز (0، 1، یا 2) کے ساتھ پرنٹ ہوتی ہے۔

1. اس صف کا استعمال کرتے ہوئے ایک 'سلویٹ اسکور' کا حساب لگائیں:

    ```python
    from sklearn import metrics
    score = metrics.silhouette_score(X, y_cluster_kmeans)
    score
    ```

## سلویٹ اسکور

ایک سلویٹ اسکور تلاش کریں جو 1 کے قریب ہو۔ یہ اسکور -1 سے 1 تک مختلف ہوتا ہے، اور اگر اسکور 1 ہو، تو کلسٹر گھنا اور دوسرے کلسٹرز سے اچھی طرح الگ ہوتا ہے۔ 0 کے قریب قدر ان کلسٹرز کی نمائندگی کرتی ہے جو ایک دوسرے کے ساتھ اوورلیپ کرتے ہیں، اور نمونے پڑوسی کلسٹرز کے فیصلہ کن حد کے بہت قریب ہوتے ہیں۔ [(ماخذ)](https://dzone.com/articles/kmeans-silhouette-score-explained-with-python-exam)

ہمارا اسکور **.53** ہے، جو بالکل درمیان میں ہے۔ یہ ظاہر کرتا ہے کہ ہمارا ڈیٹا اس قسم کی کلسٹرنگ کے لیے خاص طور پر موزوں نہیں ہے، لیکن آئیے جاری رکھتے ہیں۔

### مشق - ماڈل بنائیں

1. `KMeans` درآمد کریں اور کلسٹرنگ کا عمل شروع کریں۔

    ```python
    from sklearn.cluster import KMeans
    wcss = []
    
    for i in range(1, 11):
        kmeans = KMeans(n_clusters = i, init = 'k-means++', random_state = 42)
        kmeans.fit(X)
        wcss.append(kmeans.inertia_)
    
    ```

    یہاں کچھ حصے ہیں جن کی وضاحت ضروری ہے۔

    > 🎓 رینج: یہ کلسٹرنگ عمل کے تکرار ہیں

    > 🎓 random_state: "سینٹروئڈ کی ابتدائی ترتیب کے لیے بے ترتیب نمبر جنریشن کا تعین کرتا ہے۔" [ماخذ](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html#sklearn.cluster.KMeans)

    > 🎓 WCSS: "کلسٹر کے اندر مربع کے مجموعے" کلسٹر سینٹروئڈ کے ساتھ کلسٹر کے اندر تمام پوائنٹس کے مربع اوسط فاصلے کی پیمائش کرتا ہے۔ [ماخذ](https://medium.com/@ODSC/unsupervised-learning-evaluating-clusters-bd47eed175ce)

    > 🎓 انرشیا: کے-مینز الگورتھم سینٹروئڈز کو منتخب کرنے کی کوشش کرتا ہے تاکہ 'انرشیا' کو کم کیا جا سکے، "یہ ایک پیمانہ ہے کہ کلسٹرز اندرونی طور پر کتنے مربوط ہیں۔" [ماخذ](https://scikit-learn.org/stable/modules/clustering.html)۔ قدر ہر تکرار پر wcss متغیر میں شامل کی جاتی ہے۔

    > 🎓 k-means++: [Scikit-learn](https://scikit-learn.org/stable/modules/clustering.html#k-means) میں آپ 'k-means++' آپٹیمائزیشن استعمال کر سکتے ہیں، جو "سینٹروئڈز کو ایک دوسرے سے (عام طور پر) دور رکھنے کے لیے ترتیب دیتا ہے، جس سے بے ترتیب ابتدائی ترتیب کے مقابلے میں ممکنہ طور پر بہتر نتائج حاصل ہوتے ہیں۔

### ایلبو طریقہ

پہلے، آپ نے اندازہ لگایا کہ، چونکہ آپ نے 3 گانے کی انواع کو ہدف بنایا ہے، آپ کو 3 کلسٹرز کا انتخاب کرنا چاہیے۔ لیکن کیا واقعی ایسا ہے؟

1. 'ایلبو طریقہ' استعمال کریں تاکہ یقین دہانی ہو۔

    ```python
    plt.figure(figsize=(10,5))
    sns.lineplot(x=range(1, 11), y=wcss, marker='o', color='red')
    plt.title('Elbow')
    plt.xlabel('Number of clusters')
    plt.ylabel('WCSS')
    plt.show()
    ```

    پچھلے مرحلے میں بنائے گئے `wcss` متغیر کا استعمال کریں تاکہ ایک چارٹ بنایا جا سکے جو 'ایلبو' کے موڑ کو ظاہر کرے، جو کلسٹرز کی بہترین تعداد کی نشاندہی کرتا ہے۔ شاید یہ واقعی **3** ہے!

    ![ایلبو طریقہ](../../../../translated_images/elbow.72676169eed744ff03677e71334a16c6b8f751e9e716e3d7f40dd7cdef674cca.ur.png)

## مشق - کلسٹرز دکھائیں

1. عمل کو دوبارہ آزمائیں، اس بار تین کلسٹرز ترتیب دیں، اور کلسٹرز کو ایک اسکیٹر پلاٹ کے طور پر دکھائیں:

    ```python
    from sklearn.cluster import KMeans
    kmeans = KMeans(n_clusters = 3)
    kmeans.fit(X)
    labels = kmeans.predict(X)
    plt.scatter(df['popularity'],df['danceability'],c = labels)
    plt.xlabel('popularity')
    plt.ylabel('danceability')
    plt.show()
    ```

1. ماڈل کی درستگی چیک کریں:

    ```python
    labels = kmeans.labels_
    
    correct_labels = sum(y == labels)
    
    print("Result: %d out of %d samples were correctly labeled." % (correct_labels, y.size))
    
    print('Accuracy score: {0:0.2f}'. format(correct_labels/float(y.size)))
    ```

    اس ماڈل کی درستگی بہت اچھی نہیں ہے، اور کلسٹرز کی شکل آپ کو ایک اشارہ دیتی ہے کہ کیوں۔

    ![کلسٹرز](../../../../translated_images/clusters.b635354640d8e4fd4a49ef545495518e7be76172c97c13bd748f5b79f171f69a.ur.png)

    یہ ڈیٹا بہت غیر متوازن ہے، بہت کم تعلق رکھتا ہے اور کالم کی قدروں کے درمیان بہت زیادہ ویریئنس ہے تاکہ اچھی طرح کلسٹر ہو سکے۔ درحقیقت، جو کلسٹرز بنتے ہیں وہ شاید ان تین انواع کے زمرے سے بہت زیادہ متاثر یا جھکے ہوئے ہیں جو ہم نے اوپر بیان کیے ہیں۔ یہ ایک سیکھنے کا عمل تھا!

    Scikit-learn کی دستاویزات میں، آپ دیکھ سکتے ہیں کہ اس ماڈل کی طرح، جس کے کلسٹرز بہت اچھی طرح سے نشان زد نہیں ہیں، اس میں 'ویریئنس' کا مسئلہ ہے:

    ![مسئلہ ماڈلز](../../../../translated_images/problems.f7fb539ccd80608e1f35c319cf5e3ad1809faa3c08537aead8018c6b5ba2e33a.ur.png)
    > انفوگرافک از Scikit-learn

## ویریئنس

ویریئنس کو "Mean سے فرق کے مربع کی اوسط" کے طور پر بیان کیا گیا ہے [(ماخذ)](https://www.mathsisfun.com/data/standard-deviation.html)۔ اس کلسٹرنگ مسئلے کے تناظر میں، اس کا مطلب ہے کہ ہمارے ڈیٹا سیٹ کے نمبروں کا Mean سے تھوڑا زیادہ انحراف ہوتا ہے۔

✅ یہ ایک بہترین لمحہ ہے کہ آپ ان تمام طریقوں کے بارے میں سوچیں جن سے آپ اس مسئلے کو درست کر سکتے ہیں۔ ڈیٹا کو تھوڑا مزید ایڈجسٹ کریں؟ مختلف کالم استعمال کریں؟ مختلف الگورتھم استعمال کریں؟ اشارہ: اپنے ڈیٹا کو [اسکیل کریں](https://www.mygreatlearning.com/blog/learning-data-science-with-k-means-clustering/) تاکہ اسے نارملائز کریں اور دوسرے کالمز کو آزمائیں۔

> اس '[ویریئنس کیلکولیٹر](https://www.calculatorsoup.com/calculators/statistics/variance-calculator.php)' کو آزمائیں تاکہ اس تصور کو مزید سمجھ سکیں۔

---

## 🚀چیلنج

اس نوٹ بک کے ساتھ کچھ وقت گزاریں، پیرامیٹرز کو ایڈجسٹ کریں۔ کیا آپ آؤٹ لائرز کو ہٹانے جیسے ڈیٹا کو مزید صاف کرکے ماڈل کی درستگی کو بہتر بنا سکتے ہیں؟ آپ وزن استعمال کر سکتے ہیں تاکہ دیے گئے ڈیٹا نمونوں کو زیادہ وزن دیا جا سکے۔ آپ اور کیا کر سکتے ہیں تاکہ بہتر کلسٹرز بنائے جا سکیں؟

اشارہ: اپنے ڈیٹا کو اسکیل کرنے کی کوشش کریں۔ نوٹ بک میں تبصرہ شدہ کوڈ موجود ہے جو معیاری اسکیلنگ کو شامل کرتا ہے تاکہ ڈیٹا کالمز کو حدود کے لحاظ سے ایک دوسرے کے قریب تر بنایا جا سکے۔ آپ دیکھیں گے کہ اگرچہ سلویٹ اسکور کم ہو جاتا ہے، ایلبو گراف میں 'موڑ' ہموار ہو جاتا ہے۔ اس کی وجہ یہ ہے کہ ڈیٹا کو غیر اسکیل چھوڑنے سے کم ویریئنس والے ڈیٹا کو زیادہ وزن ملتا ہے۔ اس مسئلے پر مزید پڑھیں [یہاں](https://stats.stackexchange.com/questions/21222/are-mean-normalization-and-feature-scaling-needed-for-k-means-clustering/21226#21226)۔

## [پوسٹ لیکچر کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/30/)

## جائزہ اور خود مطالعہ

کے-مینز سمیلیٹر [جیسے اس ایک](https://user.ceng.metu.edu.tr/~akifakkus/courses/ceng574/k-means/) پر ایک نظر ڈالیں۔ آپ اس ٹول کا استعمال نمونہ ڈیٹا پوائنٹس کو تصور کرنے اور اس کے سینٹروئڈز کا تعین کرنے کے لیے کر سکتے ہیں۔ آپ ڈیٹا کی بے ترتیبی، کلسٹرز کی تعداد اور سینٹروئڈز کی تعداد کو ایڈٹ کر سکتے ہیں۔ کیا اس سے آپ کو یہ سمجھنے میں مدد ملتی ہے کہ ڈیٹا کو کیسے گروپ کیا جا سکتا ہے؟

اس کے علاوہ، [کے-مینز پر یہ ہینڈ آؤٹ](https://stanford.edu/~cpiech/cs221/handouts/kmeans.html) اسٹینفورڈ سے دیکھیں۔

## اسائنمنٹ

[مختلف کلسٹرنگ طریقے آزمائیں](assignment.md)

---

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستگی ہو سکتی ہیں۔ اصل دستاویز، جو اس کی مقامی زبان میں ہے، کو مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے لیے ہم ذمہ دار نہیں ہیں۔