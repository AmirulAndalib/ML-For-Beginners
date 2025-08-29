<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "3c4738bb0836dd838c552ab9cab7e09d",
  "translation_date": "2025-08-29T14:25:06+00:00",
  "source_file": "6-NLP/4-Hotel-Reviews-1/README.md",
  "language_code": "ur"
}
-->
# ہوٹل کے جائزوں کے ساتھ جذباتی تجزیہ - ڈیٹا کی پروسیسنگ

اس سیکشن میں آپ پچھلے اسباق میں سیکھے گئے تکنیکوں کا استعمال کرتے ہوئے ایک بڑے ڈیٹا سیٹ کا تجزیہ کریں گے۔ جب آپ مختلف کالمز کی افادیت کو اچھی طرح سمجھ لیں گے، تو آپ سیکھیں گے:

- غیر ضروری کالمز کو کیسے ہٹایا جائے
- موجودہ کالمز کی بنیاد پر نیا ڈیٹا کیسے حساب کیا جائے
- حتمی چیلنج کے لیے ڈیٹا سیٹ کو محفوظ کیسے کیا جائے

## [لیکچر سے پہلے کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/37/)

### تعارف

اب تک آپ نے سیکھا ہے کہ ٹیکسٹ ڈیٹا عددی ڈیٹا کی اقسام سے کافی مختلف ہوتا ہے۔ اگر یہ انسان کے ذریعہ لکھا یا بولا گیا ہو، تو اسے تجزیہ کرکے پیٹرنز، فریکوئنسیز، جذبات اور معنی تلاش کیے جا سکتے ہیں۔ یہ سبق آپ کو ایک حقیقی ڈیٹا سیٹ اور ایک حقیقی چیلنج میں لے جاتا ہے: **[515K ہوٹل کے جائزوں کا ڈیٹا یورپ میں](https://www.kaggle.com/jiashenliu/515k-hotel-reviews-data-in-europe)**، جس میں ایک [CC0: پبلک ڈومین لائسنس](https://creativecommons.org/publicdomain/zero/1.0/) شامل ہے۔ یہ Booking.com سے عوامی ذرائع سے حاصل کیا گیا تھا۔ ڈیٹا سیٹ کے تخلیق کار Jiashen Liu ہیں۔

### تیاری

آپ کو ضرورت ہوگی:

* Python 3 کے ساتھ .ipynb نوٹ بکس چلانے کی صلاحیت
* pandas
* NLTK، [جسے آپ کو مقامی طور پر انسٹال کرنا چاہیے](https://www.nltk.org/install.html)
* ڈیٹا سیٹ جو Kaggle پر دستیاب ہے [515K ہوٹل کے جائزوں کا ڈیٹا یورپ میں](https://www.kaggle.com/jiashenliu/515k-hotel-reviews-data-in-europe)۔ یہ تقریباً 230 MB ہے جب ان زپ کیا جائے۔ اسے ان NLP اسباق کے ساتھ وابستہ `/data` فولڈر میں ڈاؤن لوڈ کریں۔

## ڈیٹا کا تجزیاتی جائزہ

یہ چیلنج فرض کرتا ہے کہ آپ جذباتی تجزیہ اور مہمانوں کے جائزوں کے اسکورز کا استعمال کرتے ہوئے ایک ہوٹل کی سفارش کرنے والا بوٹ بنا رہے ہیں۔ ڈیٹا سیٹ جسے آپ استعمال کریں گے، 6 شہروں میں 1493 مختلف ہوٹلوں کے جائزے شامل ہیں۔

Python، ہوٹل کے جائزوں کے ڈیٹا سیٹ، اور NLTK کے جذباتی تجزیے کا استعمال کرتے ہوئے آپ معلوم کر سکتے ہیں:

* جائزوں میں سب سے زیادہ استعمال ہونے والے الفاظ اور جملے کون سے ہیں؟
* کیا ہوٹل کو بیان کرنے والے سرکاری *ٹیگز* جائزہ اسکورز سے مطابقت رکھتے ہیں (مثلاً، کیا کسی خاص ہوٹل کے لیے *چھوٹے بچوں کے ساتھ خاندان* کے لیے زیادہ منفی جائزے ہیں بجائے *اکیلے مسافر* کے، جو شاید ظاہر کرتا ہے کہ یہ *اکیلے مسافروں* کے لیے بہتر ہے؟)
* کیا NLTK کے جذباتی اسکورز ہوٹل کے جائزہ نگار کے عددی اسکور سے 'اتفاق' کرتے ہیں؟

#### ڈیٹا سیٹ

آئیے اس ڈیٹا سیٹ کو دریافت کریں جو آپ نے ڈاؤن لوڈ اور مقامی طور پر محفوظ کیا ہے۔ فائل کو کسی ایڈیٹر جیسے VS Code یا حتیٰ کہ Excel میں کھولیں۔

ڈیٹا سیٹ میں ہیڈرز درج ذیل ہیں:

*Hotel_Address, Additional_Number_of_Scoring, Review_Date, Average_Score, Hotel_Name, Reviewer_Nationality, Negative_Review, Review_Total_Negative_Word_Counts, Total_Number_of_Reviews, Positive_Review, Review_Total_Positive_Word_Counts, Total_Number_of_Reviews_Reviewer_Has_Given, Reviewer_Score, Tags, days_since_review, lat, lng*

یہاں انہیں اس طرح گروپ کیا گیا ہے کہ شاید ان کا جائزہ لینا آسان ہو:
##### ہوٹل کے کالمز

* `Hotel_Name`, `Hotel_Address`, `lat` (عرض بلد), `lng` (طول بلد)
  * *lat* اور *lng* کا استعمال کرتے ہوئے آپ Python کے ساتھ ہوٹل کے مقامات کا نقشہ بنا سکتے ہیں (شاید منفی اور مثبت جائزوں کے لیے رنگ کوڈنگ کے ساتھ)
  * Hotel_Address ہمارے لیے واضح طور پر مفید نہیں ہے، اور ہم شاید اسے آسان ترتیب اور تلاش کے لیے ملک کے ساتھ تبدیل کریں گے

**ہوٹل میٹا-جائزہ کالمز**

* `Average_Score`
  * ڈیٹا سیٹ کے تخلیق کار کے مطابق، یہ کالم ہوٹل کا *اوسط اسکور ہے، جو پچھلے سال کے تازہ ترین تبصرے کی بنیاد پر حساب کیا گیا ہے*۔ یہ اسکور حساب کرنے کا ایک غیر معمولی طریقہ لگتا ہے، لیکن یہ حاصل کردہ ڈیٹا ہے، لہذا ہم فی الحال اسے قبول کر سکتے ہیں۔

  ✅ اس ڈیٹا میں موجود دیگر کالمز کی بنیاد پر، کیا آپ اوسط اسکور حساب کرنے کا کوئی اور طریقہ سوچ سکتے ہیں؟

* `Total_Number_of_Reviews`
  * اس ہوٹل کو موصول ہونے والے جائزوں کی کل تعداد - یہ واضح نہیں ہے (کوڈ لکھے بغیر) کہ آیا یہ ڈیٹا سیٹ میں موجود جائزوں کا حوالہ دیتا ہے۔
* `Additional_Number_of_Scoring`
  * اس کا مطلب ہے کہ جائزہ اسکور دیا گیا لیکن جائزہ نگار نے کوئی مثبت یا منفی جائزہ نہیں لکھا

**جائزہ کالمز**

- `Reviewer_Score`
  - یہ ایک عددی قدر ہے جس میں زیادہ سے زیادہ 1 اعشاریہ جگہ ہوتی ہے، اور اس کی کم سے کم اور زیادہ سے زیادہ قدریں 2.5 اور 10 کے درمیان ہوتی ہیں
  - یہ واضح نہیں کیا گیا کہ 2.5 کم سے کم ممکنہ اسکور کیوں ہے
- `Negative_Review`
  - اگر جائزہ نگار نے کچھ نہیں لکھا، تو یہ فیلڈ "**No Negative**" ہوگا
  - نوٹ کریں کہ جائزہ نگار منفی جائزہ کالم میں مثبت جائزہ لکھ سکتا ہے (مثلاً "اس ہوٹل کے بارے میں کوئی بری بات نہیں ہے")
- `Review_Total_Negative_Word_Counts`
  - زیادہ منفی الفاظ کی تعداد کم اسکور کی نشاندہی کرتی ہے (جذباتیت کی جانچ کیے بغیر)
- `Positive_Review`
  - اگر جائزہ نگار نے کچھ نہیں لکھا، تو یہ فیلڈ "**No Positive**" ہوگا
  - نوٹ کریں کہ جائزہ نگار مثبت جائزہ کالم میں منفی جائزہ لکھ سکتا ہے (مثلاً "اس ہوٹل کے بارے میں کچھ بھی اچھا نہیں ہے")
- `Review_Total_Positive_Word_Counts`
  - زیادہ مثبت الفاظ کی تعداد زیادہ اسکور کی نشاندہی کرتی ہے (جذباتیت کی جانچ کیے بغیر)
- `Review_Date` اور `days_since_review`
  - جائزے پر تازگی یا پرانی ہونے کا پیمانہ لگایا جا سکتا ہے (پرانے جائزے اتنے درست نہیں ہو سکتے جتنے نئے، کیونکہ ہوٹل کی انتظامیہ بدل گئی ہو، یا تزئین و آرائش کی گئی ہو، یا ایک پول شامل کیا گیا ہو وغیرہ)
- `Tags`
  - یہ مختصر وضاحتیں ہیں جو جائزہ نگار منتخب کر سکتا ہے تاکہ وہ مہمان کی قسم، کمرے کی قسم، قیام کی مدت اور جائزہ کیسے جمع کرایا گیا تھا، بیان کرے۔
  - بدقسمتی سے، ان ٹیگز کا استعمال مسئلہ پیدا کرتا ہے، نیچے دیے گئے سیکشن میں ان کی افادیت پر تبصرہ کیا گیا ہے

**جائزہ نگار کالمز**

- `Total_Number_of_Reviews_Reviewer_Has_Given`
  - یہ سفارش ماڈل میں ایک عنصر ہو سکتا ہے، مثلاً، اگر آپ یہ تعین کر سکیں کہ زیادہ پروفیشنل جائزہ نگار جن کے پاس سینکڑوں جائزے ہیں، زیادہ منفی ہونے کا امکان رکھتے ہیں بجائے مثبت ہونے کے۔ تاہم، کسی خاص جائزے کے جائزہ نگار کو ایک منفرد کوڈ کے ساتھ شناخت نہیں کیا گیا ہے، اور اس لیے اسے جائزوں کے سیٹ سے منسلک نہیں کیا جا سکتا۔ 30 جائزہ نگار ہیں جن کے پاس 100 یا اس سے زیادہ جائزے ہیں، لیکن یہ دیکھنا مشکل ہے کہ یہ سفارش ماڈل میں کیسے مدد دے سکتا ہے۔
- `Reviewer_Nationality`
  - کچھ لوگ سوچ سکتے ہیں کہ کچھ قومیتیں مثبت یا منفی جائزہ دینے کا زیادہ امکان رکھتی ہیں کیونکہ ان کی قومی رجحانیت ہے۔ اپنے ماڈلز میں ایسے خیالی خیالات شامل کرنے سے محتاط رہیں۔ یہ قومی (اور کبھی کبھار نسلی) دقیانوسی تصورات ہیں، اور ہر جائزہ نگار ایک فرد تھا جس نے اپنے تجربے کی بنیاد پر جائزہ لکھا۔ یہ کئی زاویوں سے فلٹر کیا جا سکتا ہے جیسے ان کے پچھلے ہوٹل کے قیام، سفر کی دوری، اور ان کی ذاتی مزاج۔ یہ سوچنا کہ ان کی قومیت جائزہ اسکور کی وجہ تھی، ثابت کرنا مشکل ہے۔

##### مثالیں

| Average  Score | Total Number   Reviews | Reviewer   Score | Negative <br />Review                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Positive   Review                 | Tags                                                                                      |
| -------------- | ---------------------- | ---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------------------------------------------------------------------------------------- |
| 7.8            | 1945                   | 2.5              | یہ فی الحال ایک ہوٹل نہیں بلکہ ایک تعمیراتی سائٹ ہے۔ مجھے صبح سویرے اور پورے دن ناقابل قبول تعمیراتی شور سے پریشان کیا گیا جبکہ میں ایک طویل سفر کے بعد آرام کر رہا تھا اور کمرے میں کام کر رہا تھا۔ لوگ پورے دن کام کر رہے تھے، یعنی جیک ہتھوڑے کے ساتھ ملحقہ کمروں میں۔ میں نے کمرہ تبدیل کرنے کی درخواست کی لیکن کوئی خاموش کمرہ دستیاب نہیں تھا۔ مزید برآں، مجھے زیادہ چارج کیا گیا۔ میں نے شام کو چیک آؤٹ کیا کیونکہ مجھے بہت جلدی پرواز کے لیے روانہ ہونا تھا اور مناسب بل موصول ہوا۔ ایک دن بعد ہوٹل نے بغیر میری اجازت کے بکنگ کی قیمت سے زیادہ چارج کیا۔ یہ ایک خوفناک جگہ ہے۔ خود کو یہاں بکنگ کرکے سزا نہ دیں۔ | کچھ بھی نہیں۔ خوفناک جگہ۔ دور رہیں۔ | کاروباری سفر۔ جوڑا۔ معیاری ڈبل کمرہ۔ 2 راتیں قیام۔ |

جیسا کہ آپ دیکھ سکتے ہیں، اس مہمان کا ہوٹل میں قیام خوشگوار نہیں تھا۔ ہوٹل کا اوسط اسکور 7.8 اور 1945 جائزے ہیں، لیکن اس جائزہ نگار نے اسے 2.5 دیا اور 115 الفاظ لکھے کہ ان کا قیام کتنا منفی تھا۔ اگر انہوں نے مثبت جائزہ کالم میں کچھ بھی نہیں لکھا، تو آپ اندازہ لگا سکتے ہیں کہ کچھ بھی مثبت نہیں تھا، لیکن انہوں نے 7 الفاظ کی وارننگ لکھی۔ اگر ہم صرف الفاظ گنتے ہیں بجائے الفاظ کے معنی یا جذبات کے، تو ہم جائزہ نگار کے ارادے کا غلط اندازہ لگا سکتے ہیں۔ عجیب بات یہ ہے کہ ان کا اسکور 2.5 الجھن پیدا کرتا ہے، کیونکہ اگر ہوٹل کا قیام اتنا برا تھا، تو اسے کوئی پوائنٹس کیوں دیے گئے؟ ڈیٹا سیٹ کا قریب سے جائزہ لیتے ہوئے، آپ دیکھیں گے کہ کم سے کم ممکنہ اسکور 2.5 ہے، 0 نہیں۔ زیادہ سے زیادہ ممکنہ اسکور 10 ہے۔

##### ٹیگز

جیسا کہ اوپر ذکر کیا گیا ہے، پہلی نظر میں، `Tags` کا استعمال ڈیٹا کو زمرہ بندی کرنے کے لیے منطقی لگتا ہے۔ بدقسمتی سے، یہ ٹیگز معیاری نہیں ہیں، جس کا مطلب ہے کہ ایک دیے گئے ہوٹل میں، اختیارات *سنگل روم*, *ٹوئن روم*, اور *ڈبل روم* ہو سکتے ہیں، لیکن اگلے ہوٹل میں، وہ *ڈیلکس سنگل روم*, *کلاسک کوئین روم*, اور *ایگزیکٹو کنگ روم* ہو سکتے ہیں۔ یہ شاید ایک ہی چیزیں ہوں، لیکن اتنی مختلف حالتیں ہیں کہ انتخاب یہ بن جاتا ہے:

1. تمام اصطلاحات کو ایک واحد معیار میں تبدیل کرنے کی کوشش کریں، جو بہت مشکل ہے، کیونکہ یہ واضح نہیں ہے کہ ہر معاملے میں تبدیلی کا راستہ کیا ہوگا (مثلاً، *کلاسک سنگل روم* کو *سنگل روم* میں میپ کیا جا سکتا ہے لیکن *سپیریئر کوئین روم ود کورٹ یارڈ گارڈن اور سٹی ویو* کو میپ کرنا بہت مشکل ہے)

1. ہم ایک NLP طریقہ اختیار کر سکتے ہیں اور کچھ اصطلاحات جیسے *اکیلا*, *کاروباری مسافر*, یا *چھوٹے بچوں کے ساتھ خاندان* کی فریکوئنسی کو ہر ہوٹل پر لاگو کر سکتے ہیں، اور اسے سفارش میں شامل کر سکتے ہیں  

ٹیگز عام طور پر (لیکن ہمیشہ نہیں) ایک واحد فیلڈ ہوتے ہیں جس میں 5 سے 6 کاما سے جدا شدہ اقدار شامل ہوتی ہیں جو *سفر کی قسم*, *مہمانوں کی قسم*, *کمرے کی قسم*, *راتوں کی تعداد*, اور *جائزہ کس قسم کے ڈیوائس پر جمع کرایا گیا تھا* سے مطابقت رکھتے ہیں۔ تاہم، کیونکہ کچھ جائزہ نگار ہر فیلڈ کو نہیں بھرتے (وہ ایک کو خالی چھوڑ سکتے ہیں)، اقدار ہمیشہ ایک ہی ترتیب میں نہیں ہوتی ہیں۔

مثال کے طور پر، *گروپ کی قسم* لیں۔ اس فیلڈ میں `Tags` کالم میں 1025 منفرد امکانات ہیں، اور بدقسمتی سے ان میں سے صرف کچھ گروپ کا حوالہ دیتے ہیں (کچھ کمرے کی قسم وغیرہ ہیں)۔ اگر آپ صرف ان کو فلٹر کریں جو خاندان کا ذکر کرتے ہیں، تو نتائج میں بہت سے *فیملی روم* قسم کے نتائج شامل ہیں۔ اگر آپ اصطلاح *کے ساتھ* شامل کریں، یعنی *فیملی کے ساتھ* اقدار گنیں، تو نتائج بہتر ہیں، 515,000 نتائج میں سے 80,000 سے زیادہ میں "چھوٹے بچوں کے ساتھ خاندان" یا "بڑے بچوں کے ساتھ خاندان" کا جملہ شامل ہے۔

اس کا مطلب ہے کہ ٹیگز کالم ہمارے لیے مکمل طور پر بے کار نہیں ہے، لیکن اسے مفید بنانے کے لیے کچھ کام کرنا پڑے گا۔

##### ہوٹل کا اوسط اسکور

ڈیٹا سیٹ میں کچھ عجیب و غریب چیزیں یا تضادات ہیں جنہیں میں سمجھ نہیں سکتا، لیکن انہیں یہاں بیان کیا گیا ہے تاکہ آپ ان سے آگاہ ہوں جب آپ اپنے ماڈلز بنا رہے ہوں۔ اگر آپ اسے سمجھتے ہیں، تو براہ کرم ہمیں بحث کے سیکشن میں بتائیں!

ڈیٹا سیٹ میں اوسط اسکور اور جائزوں کی تعداد سے متعلق درج ذیل کالمز ہیں:

1. Hotel_Name
2. Additional_Number_of_Scoring
3. Average_Score
4. Total_Number_of_Reviews
5. Reviewer_Score  

اس ڈیٹا سیٹ میں سب سے زیادہ جائزوں والا واحد ہوٹل *Britannia International Hotel Canary Wharf* ہے جس کے 4789 جائزے ہیں، 515,000 میں سے۔ لیکن اگر ہم اس ہوٹل کے لیے `Total_Number_of_Reviews` ویلیو دیکھیں، تو یہ 9086 ہے۔ آپ اندازہ لگا سکتے ہیں کہ بہت زیادہ اسکورز بغیر جائزوں کے ہیں، لہذا شاید ہمیں `Additional_Number_of_Scoring` کالم ویلیو شامل کرنی چاہیے۔ وہ ویلیو 2682 ہے، اور اسے 4789 میں شامل کرنے سے ہمیں 7471 ملتا ہے جو اب بھی `Total_Number_of_Reviews` سے 1615 کم ہے۔

اگر آپ `Average_Score` کالمز لیں، تو آپ اندازہ لگا سکتے ہیں کہ یہ ڈیٹا سیٹ میں موجود جائزوں کے اوسط کا حوالہ دیتا ہے، لیکن Kaggle کی وضاحت ہے "*ہوٹل کا اوسط اسکور، جو پچھلے سال کے تازہ ترین تبصرے کی بنیاد پر حساب کیا گیا ہے*۔" یہ اتنا مفید نہیں لگتا، لیکن ہم اپنے جائزہ اسکورز کی بنیاد پر اپنا اوسط حساب کر سکتے ہیں۔ اسی ہوٹل کو مثال کے طور پر لیتے ہوئے، ہوٹل کا اوسط اسکور 7.1 دیا گیا ہے لیکن ڈیٹا سیٹ میں موجود جائزہ نگار کے اسکورز کا حساب کردہ اوسط 6.8 ہے۔ یہ قریب ہے، لیکن ایک جیسا نہیں، اور ہم صرف اندازہ لگا سکتے ہیں کہ `Additional_Number_of_Scoring` جائزوں میں دیے گئے اسکورز نے اوسط کو 7.1 تک بڑھا دیا۔ بدقسمتی سے اس دعوے کو جانچنے یا ثابت کرنے کا کوئی طریقہ نہ ہونے کے ساتھ، `Average_Score`, `Additional_Number_of_Scoring` اور `Total_Number_of_Reviews` کا استعمال یا ان پر اعتماد کرنا مشکل ہے جب وہ ڈیٹا پر مبنی ہوں یا اس کا حوالہ دیتے ہوں جو ہمارے پاس نہیں ہے۔

چیزوں کو مزید پیچیدہ بنانے کے لیے، دوسرے سب سے زیادہ جائزوں والے ہوٹل کا حساب کردہ اوسط اسکور 8.12 ہے اور ڈیٹا سیٹ `Average_Score` 8.1 ہے۔ کیا یہ درست اسکور ایک اتفاق ہے یا پہلا ہوٹل ایک تضاد ہے؟
ان ہوٹلوں کے ممکنہ طور پر غیر معمولی ہونے کے امکان پر، اور شاید زیادہ تر اقدار درست ہوں (لیکن کچھ کسی وجہ سے نہیں)، ہم اگلے مرحلے میں ایک مختصر پروگرام لکھیں گے تاکہ ڈیٹا سیٹ میں موجود اقدار کو دریافت کریں اور ان کے درست استعمال (یا عدم استعمال) کا تعین کریں۔

> 🚨 ایک احتیاطی نوٹ
>
> جب آپ اس ڈیٹا سیٹ کے ساتھ کام کر رہے ہوں گے، تو آپ ایسا کوڈ لکھیں گے جو متن سے کچھ حساب لگائے بغیر کہ آپ خود متن کو پڑھیں یا تجزیہ کریں۔ یہی NLP کا جوہر ہے، مطلب یا جذبات کی تشریح کرنا بغیر کسی انسان کے۔ تاہم، یہ ممکن ہے کہ آپ کچھ منفی جائزے پڑھیں۔ میں آپ کو ایسا نہ کرنے کی تاکید کروں گا، کیونکہ آپ کو اس کی ضرورت نہیں ہے۔ ان میں سے کچھ بے وقوفانہ یا غیر متعلقہ منفی ہوٹل کے جائزے ہو سکتے ہیں، جیسے "موسم اچھا نہیں تھا"، جو ہوٹل یا کسی کے بھی قابو سے باہر ہے۔ لیکن کچھ جائزوں کا تاریک پہلو بھی ہوتا ہے۔ کبھی کبھار منفی جائزے نسل پرستی، جنس پرستی، یا عمر پرستی پر مبنی ہوتے ہیں۔ یہ بدقسمتی ہے لیکن عوامی ویب سائٹ سے حاصل کردہ ڈیٹا سیٹ میں متوقع ہے۔ کچھ جائزہ نگار ایسے جائزے چھوڑتے ہیں جو آپ کو ناپسندیدہ، غیر آرام دہ، یا پریشان کن لگ سکتے ہیں۔ بہتر ہے کہ کوڈ جذبات کی پیمائش کرے بجائے اس کے کہ آپ خود انہیں پڑھ کر پریشان ہوں۔ یہ کہا جا سکتا ہے کہ ایسے لوگ اقلیت میں ہیں، لیکن وہ موجود ہیں۔

## مشق - ڈیٹا کی دریافت
### ڈیٹا لوڈ کریں

ڈیٹا کو بصری طور پر جانچنے کے لیے کافی ہے، اب آپ کچھ کوڈ لکھیں گے اور کچھ جوابات حاصل کریں گے! اس سیکشن میں pandas لائبریری استعمال کی گئی ہے۔ آپ کا پہلا کام یہ یقینی بنانا ہے کہ آپ CSV ڈیٹا کو لوڈ اور پڑھ سکتے ہیں۔ pandas لائبریری کے پاس ایک تیز CSV لوڈر ہے، اور نتیجہ ایک ڈیٹا فریم میں رکھا جاتا ہے، جیسا کہ پچھلے اسباق میں۔ جو CSV ہم لوڈ کر رہے ہیں اس میں پانچ لاکھ سے زیادہ قطاریں ہیں، لیکن صرف 17 کالم ہیں۔ pandas آپ کو ڈیٹا فریم کے ساتھ تعامل کرنے کے بہت سے طاقتور طریقے فراہم کرتا ہے، بشمول ہر قطار پر آپریشن کرنے کی صلاحیت۔

اب سے اس سبق میں، کوڈ کے ٹکڑے اور کوڈ کی کچھ وضاحتیں اور نتائج کے بارے میں کچھ گفتگو ہوگی۔ اپنے کوڈ کے لیے شامل کردہ _notebook.ipynb_ استعمال کریں۔

آئیے اس ڈیٹا فائل کو لوڈ کرنے سے شروع کریں جسے آپ استعمال کریں گے:

```python
# Load the hotel reviews from CSV
import pandas as pd
import time
# importing time so the start and end time can be used to calculate file loading time
print("Loading data file now, this could take a while depending on file size")
start = time.time()
# df is 'DataFrame' - make sure you downloaded the file to the data folder
df = pd.read_csv('../../data/Hotel_Reviews.csv')
end = time.time()
print("Loading took " + str(round(end - start, 2)) + " seconds")
```

اب جب کہ ڈیٹا لوڈ ہو چکا ہے، ہم اس پر کچھ آپریشنز کر سکتے ہیں۔ اس کوڈ کو اپنے پروگرام کے اوپر رکھیں اگلے حصے کے لیے۔

## ڈیٹا کی دریافت

اس معاملے میں، ڈیٹا پہلے سے *صاف* ہے، یعنی یہ کام کرنے کے لیے تیار ہے، اور اس میں دوسرے زبانوں کے حروف نہیں ہیں جو صرف انگریزی حروف کی توقع کرنے والے الگورتھم کو پریشان کر سکتے ہیں۔

✅ آپ کو ایسے ڈیٹا کے ساتھ کام کرنا پڑ سکتا ہے جسے NLP تکنیکوں کو لاگو کرنے سے پہلے فارمیٹ کرنے کے لیے کچھ ابتدائی پروسیسنگ کی ضرورت ہو، لیکن اس بار نہیں۔ اگر آپ کو ایسا کرنا پڑے تو آپ غیر انگریزی حروف کو کیسے ہینڈل کریں گے؟

ایک لمحہ نکالیں تاکہ یہ یقینی بنایا جا سکے کہ ڈیٹا لوڈ ہونے کے بعد، آپ اسے کوڈ کے ساتھ دریافت کر سکتے ہیں۔ `Negative_Review` اور `Positive_Review` کالمز پر توجہ مرکوز کرنا بہت آسان ہے۔ یہ قدرتی متن سے بھرے ہوئے ہیں جو آپ کے NLP الگورتھم کے لیے پروسیس کرنے کے لیے ہیں۔ لیکن رکیں! NLP اور جذبات پر جانے سے پہلے، آپ کو نیچے دیے گئے کوڈ کی پیروی کرنی چاہیے تاکہ یہ معلوم کیا جا سکے کہ ڈیٹا سیٹ میں دی گئی اقدار pandas کے ساتھ حساب کردہ اقدار سے مطابقت رکھتی ہیں یا نہیں۔

## ڈیٹا فریم آپریشنز

اس سبق میں پہلا کام یہ ہے کہ درج ذیل دعووں کی درستگی کو جانچنے کے لیے کچھ کوڈ لکھیں جو ڈیٹا فریم کا جائزہ لے (بغیر اسے تبدیل کیے)۔

> بہت سے پروگرامنگ کاموں کی طرح، انہیں مکمل کرنے کے کئی طریقے ہیں، لیکن اچھا مشورہ یہ ہے کہ اسے سب سے آسان اور آسان طریقے سے کریں، خاص طور پر اگر یہ مستقبل میں اس کوڈ کو سمجھنے میں آسان ہو۔ ڈیٹا فریمز کے ساتھ، ایک جامع API موجود ہے جو اکثر آپ کے مطلوبہ کام کو مؤثر طریقے سے انجام دینے کا طریقہ فراہم کرے گا۔

درج ذیل سوالات کو کوڈنگ کے کاموں کے طور پر سمجھیں اور انہیں حل کرنے کی کوشش کریں بغیر حل دیکھے۔

1. اس ڈیٹا فریم کی *شکل* پرنٹ کریں جو آپ نے ابھی لوڈ کیا ہے (شکل قطاروں اور کالمز کی تعداد ہے)
2. جائزہ نگاروں کی قومیت کے لیے تعدد شمار کریں:
   1. `Reviewer_Nationality` کالم کے لیے کتنی مختلف اقدار ہیں اور وہ کیا ہیں؟
   2. ڈیٹا سیٹ میں سب سے عام جائزہ نگار قومیت کون سی ہے (ملک اور جائزوں کی تعداد پرنٹ کریں)؟
   3. اگلی 10 سب سے زیادہ بار پائی جانے والی قومیتیں اور ان کی تعدد شمار کیا ہیں؟
3. ہر قومیت کے لیے سب سے زیادہ جائزہ لیا گیا ہوٹل کیا تھا؟
4. ڈیٹا سیٹ میں ہر ہوٹل کے لیے کتنے جائزے ہیں (ہوٹل کی تعدد شمار)؟
5. جبکہ ڈیٹا سیٹ میں ہر ہوٹل کے لیے `Average_Score` کالم موجود ہے، آپ ایک اوسط اسکور بھی حساب کر سکتے ہیں (ڈیٹا سیٹ میں ہر ہوٹل کے لیے تمام جائزہ نگار اسکورز کا اوسط حاصل کرنا)۔ اپنے ڈیٹا فریم میں ایک نیا کالم شامل کریں جس کا کالم ہیڈر `Calc_Average_Score` ہو جس میں وہ حساب کردہ اوسط ہو۔ 
6. کیا کسی ہوٹل کے پاس ایک جیسا (1 اعشاریہ تک گول) `Average_Score` اور `Calc_Average_Score` ہے؟
   1. ایک Python فنکشن لکھنے کی کوشش کریں جو ایک سیریز (قطار) کو دلیل کے طور پر لے اور اقدار کا موازنہ کرے، جب اقدار برابر نہ ہوں تو ایک پیغام پرنٹ کرے۔ پھر `.apply()` طریقہ استعمال کریں تاکہ ہر قطار کو فنکشن کے ساتھ پروسیس کیا جا سکے۔
7. `Negative_Review` کالم میں "No Negative" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔
8. `Positive_Review` کالم میں "No Positive" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔
9. `Positive_Review` کالم میں "No Positive" **اور** `Negative_Review` کالم میں "No Negative" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔

### کوڈ کے جوابات

1. اس ڈیٹا فریم کی *شکل* پرنٹ کریں جو آپ نے ابھی لوڈ کیا ہے (شکل قطاروں اور کالمز کی تعداد ہے)

   ```python
   print("The shape of the data (rows, cols) is " + str(df.shape))
   > The shape of the data (rows, cols) is (515738, 17)
   ```

2. جائزہ نگاروں کی قومیت کے لیے تعدد شمار کریں:

   1. `Reviewer_Nationality` کالم کے لیے کتنی مختلف اقدار ہیں اور وہ کیا ہیں؟
   2. ڈیٹا سیٹ میں سب سے عام جائزہ نگار قومیت کون سی ہے (ملک اور جائزوں کی تعداد پرنٹ کریں)؟

   ```python
   # value_counts() creates a Series object that has index and values in this case, the country and the frequency they occur in reviewer nationality
   nationality_freq = df["Reviewer_Nationality"].value_counts()
   print("There are " + str(nationality_freq.size) + " different nationalities")
   # print first and last rows of the Series. Change to nationality_freq.to_string() to print all of the data
   print(nationality_freq) 
   
   There are 227 different nationalities
    United Kingdom               245246
    United States of America      35437
    Australia                     21686
    Ireland                       14827
    United Arab Emirates          10235
                                  ...  
    Comoros                           1
    Palau                             1
    Northern Mariana Islands          1
    Cape Verde                        1
    Guinea                            1
   Name: Reviewer_Nationality, Length: 227, dtype: int64
   ```

   3. اگلی 10 سب سے زیادہ بار پائی جانے والی قومیتیں اور ان کی تعدد شمار کیا ہیں؟

      ```python
      print("The highest frequency reviewer nationality is " + str(nationality_freq.index[0]).strip() + " with " + str(nationality_freq[0]) + " reviews.")
      # Notice there is a leading space on the values, strip() removes that for printing
      # What is the top 10 most common nationalities and their frequencies?
      print("The next 10 highest frequency reviewer nationalities are:")
      print(nationality_freq[1:11].to_string())
      
      The highest frequency reviewer nationality is United Kingdom with 245246 reviews.
      The next 10 highest frequency reviewer nationalities are:
       United States of America     35437
       Australia                    21686
       Ireland                      14827
       United Arab Emirates         10235
       Saudi Arabia                  8951
       Netherlands                   8772
       Switzerland                   8678
       Germany                       7941
       Canada                        7894
       France                        7296
      ```

3. ہر قومیت کے لیے سب سے زیادہ جائزہ لیا گیا ہوٹل کیا تھا؟

   ```python
   # What was the most frequently reviewed hotel for the top 10 nationalities
   # Normally with pandas you will avoid an explicit loop, but wanted to show creating a new dataframe using criteria (don't do this with large amounts of data because it could be very slow)
   for nat in nationality_freq[:10].index:
      # First, extract all the rows that match the criteria into a new dataframe
      nat_df = df[df["Reviewer_Nationality"] == nat]   
      # Now get the hotel freq
      freq = nat_df["Hotel_Name"].value_counts()
      print("The most reviewed hotel for " + str(nat).strip() + " was " + str(freq.index[0]) + " with " + str(freq[0]) + " reviews.") 
      
   The most reviewed hotel for United Kingdom was Britannia International Hotel Canary Wharf with 3833 reviews.
   The most reviewed hotel for United States of America was Hotel Esther a with 423 reviews.
   The most reviewed hotel for Australia was Park Plaza Westminster Bridge London with 167 reviews.
   The most reviewed hotel for Ireland was Copthorne Tara Hotel London Kensington with 239 reviews.
   The most reviewed hotel for United Arab Emirates was Millennium Hotel London Knightsbridge with 129 reviews.
   The most reviewed hotel for Saudi Arabia was The Cumberland A Guoman Hotel with 142 reviews.
   The most reviewed hotel for Netherlands was Jaz Amsterdam with 97 reviews.
   The most reviewed hotel for Switzerland was Hotel Da Vinci with 97 reviews.
   The most reviewed hotel for Germany was Hotel Da Vinci with 86 reviews.
   The most reviewed hotel for Canada was St James Court A Taj Hotel London with 61 reviews.
   ```

4. ڈیٹا سیٹ میں ہر ہوٹل کے لیے کتنے جائزے ہیں (ہوٹل کی تعدد شمار)؟

   ```python
   # First create a new dataframe based on the old one, removing the uneeded columns
   hotel_freq_df = df.drop(["Hotel_Address", "Additional_Number_of_Scoring", "Review_Date", "Average_Score", "Reviewer_Nationality", "Negative_Review", "Review_Total_Negative_Word_Counts", "Positive_Review", "Review_Total_Positive_Word_Counts", "Total_Number_of_Reviews_Reviewer_Has_Given", "Reviewer_Score", "Tags", "days_since_review", "lat", "lng"], axis = 1)
   
   # Group the rows by Hotel_Name, count them and put the result in a new column Total_Reviews_Found
   hotel_freq_df['Total_Reviews_Found'] = hotel_freq_df.groupby('Hotel_Name').transform('count')
   
   # Get rid of all the duplicated rows
   hotel_freq_df = hotel_freq_df.drop_duplicates(subset = ["Hotel_Name"])
   display(hotel_freq_df) 
   ```
   |                 Hotel_Name                 | Total_Number_of_Reviews | Total_Reviews_Found |
   | :----------------------------------------: | :---------------------: | :-----------------: |
   | Britannia International Hotel Canary Wharf |          9086           |        4789         |
   |    Park Plaza Westminster Bridge London    |          12158          |        4169         |
   |   Copthorne Tara Hotel London Kensington   |          7105           |        3578         |
   |                    ...                     |           ...           |         ...         |
   |       Mercure Paris Porte d Orleans        |           110           |         10          |
   |                Hotel Wagner                |           135           |         10          |
   |            Hotel Gallitzinberg             |           173           |          8          |
   
   آپ نے دیکھا ہوگا کہ *ڈیٹا سیٹ میں شمار کردہ* نتائج `Total_Number_of_Reviews` کی قدر سے میل نہیں کھاتے۔ یہ واضح نہیں ہے کہ آیا ڈیٹا سیٹ میں یہ قدر ہوٹل کے کل جائزوں کی نمائندگی کرتی ہے، لیکن سب کو حاصل نہیں کیا گیا، یا کچھ اور حساب۔ `Total_Number_of_Reviews` ماڈل میں استعمال نہیں کیا جاتا کیونکہ یہ غیر واضح ہے۔

5. جبکہ ڈیٹا سیٹ میں ہر ہوٹل کے لیے `Average_Score` کالم موجود ہے، آپ ایک اوسط اسکور بھی حساب کر سکتے ہیں (ڈیٹا سیٹ میں ہر ہوٹل کے لیے تمام جائزہ نگار اسکورز کا اوسط حاصل کرنا)۔ اپنے ڈیٹا فریم میں ایک نیا کالم شامل کریں جس کا کالم ہیڈر `Calc_Average_Score` ہو جس میں وہ حساب کردہ اوسط ہو۔ `Hotel_Name`, `Average_Score`, اور `Calc_Average_Score` کالمز پرنٹ کریں۔

   ```python
   # define a function that takes a row and performs some calculation with it
   def get_difference_review_avg(row):
     return row["Average_Score"] - row["Calc_Average_Score"]
   
   # 'mean' is mathematical word for 'average'
   df['Calc_Average_Score'] = round(df.groupby('Hotel_Name').Reviewer_Score.transform('mean'), 1)
   
   # Add a new column with the difference between the two average scores
   df["Average_Score_Difference"] = df.apply(get_difference_review_avg, axis = 1)
   
   # Create a df without all the duplicates of Hotel_Name (so only 1 row per hotel)
   review_scores_df = df.drop_duplicates(subset = ["Hotel_Name"])
   
   # Sort the dataframe to find the lowest and highest average score difference
   review_scores_df = review_scores_df.sort_values(by=["Average_Score_Difference"])
   
   display(review_scores_df[["Average_Score_Difference", "Average_Score", "Calc_Average_Score", "Hotel_Name"]])
   ```

   آپ `Average_Score` قدر کے بارے میں بھی سوچ سکتے ہیں اور کیوں یہ کبھی کبھار حساب کردہ اوسط اسکور سے مختلف ہوتی ہے۔ چونکہ ہم نہیں جان سکتے کہ کیوں کچھ اقدار میل کھاتی ہیں، لیکن دوسروں میں فرق ہے، اس معاملے میں محفوظ ترین طریقہ یہ ہے کہ ہم اپنے پاس موجود جائزہ اسکورز کا استعمال کرتے ہوئے خود اوسط حساب کریں۔ یہ کہا جا سکتا ہے کہ فرق عام طور پر بہت چھوٹا ہوتا ہے، یہاں وہ ہوٹل ہیں جن کے ڈیٹا سیٹ اوسط اور حساب کردہ اوسط میں سب سے زیادہ انحراف ہے:

   | Average_Score_Difference | Average_Score | Calc_Average_Score |                                  Hotel_Name |
   | :----------------------: | :-----------: | :----------------: | ------------------------------------------: |
   |           -0.8           |      7.7      |        8.5         |                  Best Western Hotel Astoria |
   |           -0.7           |      8.8      |        9.5         | Hotel Stendhal Place Vend me Paris MGallery |
   |           -0.7           |      7.5      |        8.2         |               Mercure Paris Porte d Orleans |
   |           -0.7           |      7.9      |        8.6         |             Renaissance Paris Vendome Hotel |
   |           -0.5           |      7.0      |        7.5         |                         Hotel Royal Elys es |
   |           ...            |      ...      |        ...         |                                         ... |
   |           0.7            |      7.5      |        6.8         |     Mercure Paris Op ra Faubourg Montmartre |
   |           0.8            |      7.1      |        6.3         |      Holiday Inn Paris Montparnasse Pasteur |
   |           0.9            |      6.8      |        5.9         |                               Villa Eugenie |
   |           0.9            |      8.6      |        7.7         |   MARQUIS Faubourg St Honor Relais Ch teaux |
   |           1.3            |      7.2      |        5.9         |                          Kube Hotel Ice Bar |

   چونکہ صرف 1 ہوٹل کے اسکور میں فرق 1 سے زیادہ ہے، اس کا مطلب ہے کہ ہم شاید فرق کو نظر انداز کر سکتے ہیں اور حساب کردہ اوسط اسکور استعمال کر سکتے ہیں۔

6. `Negative_Review` کالم میں "No Negative" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔

7. `Positive_Review` کالم میں "No Positive" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔

8. `Positive_Review` کالم میں "No Positive" **اور** `Negative_Review` کالم میں "No Negative" اقدار والی کتنی قطاریں ہیں، حساب کریں اور پرنٹ کریں۔

   ```python
   # with lambdas:
   start = time.time()
   no_negative_reviews = df.apply(lambda x: True if x['Negative_Review'] == "No Negative" else False , axis=1)
   print("Number of No Negative reviews: " + str(len(no_negative_reviews[no_negative_reviews == True].index)))
   
   no_positive_reviews = df.apply(lambda x: True if x['Positive_Review'] == "No Positive" else False , axis=1)
   print("Number of No Positive reviews: " + str(len(no_positive_reviews[no_positive_reviews == True].index)))
   
   both_no_reviews = df.apply(lambda x: True if x['Negative_Review'] == "No Negative" and x['Positive_Review'] == "No Positive" else False , axis=1)
   print("Number of both No Negative and No Positive reviews: " + str(len(both_no_reviews[both_no_reviews == True].index)))
   end = time.time()
   print("Lambdas took " + str(round(end - start, 2)) + " seconds")
   
   Number of No Negative reviews: 127890
   Number of No Positive reviews: 35946
   Number of both No Negative and No Positive reviews: 127
   Lambdas took 9.64 seconds
   ```

## ایک اور طریقہ

آئٹمز کو Lambdas کے بغیر شمار کرنے کا ایک اور طریقہ، اور قطاروں کو شمار کرنے کے لیے sum استعمال کریں:

   ```python
   # without lambdas (using a mixture of notations to show you can use both)
   start = time.time()
   no_negative_reviews = sum(df.Negative_Review == "No Negative")
   print("Number of No Negative reviews: " + str(no_negative_reviews))
   
   no_positive_reviews = sum(df["Positive_Review"] == "No Positive")
   print("Number of No Positive reviews: " + str(no_positive_reviews))
   
   both_no_reviews = sum((df.Negative_Review == "No Negative") & (df.Positive_Review == "No Positive"))
   print("Number of both No Negative and No Positive reviews: " + str(both_no_reviews))
   
   end = time.time()
   print("Sum took " + str(round(end - start, 2)) + " seconds")
   
   Number of No Negative reviews: 127890
   Number of No Positive reviews: 35946
   Number of both No Negative and No Positive reviews: 127
   Sum took 0.19 seconds
   ```

   آپ نے دیکھا ہوگا کہ 127 قطاریں ایسی ہیں جن میں `Negative_Review` اور `Positive_Review` کالمز کے لیے "No Negative" اور "No Positive" دونوں اقدار موجود ہیں۔ اس کا مطلب ہے کہ جائزہ نگار نے ہوٹل کو عددی اسکور دیا، لیکن مثبت یا منفی جائزہ لکھنے سے انکار کر دیا۔ خوش قسمتی سے یہ قطاروں کی ایک چھوٹی تعداد ہے (127 میں سے 515738، یا 0.02%)، لہذا یہ شاید ہمارے ماڈل یا نتائج کو کسی خاص سمت میں متاثر نہیں کرے گا، لیکن آپ نے شاید جائزوں کے ڈیٹا سیٹ میں ایسی قطاروں کی توقع نہیں کی ہوگی جن میں کوئی جائزہ نہ ہو، لہذا یہ ڈیٹا کو دریافت کرنے کے قابل ہے تاکہ ایسی قطاریں دریافت کی جا سکیں۔

اب جب کہ آپ نے ڈیٹا سیٹ کو دریافت کر لیا ہے، اگلے سبق میں آپ ڈیٹا کو فلٹر کریں گے اور کچھ جذباتی تجزیہ شامل کریں گے۔

---
## 🚀چیلنج

یہ سبق، جیسا کہ ہم نے پچھلے اسباق میں دیکھا، یہ ظاہر کرتا ہے کہ آپ کے ڈیٹا اور اس کی خامیوں کو سمجھنا آپریشنز انجام دینے سے پہلے کتنا اہم ہے۔ متن پر مبنی ڈیٹا، خاص طور پر، محتاط جانچ پڑتال کا متقاضی ہے۔ مختلف متن پر مبنی ڈیٹا سیٹس کو کھودیں اور دیکھیں کہ کیا آپ ایسے علاقے دریافت کر سکتے ہیں جو ماڈل میں تعصب یا جذباتی جھکاؤ متعارف کرا سکتے ہیں۔

## [لیکچر کے بعد کا کوئز](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/38/)

## جائزہ اور خود مطالعہ

[NLP پر یہ لرننگ پاتھ](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/?WT.mc_id=academic-77952-leestott) لیں تاکہ ان ٹولز کو دریافت کریں جنہیں آپ تقریر اور متن پر مبنی ماڈلز بناتے وقت آزما سکتے ہیں۔

## اسائنمنٹ 

[NLTK](assignment.md)

---

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا عدم درستگی ہو سکتی ہیں۔ اصل دستاویز کو اس کی اصل زبان میں مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے لیے ہم ذمہ دار نہیں ہیں۔