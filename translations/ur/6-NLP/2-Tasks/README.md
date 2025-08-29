<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "6534e145d52a3890590d27be75386e5d",
  "translation_date": "2025-08-29T14:20:10+00:00",
  "source_file": "6-NLP/2-Tasks/README.md",
  "language_code": "ur"
}
-->
# عام قدرتی زبان پروسیسنگ کے کام اور تکنیکیں

زیادہ تر *قدرتی زبان پروسیسنگ* کے کاموں کے لیے، متن کو پروسیس کرنے کے لیے اسے توڑنا، جانچنا، اور نتائج کو قواعد اور ڈیٹا سیٹس کے ساتھ محفوظ یا حوالہ دینا ضروری ہوتا ہے۔ یہ کام پروگرامر کو متن میں الفاظ اور اصطلاحات کی _معنی_، _ارادہ_ یا صرف _تعدد_ کو سمجھنے میں مدد دیتے ہیں۔

## [لیکچر سے پہلے کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/33/)

آئیے متن کو پروسیس کرنے کے لیے استعمال ہونے والی عام تکنیکوں کو دریافت کریں۔ مشین لرننگ کے ساتھ مل کر، یہ تکنیکیں آپ کو بڑی مقدار میں متن کو مؤثر طریقے سے تجزیہ کرنے میں مدد دیتی ہیں۔ تاہم، ان کاموں پر مشین لرننگ لگانے سے پہلے، آئیے ان مسائل کو سمجھیں جن کا سامنا ایک NLP ماہر کو ہوتا ہے۔

## NLP کے عام کام

متن کا تجزیہ کرنے کے مختلف طریقے ہیں جس پر آپ کام کر رہے ہیں۔ کچھ کام ہیں جو آپ انجام دے سکتے ہیں اور ان کاموں کے ذریعے آپ متن کو سمجھنے اور نتائج اخذ کرنے کے قابل ہوتے ہیں۔ آپ عام طور پر ان کاموں کو ایک ترتیب میں انجام دیتے ہیں۔

### ٹوکنائزیشن

شاید سب سے پہلا کام جو زیادہ تر NLP الگورتھم کرتے ہیں وہ متن کو ٹوکنز یا الفاظ میں تقسیم کرنا ہے۔ اگرچہ یہ آسان لگتا ہے، لیکن مختلف زبانوں کے جملے اور الفاظ کے حدود کو مدنظر رکھنا اسے مشکل بنا سکتا ہے۔ آپ کو حدود کا تعین کرنے کے لیے مختلف طریقے استعمال کرنے پڑ سکتے ہیں۔

