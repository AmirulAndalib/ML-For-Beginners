<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "76438ce4e5d48982d48f1b55c981caac",
  "translation_date": "2025-08-29T14:00:21+00:00",
  "source_file": "4-Classification/1-Introduction/README.md",
  "language_code": "ur"
}
-->
# تعارف: درجہ بندی

ان چار اسباق میں، آپ مشین لرننگ کے ایک بنیادی پہلو - _درجہ بندی_ - کو دریافت کریں گے۔ ہم ایشیا اور بھارت کے شاندار کھانوں کے بارے میں ایک ڈیٹا سیٹ کے ساتھ مختلف درجہ بندی کے الگورتھمز کا استعمال کریں گے۔ امید ہے کہ آپ بھوکے ہیں!

![صرف ایک چٹکی!](../../../../translated_images/pinch.1b035ec9ba7e0d408313b551b60c721c9c290b2dd2094115bc87e6ddacd114c9.ur.png)

> ان اسباق میں پان-ایشیائی کھانوں کا جشن منائیں! تصویر: [Jen Looper](https://twitter.com/jenlooper)

درجہ بندی [نگرانی شدہ سیکھنے](https://wikipedia.org/wiki/Supervised_learning) کی ایک قسم ہے جو رجعتی تکنیکوں سے بہت مشابہت رکھتی ہے۔ اگر مشین لرننگ ڈیٹا سیٹس کا استعمال کرتے ہوئے چیزوں کے نام یا اقدار کی پیش گوئی کرنے کے بارے میں ہے، تو درجہ بندی عام طور پر دو گروپوں میں تقسیم ہوتی ہے: _بائنری درجہ بندی_ اور _ملٹی کلاس درجہ بندی_۔

[![درجہ بندی کا تعارف](https://img.youtube.com/vi/eg8DJYwdMyg/0.jpg)](https://youtu.be/eg8DJYwdMyg "درجہ بندی کا تعارف")

> 🎥 اوپر دی گئی تصویر پر کلک کریں: MIT کے جان گٹگ درجہ بندی کا تعارف دیتے ہیں

یاد رکھیں:

- **لکیری رجعت** نے آپ کو متغیرات کے درمیان تعلقات کی پیش گوئی کرنے اور یہ اندازہ لگانے میں مدد دی کہ نیا ڈیٹا پوائنٹ اس لائن کے تعلق میں کہاں گرے گا۔ مثال کے طور پر، آپ پیش گوئی کر سکتے ہیں کہ _ستمبر کے مقابلے میں دسمبر میں کدو کی قیمت کیا ہوگی_۔
- **لاجسٹک رجعت** نے آپ کو "بائنری زمرے" دریافت کرنے میں مدد دی: اس قیمت پر، _کیا یہ کدو نارنجی ہے یا غیر نارنجی_؟

درجہ بندی مختلف الگورتھمز کا استعمال کرتی ہے تاکہ ڈیٹا پوائنٹ کے لیبل یا کلاس کا تعین کیا جا سکے۔ آئیے اس کھانے کے ڈیٹا کے ساتھ کام کریں تاکہ یہ دیکھ سکیں کہ آیا اجزاء کے ایک گروپ کو دیکھ کر ہم اس کے کھانے کی اصل کا پتہ لگا سکتے ہیں۔

## [پری لیکچر کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/19/)

> ### [یہ سبق R میں دستیاب ہے!](../../../../4-Classification/1-Introduction/solution/R/lesson_10.html)

### تعارف

درجہ بندی مشین لرننگ کے محقق اور ڈیٹا سائنسدان کی بنیادی سرگرمیوں میں سے ایک ہے۔ بائنری ویلیو کی بنیادی درجہ بندی ("کیا یہ ای میل اسپام ہے یا نہیں؟") سے لے کر کمپیوٹر وژن کا استعمال کرتے ہوئے پیچیدہ تصویر کی درجہ بندی اور تقسیم تک، ڈیٹا کو کلاسز میں ترتیب دینا اور اس سے سوالات پوچھنا ہمیشہ مفید ہوتا ہے۔

اس عمل کو زیادہ سائنسی انداز میں بیان کرنے کے لیے، آپ کا درجہ بندی کا طریقہ ایک پیش گوئی ماڈل بناتا ہے جو آپ کو ان پٹ متغیرات اور آؤٹ پٹ متغیرات کے درمیان تعلق کو نقشہ بنانے کے قابل بناتا ہے۔

![بائنری بمقابلہ ملٹی کلاس درجہ بندی](../../../../translated_images/binary-multiclass.b56d0c86c81105a697dddd82242c1d11e4d78b7afefea07a44627a0f1111c1a9.ur.png)

> بائنری بمقابلہ ملٹی کلاس مسائل جنہیں درجہ بندی کے الگورتھمز سنبھال سکتے ہیں۔ انفوگرافک: [Jen Looper](https://twitter.com/jenlooper)

ہمارے ڈیٹا کو صاف کرنے، بصری بنانے، اور اسے مشین لرننگ کے کاموں کے لیے تیار کرنے کے عمل کو شروع کرنے سے پہلے، آئیے یہ سیکھیں کہ مشین لرننگ کو ڈیٹا کی درجہ بندی کے لیے مختلف طریقوں سے کیسے استعمال کیا جا سکتا ہے۔

[شماریات](https://wikipedia.org/wiki/Statistical_classification) سے ماخوذ، کلاسک مشین لرننگ کا استعمال کرتے ہوئے درجہ بندی خصوصیات جیسے `smoker`, `weight`, اور `age` کا استعمال کرتی ہے تاکہ _X بیماری کے ہونے کے امکان_ کا تعین کیا جا سکے۔ ایک نگرانی شدہ سیکھنے کی تکنیک کے طور پر، جو آپ نے پہلے رجعتی مشقوں میں انجام دی، آپ کا ڈیٹا لیبل شدہ ہوتا ہے اور مشین لرننگ الگورتھمز ان لیبلز کا استعمال کرتے ہوئے ڈیٹا سیٹ کی کلاسز (یا 'خصوصیات') کی پیش گوئی کرتے ہیں اور انہیں کسی گروپ یا نتیجے میں تفویض کرتے ہیں۔

✅ ایک لمحہ نکال کر کھانوں کے بارے میں ایک ڈیٹا سیٹ کا تصور کریں۔ ملٹی کلاس ماڈل کیا جواب دے سکتا ہے؟ بائنری ماڈل کیا جواب دے سکتا ہے؟ اگر آپ یہ جاننا چاہتے ہیں کہ آیا دی گئی کھانے میں میتھی کا استعمال ممکن ہے؟ یا اگر آپ یہ دیکھنا چاہتے ہیں کہ، ایک گروسری بیگ میں ستارہ انیس، آرٹچوک، گوبھی، اور ہارسریڈش کے ساتھ، آپ ایک عام بھارتی ڈش بنا سکتے ہیں؟

[![پاگل پراسرار باسکٹ](https://img.youtube.com/vi/GuTeDbaNoEU/0.jpg)](https://youtu.be/GuTeDbaNoEU "پاگل پراسرار باسکٹ")

> 🎥 اوپر دی گئی تصویر پر کلک کریں۔ شو 'چوپڈ' کا پورا تصور 'پراسرار باسکٹ' ہے جہاں شیف کو اجزاء کے ایک بے ترتیب انتخاب سے کچھ ڈش بنانی ہوتی ہے۔ یقیناً ایک مشین لرننگ ماڈل مددگار ثابت ہوتا!

## ہیلو 'کلاسفائر'

ہم اس کھانے کے ڈیٹا سیٹ سے جو سوال پوچھنا چاہتے ہیں وہ دراصل ایک **ملٹی کلاس سوال** ہے، کیونکہ ہمارے پاس کام کرنے کے لیے کئی ممکنہ قومی کھانے ہیں۔ اجزاء کے ایک بیچ کو دیکھتے ہوئے، ان میں سے کون سی کلاسز ڈیٹا کے مطابق ہوں گی؟

Scikit-learn مختلف الگورتھمز پیش کرتا ہے جنہیں ڈیٹا کی درجہ بندی کے لیے استعمال کیا جا سکتا ہے، اس پر منحصر ہے کہ آپ کس قسم کا مسئلہ حل کرنا چاہتے ہیں۔ اگلے دو اسباق میں، آپ ان الگورتھمز کے بارے میں سیکھیں گے۔

## مشق - اپنے ڈیٹا کو صاف کریں اور متوازن کریں

پروجیکٹ شروع کرنے سے پہلے پہلا کام یہ ہے کہ اپنے ڈیٹا کو صاف کریں اور **متوازن** کریں تاکہ بہتر نتائج حاصل کیے جا سکیں۔ اس فولڈر کی جڑ میں موجود خالی _notebook.ipynb_ فائل سے شروع کریں۔

پہلی چیز جو انسٹال کرنی ہے وہ ہے [imblearn](https://imbalanced-learn.org/stable/)۔ یہ ایک Scikit-learn پیکیج ہے جو آپ کو ڈیٹا کو بہتر طور پر متوازن کرنے کی اجازت دے گا (آپ اس کام کے بارے میں مزید سیکھیں گے)۔

1. `imblearn` انسٹال کرنے کے لیے، `pip install` چلائیں، جیسے:

    ```python
    pip install imblearn
    ```

1. وہ پیکیجز درآمد کریں جن کی آپ کو اپنے ڈیٹا کو درآمد کرنے اور بصری بنانے کے لیے ضرورت ہے، نیز `SMOTE` کو `imblearn` سے درآمد کریں۔

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import matplotlib as mpl
    import numpy as np
    from imblearn.over_sampling import SMOTE
    ```

    اب آپ اگلے مرحلے میں ڈیٹا درآمد کرنے کے لیے تیار ہیں۔

1. اگلا کام ڈیٹا درآمد کرنا ہوگا:

    ```python
    df  = pd.read_csv('../data/cuisines.csv')
    ```

   `read_csv()` کا استعمال _cusines.csv_ فائل کے مواد کو پڑھنے اور اسے متغیر `df` میں رکھنے کے لیے کرے گا۔

1. ڈیٹا کی شکل چیک کریں:

    ```python
    df.head()
    ```

   پہلی پانچ قطاریں اس طرح نظر آتی ہیں:

    ```output
    |     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
    | --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
    | 0   | 65         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 1   | 66         | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 2   | 67         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 3   | 68         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 4   | 69         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |
    ```

1. اس ڈیٹا کے بارے میں معلومات حاصل کریں `info()` کو کال کر کے:

    ```python
    df.info()
    ```

    آپ کا آؤٹ پٹ اس طرح نظر آتا ہے:

    ```output
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2448 entries, 0 to 2447
    Columns: 385 entries, Unnamed: 0 to zucchini
    dtypes: int64(384), object(1)
    memory usage: 7.2+ MB
    ```

## مشق - کھانوں کے بارے میں سیکھنا

اب کام زیادہ دلچسپ ہونے لگتا ہے۔ آئیے ڈیٹا کی تقسیم کو دریافت کریں، فی کھانے 

1. ڈیٹا کو بارز کے طور پر `barh()` کو کال کر کے پلاٹ کریں:

    ```python
    df.cuisine.value_counts().plot.barh()
    ```

    ![کھانے کے ڈیٹا کی تقسیم](../../../../translated_images/cuisine-dist.d0cc2d551abe5c25f83d73a5f560927e4a061e9a4560bac1e97d35682ef3ca6d.ur.png)

    کھانوں کی ایک محدود تعداد ہے، لیکن ڈیٹا کی تقسیم غیر مساوی ہے۔ آپ اسے ٹھیک کر سکتے ہیں! ایسا کرنے سے پہلے، تھوڑا اور دریافت کریں۔

1. معلوم کریں کہ فی کھانے کتنے ڈیٹا دستیاب ہیں اور اسے پرنٹ کریں:

    ```python
    thai_df = df[(df.cuisine == "thai")]
    japanese_df = df[(df.cuisine == "japanese")]
    chinese_df = df[(df.cuisine == "chinese")]
    indian_df = df[(df.cuisine == "indian")]
    korean_df = df[(df.cuisine == "korean")]
    
    print(f'thai df: {thai_df.shape}')
    print(f'japanese df: {japanese_df.shape}')
    print(f'chinese df: {chinese_df.shape}')
    print(f'indian df: {indian_df.shape}')
    print(f'korean df: {korean_df.shape}')
    ```

    آؤٹ پٹ اس طرح نظر آتا ہے:

    ```output
    thai df: (289, 385)
    japanese df: (320, 385)
    chinese df: (442, 385)
    indian df: (598, 385)
    korean df: (799, 385)
    ```

## اجزاء دریافت کرنا

اب آپ ڈیٹا میں مزید گہرائی میں جا سکتے ہیں اور یہ جان سکتے ہیں کہ فی کھانے عام اجزاء کیا ہیں۔ آپ کو بار بار آنے والے ڈیٹا کو صاف کرنا چاہیے جو کھانوں کے درمیان الجھن پیدا کرتا ہے، تو آئیے اس مسئلے کے بارے میں سیکھیں۔

1. Python میں ایک فنکشن `create_ingredient()` بنائیں تاکہ ایک اجزاء کا ڈیٹا فریم بنایا جا سکے۔ یہ فنکشن ایک غیر مددگار کالم کو ہٹانے اور اجزاء کو ان کی گنتی کے ذریعے ترتیب دینے سے شروع ہوگا:

    ```python
    def create_ingredient_df(df):
        ingredient_df = df.T.drop(['cuisine','Unnamed: 0']).sum(axis=1).to_frame('value')
        ingredient_df = ingredient_df[(ingredient_df.T != 0).any()]
        ingredient_df = ingredient_df.sort_values(by='value', ascending=False,
        inplace=False)
        return ingredient_df
    ```

   اب آپ اس فنکشن کا استعمال کر کے فی کھانے دس سب سے زیادہ مقبول اجزاء کا اندازہ لگا سکتے ہیں۔

1. `create_ingredient()` کو کال کریں اور اسے `barh()` کو کال کر کے پلاٹ کریں:

    ```python
    thai_ingredient_df = create_ingredient_df(thai_df)
    thai_ingredient_df.head(10).plot.barh()
    ```

    ![تھائی](../../../../translated_images/thai.0269dbab2e78bd38a132067759fe980008bdb80b6d778e5313448dbe12bed846.ur.png)

1. جاپانی ڈیٹا کے لیے بھی ایسا ہی کریں:

    ```python
    japanese_ingredient_df = create_ingredient_df(japanese_df)
    japanese_ingredient_df.head(10).plot.barh()
    ```

    ![جاپانی](../../../../translated_images/japanese.30260486f2a05c463c8faa62ebe7b38f0961ed293bd9a6db8eef5d3f0cf17155.ur.png)

1. اب چینی اجزاء کے لیے:

    ```python
    chinese_ingredient_df = create_ingredient_df(chinese_df)
    chinese_ingredient_df.head(10).plot.barh()
    ```

    ![چینی](../../../../translated_images/chinese.e62cafa5309f111afd1b54490336daf4e927ce32bed837069a0b7ce481dfae8d.ur.png)

1. بھارتی اجزاء کو پلاٹ کریں:

    ```python
    indian_ingredient_df = create_ingredient_df(indian_df)
    indian_ingredient_df.head(10).plot.barh()
    ```

    ![بھارتی](../../../../translated_images/indian.2c4292002af1a1f97a4a24fec6b1459ee8ff616c3822ae56bb62b9903e192af6.ur.png)

1. آخر میں، کورین اجزاء کو پلاٹ کریں:

    ```python
    korean_ingredient_df = create_ingredient_df(korean_df)
    korean_ingredient_df.head(10).plot.barh()
    ```

    ![کورین](../../../../translated_images/korean.4a4f0274f3d9805a65e61f05597eeaad8620b03be23a2c0a705c023f65fad2c0.ur.png)

1. اب، سب سے عام اجزاء کو ہٹا دیں جو مختلف کھانوں کے درمیان الجھن پیدا کرتے ہیں، `drop()` کو کال کر کے: 

   سب کو چاول، لہسن اور ادرک پسند ہیں!

    ```python
    feature_df= df.drop(['cuisine','Unnamed: 0','rice','garlic','ginger'], axis=1)
    labels_df = df.cuisine #.unique()
    feature_df.head()
    ```

## ڈیٹا سیٹ کو متوازن کریں

اب جب کہ آپ نے ڈیٹا کو صاف کر لیا ہے، [SMOTE](https://imbalanced-learn.org/dev/references/generated/imblearn.over_sampling.SMOTE.html) - "Synthetic Minority Over-sampling Technique" - کا استعمال کریں تاکہ اسے متوازن کیا جا سکے۔

1. `fit_resample()` کو کال کریں، یہ حکمت عملی انٹرپولیشن کے ذریعے نئے نمونے تیار کرتی ہے۔

    ```python
    oversample = SMOTE()
    transformed_feature_df, transformed_label_df = oversample.fit_resample(feature_df, labels_df)
    ```

    اپنے ڈیٹا کو متوازن کر کے، آپ کو اسے درجہ بندی کرتے وقت بہتر نتائج حاصل ہوں گے۔ بائنری درجہ بندی کے بارے میں سوچیں۔ اگر آپ کے ڈیٹا کا زیادہ تر حصہ ایک کلاس ہے، تو مشین لرننگ ماڈل زیادہ کثرت سے اس کلاس کی پیش گوئی کرے گا، صرف اس لیے کہ اس کے لیے زیادہ ڈیٹا موجود ہے۔ ڈیٹا کو متوازن کرنا کسی بھی جھکے ہوئے ڈیٹا کو لے کر اس عدم توازن کو دور کرنے میں مدد کرتا ہے۔

1. اب آپ اجزاء فی لیبل کی تعداد چیک کر سکتے ہیں:

    ```python
    print(f'new label count: {transformed_label_df.value_counts()}')
    print(f'old label count: {df.cuisine.value_counts()}')
    ```

    آپ کا آؤٹ پٹ اس طرح نظر آتا ہے:

    ```output
    new label count: korean      799
    chinese     799
    indian      799
    japanese    799
    thai        799
    Name: cuisine, dtype: int64
    old label count: korean      799
    indian      598
    chinese     442
    japanese    320
    thai        289
    Name: cuisine, dtype: int64
    ```

    ڈیٹا صاف، متوازن، اور بہت مزیدار ہے!

1. آخری مرحلہ یہ ہے کہ اپنے متوازن ڈیٹا کو، بشمول لیبلز اور خصوصیات، ایک نئے ڈیٹا فریم میں محفوظ کریں جسے فائل میں برآمد کیا جا سکتا ہے:

    ```python
    transformed_df = pd.concat([transformed_label_df,transformed_feature_df],axis=1, join='outer')
    ```

1. آپ `transformed_df.head()` اور `transformed_df.info()` کا استعمال کر کے ڈیٹا پر ایک اور نظر ڈال سکتے ہیں۔ اس ڈیٹا کی ایک کاپی مستقبل کے اسباق میں استعمال کے لیے محفوظ کریں:

    ```python
    transformed_df.head()
    transformed_df.info()
    transformed_df.to_csv("../data/cleaned_cuisines.csv")
    ```

    یہ تازہ CSV اب جڑ کے ڈیٹا فولڈر میں پایا جا سکتا ہے۔

---

## 🚀چیلنج

اس نصاب میں کئی دلچسپ ڈیٹا سیٹس شامل ہیں۔ `data` فولڈرز کو کھنگالیں اور دیکھیں کہ آیا ان میں کوئی ایسا ڈیٹا سیٹ موجود ہے جو بائنری یا ملٹی کلاس درجہ بندی کے لیے موزوں ہو؟ آپ اس ڈیٹا سیٹ سے کیا سوالات پوچھیں گے؟

## [پوسٹ لیکچر کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/20/)

## جائزہ اور خود مطالعہ

SMOTE کے API کو دریافت کریں۔ یہ کن استعمال کے معاملات کے لیے بہترین ہے؟ یہ کون سے مسائل حل کرتا ہے؟

## اسائنمنٹ 

[درجہ بندی کے طریقے دریافت کریں](assignment.md)

---

**ڈس کلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستگی ہو سکتی ہیں۔ اصل دستاویز کو اس کی اصل زبان میں مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے لیے ہم ذمہ دار نہیں ہیں۔