![ٹوکنائزیشن](../../../../translated_images/tokenization.1641a160c66cd2d93d4524e8114e93158a9ce0eba3ecf117bae318e8a6ad3487.ur.png)
> **Pride and Prejudice** سے ایک جملے کو ٹوکنائز کرنا۔ انفوگرافک [Jen Looper](https://twitter.com/jenlooper) کے ذریعے۔

### ایمبیڈنگز

[ورڈ ایمبیڈنگز](https://wikipedia.org/wiki/Word_embedding) آپ کے متن کے ڈیٹا کو عددی طور پر تبدیل کرنے کا ایک طریقہ ہیں۔ ایمبیڈنگز اس طرح کی جاتی ہیں کہ ایک جیسے معنی والے یا ایک ساتھ استعمال ہونے والے الفاظ ایک ساتھ گروپ ہو جائیں۔

![ورڈ ایمبیڈنگز](../../../../translated_images/embedding.2cf8953c4b3101d188c2f61a5de5b6f53caaa5ad4ed99236d42bc3b6bd6a1fe2.ur.png)
> "I have the highest respect for your nerves, they are my old friends." - **Pride and Prejudice** کے ایک جملے کے لیے ورڈ ایمبیڈنگز۔ انفوگرافک [Jen Looper](https://twitter.com/jenlooper) کے ذریعے۔

✅ [یہ دلچسپ ٹول](https://projector.tensorflow.org/) آزمائیں تاکہ ورڈ ایمبیڈنگز کے ساتھ تجربہ کریں۔ کسی ایک لفظ پر کلک کرنے سے ایک جیسے الفاظ کے گروپ ظاہر ہوتے ہیں: 'toy' 'disney', 'lego', 'playstation', اور 'console' کے ساتھ گروپ ہوتا ہے۔

### پارسنگ اور پارٹ آف اسپیچ ٹیگنگ

ہر وہ لفظ جو ٹوکنائز کیا گیا ہے اسے پارٹ آف اسپیچ کے طور پر ٹیگ کیا جا سکتا ہے - جیسے اسم، فعل، یا صفت۔ جملہ `the quick red fox jumped over the lazy brown dog` کو POS ٹیگ کیا جا سکتا ہے جیسے fox = اسم، jumped = فعل۔

![پارسنگ](../../../../translated_images/parse.d0c5bbe1106eae8fe7d60a183cd1736c8b6cec907f38000366535f84f3036101.ur.png)

> **Pride and Prejudice** کے ایک جملے کو پارس کرنا۔ انفوگرافک [Jen Looper](https://twitter.com/jenlooper) کے ذریعے۔

پارسنگ یہ پہچاننا ہے کہ جملے میں کون سے الفاظ ایک دوسرے سے متعلق ہیں - مثال کے طور پر `the quick red fox jumped` ایک صفت-اسم-فعل ترتیب ہے جو `lazy brown dog` ترتیب سے الگ ہے۔

### الفاظ اور جملوں کی تعدد

جب کسی بڑے متن کا تجزیہ کیا جا رہا ہو تو ایک مفید طریقہ یہ ہے کہ ہر دلچسپی والے لفظ یا جملے کی لغت بنائی جائے اور یہ دیکھا جائے کہ وہ کتنی بار ظاہر ہوتے ہیں۔ جملہ `the quick red fox jumped over the lazy brown dog` میں لفظ 'the' کی تعدد 2 ہے۔

آئیے ایک مثال کے متن کو دیکھتے ہیں جہاں ہم الفاظ کی تعدد گنتے ہیں۔ Rudyard Kipling کی نظم The Winners میں درج ذیل اشعار شامل ہیں:

```output
What the moral? Who rides may read.
When the night is thick and the tracks are blind
A friend at a pinch is a friend, indeed,
But a fool to wait for the laggard behind.
Down to Gehenna or up to the Throne,
He travels the fastest who travels alone.
```

چونکہ جملوں کی تعدد کیس حساس یا غیر حساس ہو سکتی ہے، جملہ `a friend` کی تعدد 2 ہے، `the` کی تعدد 6 ہے، اور `travels` کی تعدد 2 ہے۔

### این-گرامز

متن کو مقررہ لمبائی کے الفاظ کے سلسلے میں تقسیم کیا جا سکتا ہے، ایک لفظ (یونیگرام)، دو الفاظ (بائیگرامز)، تین الفاظ (ٹرائیگرامز) یا کسی بھی تعداد کے الفاظ (این-گرامز)۔

مثال کے طور پر `the quick red fox jumped over the lazy brown dog` کے ساتھ این-گرام اسکور 2 درج ذیل این-گرامز پیدا کرتا ہے:

1. the quick  
2. quick red  
3. red fox  
4. fox jumped  
5. jumped over  
6. over the  
7. the lazy  
8. lazy brown  
9. brown dog  

یہ تصور کرنا آسان ہو سکتا ہے کہ یہ جملے پر ایک سلائیڈنگ باکس کی طرح ہے۔ یہاں یہ 3 الفاظ کے این-گرامز کے لیے ہے، ہر جملے میں این-گرام کو بولڈ کیا گیا ہے:

1.   <u>**the quick red**</u> fox jumped over the lazy brown dog  
2.   the **<u>quick red fox</u>** jumped over the lazy brown dog  
3.   the quick **<u>red fox jumped</u>** over the lazy brown dog  
4.   the quick red **<u>fox jumped over</u>** the lazy brown dog  
5.   the quick red fox **<u>jumped over the</u>** lazy brown dog  
6.   the quick red fox jumped **<u>over the lazy</u>** brown dog  
7.   the quick red fox jumped over <u>**the lazy brown**</u> dog  
8.   the quick red fox jumped over the **<u>lazy brown dog</u>**  

![این-گرامز سلائیڈنگ ونڈو](../../../../6-NLP/2-Tasks/images/n-grams.gif)

> این-گرام ویلیو 3: انفوگرافک [Jen Looper](https://twitter.com/jenlooper) کے ذریعے۔

### اسم جملے کا استخراج

زیادہ تر جملوں میں ایک اسم ہوتا ہے جو جملے کا موضوع یا مفعول ہوتا ہے۔ انگریزی میں، یہ اکثر 'a'، 'an'، یا 'the' کے ساتھ پہچانا جا سکتا ہے۔ جملے کے معنی کو سمجھنے کی کوشش کرتے وقت 'اسم جملے کا استخراج' ایک عام NLP کام ہے۔

✅ جملے "I cannot fix on the hour, or the spot, or the look or the words, which laid the foundation. It is too long ago. I was in the middle before I knew that I had begun." میں کیا آپ اسم جملے کی شناخت کر سکتے ہیں؟

جملے `the quick red fox jumped over the lazy brown dog` میں 2 اسم جملے ہیں: **quick red fox** اور **lazy brown dog**۔

### جذبات کا تجزیہ

کسی جملے یا متن کا تجزیہ کیا جا سکتا ہے کہ وہ کتنا *مثبت* یا *منفی* ہے۔ جذبات کو *پولیریٹی* اور *معروضیت/موضوعیت* میں ماپا جاتا ہے۔ پولیریٹی -1.0 سے 1.0 (منفی سے مثبت) اور 0.0 سے 1.0 (سب سے زیادہ معروضی سے سب سے زیادہ موضوعی) میں ماپی جاتی ہے۔

✅ بعد میں آپ سیکھیں گے کہ جذبات کا تعین کرنے کے مختلف طریقے ہیں، لیکن ایک طریقہ یہ ہے کہ الفاظ اور جملوں کی ایک فہرست ہو جو انسانی ماہر کے ذریعے مثبت یا منفی کے طور پر درجہ بندی کی گئی ہو اور اس ماڈل کو متن پر لاگو کر کے پولیریٹی اسکور کا حساب لگایا جائے۔ کیا آپ دیکھ سکتے ہیں کہ یہ کچھ حالات میں کیسے کام کرے گا اور دوسروں میں کم مؤثر ہوگا؟

### انفلیکشن

انفلیکشن آپ کو کسی لفظ کو لے کر اس کا واحد یا جمع حاصل کرنے کی اجازت دیتا ہے۔

### لیماٹائزیشن

ایک *لیما* الفاظ کے ایک سیٹ کے لیے جڑ یا ہیڈورڈ ہے، مثال کے طور پر *flew*, *flies*, *flying* کا لیما فعل *fly* ہے۔

NLP محقق کے لیے کچھ مفید ڈیٹا بیس بھی دستیاب ہیں، خاص طور پر:

### ورڈ نیٹ

[ورڈ نیٹ](https://wordnet.princeton.edu/) الفاظ، مترادفات، متضاد الفاظ اور مختلف زبانوں میں ہر لفظ کے لیے بہت سی دیگر تفصیلات کا ڈیٹا بیس ہے۔ یہ ترجمے، اسپیل چیکرز، یا کسی بھی قسم کے زبان کے ٹولز بنانے کی کوشش کرتے وقت انتہائی مفید ہے۔

## NLP لائبریریاں

خوش قسمتی سے، آپ کو یہ تمام تکنیکیں خود بنانے کی ضرورت نہیں ہے، کیونکہ بہترین Python لائبریریاں دستیاب ہیں جو قدرتی زبان پروسیسنگ یا مشین لرننگ میں مہارت نہ رکھنے والے ڈویلپرز کے لیے اسے بہت زیادہ قابل رسائی بناتی ہیں۔ اگلے اسباق میں ان کے مزید مثالیں شامل ہیں، لیکن یہاں آپ کو اگلے کام میں مدد کے لیے کچھ مفید مثالیں سیکھنے کو ملیں گی۔

### مشق - `TextBlob` لائبریری کا استعمال

آئیے ایک لائبریری استعمال کریں جسے TextBlob کہتے ہیں کیونکہ اس میں ان قسم کے کاموں سے نمٹنے کے لیے مفید APIs شامل ہیں۔ TextBlob "[NLTK](https://nltk.org) اور [pattern](https://github.com/clips/pattern) کے مضبوط کندھوں پر کھڑا ہے، اور دونوں کے ساتھ اچھا کام کرتا ہے۔" اس کے API میں مشین لرننگ کی کافی مقدار شامل ہے۔

> نوٹ: TextBlob کے لیے ایک مفید [Quick Start](https://textblob.readthedocs.io/en/dev/quickstart.html#quickstart) گائیڈ دستیاب ہے جو تجربہ کار Python ڈویلپرز کے لیے تجویز کیا جاتا ہے۔

جب *اسم جملے* کی شناخت کرنے کی کوشش کی جاتی ہے، TextBlob کئی اختیارات پیش کرتا ہے تاکہ اسم جملے تلاش کیے جا سکیں۔

1. `ConllExtractor` کو دیکھیں۔

    ```python
    from textblob import TextBlob
    from textblob.np_extractors import ConllExtractor
    # import and create a Conll extractor to use later 
    extractor = ConllExtractor()
    
    # later when you need a noun phrase extractor:
    user_input = input("> ")
    user_input_blob = TextBlob(user_input, np_extractor=extractor)  # note non-default extractor specified
    np = user_input_blob.noun_phrases                                    
    ```

    > یہاں کیا ہو رہا ہے؟ [ConllExtractor](https://textblob.readthedocs.io/en/dev/api_reference.html?highlight=Conll#textblob.en.np_extractors.ConllExtractor) "ایک اسم جملے کا استخراج کرنے والا ہے جو ConLL-2000 تربیتی کارپس کے ساتھ تربیت یافتہ چنک پارسنگ استعمال کرتا ہے۔" ConLL-2000 2000 میں Computational Natural Language Learning کانفرنس کا حوالہ دیتا ہے۔ ہر سال کانفرنس نے ایک مشکل NLP مسئلے کو حل کرنے کے لیے ورکشاپ کی میزبانی کی، اور 2000 میں یہ اسم چنکنگ تھا۔ ایک ماڈل Wall Street Journal پر تربیت یافتہ تھا، "سیکشنز 15-18 کو تربیتی ڈیٹا کے طور پر (211727 ٹوکنز) اور سیکشن 20 کو ٹیسٹ ڈیٹا کے طور پر (47377 ٹوکنز)"۔ آپ استعمال کیے گئے طریقہ کار کو [یہاں](https://www.clips.uantwerpen.be/conll2000/chunking/) اور [نتائج](https://ifarm.nl/erikt/research/np-chunking.html) دیکھ سکتے ہیں۔

### چیلنج - اپنے بوٹ کو NLP کے ساتھ بہتر بنانا

پچھلے سبق میں آپ نے ایک بہت ہی سادہ Q&A بوٹ بنایا۔ اب، آپ مارون کو تھوڑا زیادہ ہمدرد بنائیں گے، اپنے ان پٹ کا تجزیہ کر کے جذبات کا تعین کریں گے اور اس کے مطابق جواب دیں گے۔ آپ کو ایک `noun_phrase` کی شناخت بھی کرنی ہوگی اور اس کے بارے میں مزید ان پٹ طلب کرنا ہوگا۔

اپنے بوٹ کو بہتر بنانے کے لیے آپ کے اقدامات:

1. صارف کو بوٹ کے ساتھ بات چیت کرنے کے طریقے کے بارے میں ہدایات پرنٹ کریں  
2. لوپ شروع کریں  
   1. صارف کا انپٹ قبول کریں  
   2. اگر صارف نے باہر نکلنے کو کہا ہے، تو باہر نکلیں  
   3. صارف کے انپٹ کو پروسیس کریں اور مناسب جذباتی جواب کا تعین کریں  
   4. اگر جذبات میں ایک اسم جملہ کا پتہ چلتا ہے، تو اسے جمع کریں اور اس موضوع پر مزید انپٹ طلب کریں  
   5. جواب پرنٹ کریں  
3. مرحلہ 2 پر واپس لوپ کریں  

یہاں TextBlob کا استعمال کرتے ہوئے جذبات کا تعین کرنے کے لیے کوڈ کا ٹکڑا ہے۔ نوٹ کریں کہ جذباتی جواب کے صرف چار *گریڈینٹس* ہیں (اگر آپ چاہیں تو مزید ہو سکتے ہیں):

```python
if user_input_blob.polarity <= -0.5:
  response = "Oh dear, that sounds bad. "
elif user_input_blob.polarity <= 0:
  response = "Hmm, that's not great. "
elif user_input_blob.polarity <= 0.5:
  response = "Well, that sounds positive. "
elif user_input_blob.polarity <= 1:
  response = "Wow, that sounds great. "
```

یہاں کچھ نمونہ آؤٹ پٹ ہے جو آپ کی رہنمائی کرے گا (صارف کا انپٹ > سے شروع ہونے والی لائنوں پر ہے):

```output
Hello, I am Marvin, the friendly robot.
You can end this conversation at any time by typing 'bye'
After typing each answer, press 'enter'
How are you today?
> I am ok
Well, that sounds positive. Can you tell me more?
> I went for a walk and saw a lovely cat
Well, that sounds positive. Can you tell me more about lovely cats?
> cats are the best. But I also have a cool dog
Wow, that sounds great. Can you tell me more about cool dogs?
> I have an old hounddog but he is sick
Hmm, that's not great. Can you tell me more about old hounddogs?
> bye
It was nice talking to you, goodbye!
```

اس کام کا ایک ممکنہ حل [یہاں](https://github.com/microsoft/ML-For-Beginners/blob/main/6-NLP/2-Tasks/solution/bot.py) ہے۔

✅ علم کی جانچ

1. کیا آپ کو لگتا ہے کہ ہمدرد جوابات کسی کو یہ سوچنے پر مجبور کر سکتے ہیں کہ بوٹ واقعی انہیں سمجھتا ہے؟  
2. کیا اسم جملے کی شناخت بوٹ کو زیادہ 'قابل یقین' بناتی ہے؟  
3. جملے سے 'اسم جملے' نکالنا کیوں مفید ہو سکتا ہے؟  

---

پچھلے علم کی جانچ میں بوٹ کو نافذ کریں اور اسے کسی دوست پر آزمائیں۔ کیا یہ انہیں دھوکہ دے سکتا ہے؟ کیا آپ اپنے بوٹ کو زیادہ 'قابل یقین' بنا سکتے ہیں؟

## 🚀چیلنج

پچھلے علم کی جانچ میں ایک کام لیں اور اسے نافذ کرنے کی کوشش کریں۔ بوٹ کو کسی دوست پر آزمائیں۔ کیا یہ انہیں دھوکہ دے سکتا ہے؟ کیا آپ اپنے بوٹ کو زیادہ 'قابل یقین' بنا سکتے ہیں؟

## [لیکچر کے بعد کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/34/)

## جائزہ اور خود مطالعہ

اگلے چند اسباق میں آپ جذباتی تجزیہ کے بارے میں مزید سیکھیں گے۔ اس دلچسپ تکنیک پر تحقیق کریں جیسے [KDNuggets](https://www.kdnuggets.com/tag/nlp) پر مضامین۔

## اسائنمنٹ 

[بوٹ کو بات کرنے پر مجبور کریں](assignment.md)

---

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا غیر درستیاں ہو سکتی ہیں۔ اصل دستاویز کو اس کی اصل زبان میں مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ ہم اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے ذمہ دار نہیں ہیں۔