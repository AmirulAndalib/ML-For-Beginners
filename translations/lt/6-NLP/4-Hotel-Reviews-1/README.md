<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "3c4738bb0836dd838c552ab9cab7e09d",
  "translation_date": "2025-09-03T18:50:26+00:00",
  "source_file": "6-NLP/4-Hotel-Reviews-1/README.md",
  "language_code": "lt"
}
-->
# Sentimentų analizė su viešbučių apžvalgomis - duomenų apdorojimas

Šiame skyriuje naudosite ankstesnėse pamokose išmoktas technikas, kad atliktumėte didelio duomenų rinkinio tyrimą. Kai gerai suprasite įvairių stulpelių naudingumą, išmoksite:

- kaip pašalinti nereikalingus stulpelius
- kaip apskaičiuoti naujus duomenis remiantis esamais stulpeliais
- kaip išsaugoti gautą duomenų rinkinį, kad galėtumėte jį naudoti galutiniame iššūkyje

## [Prieš paskaitą - testas](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/37/)

### Įvadas

Iki šiol sužinojote, kad tekstiniai duomenys labai skiriasi nuo skaitinių duomenų tipų. Jei tekstą parašė ar pasakė žmogus, jį galima analizuoti, siekiant rasti dėsningumus, dažnius žodžius, sentimentus ir prasmę. Ši pamoka supažindins jus su realiu duomenų rinkiniu ir realiu iššūkiu: **[515 tūkst. viešbučių apžvalgų Europoje](https://www.kaggle.com/jiashenliu/515k-hotel-reviews-data-in-europe)**, kuris turi [CC0: Public Domain licenciją](https://creativecommons.org/publicdomain/zero/1.0/). Duomenys buvo surinkti iš Booking.com viešų šaltinių. Duomenų rinkinio kūrėjas yra Jiashen Liu.

### Pasiruošimas

Jums reikės:

* Galimybės paleisti .ipynb užrašų knygeles naudojant Python 3
* pandas bibliotekos
* NLTK, [kurią turėtumėte įdiegti lokaliai](https://www.nltk.org/install.html)
* Duomenų rinkinio, kuris yra prieinamas Kaggle [515K Hotel Reviews Data in Europe](https://www.kaggle.com/jiashenliu/515k-hotel-reviews-data-in-europe). Jo dydis yra apie 230 MB išpakuotas. Atsisiųskite jį į šaknies `/data` aplanką, susijusį su šiomis NLP pamokomis.

## Duomenų tyrimas

Šis iššūkis remiasi prielaida, kad kuriate viešbučių rekomendacijų botą, naudodami sentimentų analizę ir svečių apžvalgų įvertinimus. Duomenų rinkinys, kurį naudosite, apima 1493 skirtingų viešbučių apžvalgas iš 6 miestų.

Naudodami Python, viešbučių apžvalgų duomenų rinkinį ir NLTK sentimentų analizę, galite sužinoti:

* Kokie yra dažniausiai naudojami žodžiai ir frazės apžvalgose?
* Ar oficialios *žymos*, apibūdinančios viešbutį, koreliuoja su apžvalgų įvertinimais (pvz., ar tam tikro viešbučio apžvalgos yra labiau neigiamos *šeimoms su mažais vaikais* nei *keliautojams vieniems*, galbūt nurodant, kad jis labiau tinka *keliautojams vieniems*)?
* Ar NLTK sentimentų įvertinimai „sutampa“ su viešbučio apžvalgininko skaitiniu įvertinimu?

#### Duomenų rinkinys

Pažvelkime į duomenų rinkinį, kurį atsisiuntėte ir išsaugojote lokaliai. Atidarykite failą redaktoriuje, pvz., VS Code arba net Excel.

Duomenų rinkinio antraštės yra tokios:

*Hotel_Address, Additional_Number_of_Scoring, Review_Date, Average_Score, Hotel_Name, Reviewer_Nationality, Negative_Review, Review_Total_Negative_Word_Counts, Total_Number_of_Reviews, Positive_Review, Review_Total_Positive_Word_Counts, Total_Number_of_Reviews_Reviewer_Has_Given, Reviewer_Score, Tags, days_since_review, lat, lng*

Čia jos suskirstytos į grupes, kad būtų lengviau analizuoti: 
##### Viešbučių stulpeliai

* `Hotel_Name`, `Hotel_Address`, `lat` (platuma), `lng` (ilguma)
  * Naudodami *lat* ir *lng* galite sukurti žemėlapį su Python, rodantį viešbučių vietas (galbūt spalvotai pažymėtas pagal neigiamas ir teigiamas apžvalgas)
  * Hotel_Address nėra akivaizdžiai naudingas mums, ir greičiausiai jį pakeisime šalimi, kad būtų lengviau rūšiuoti ir ieškoti

**Viešbučių meta-apžvalgų stulpeliai**

* `Average_Score`
  * Pasak duomenų rinkinio kūrėjo, šis stulpelis yra *vidutinis viešbučio įvertinimas, apskaičiuotas remiantis naujausiais komentarais per pastaruosius metus*. Tai atrodo kaip neįprastas būdas apskaičiuoti įvertinimą, tačiau tai yra surinkti duomenys, todėl kol kas galime juos priimti kaip faktą. 
  
  ✅ Remdamiesi kitais šio duomenų rinkinio stulpeliais, ar galite sugalvoti kitą būdą apskaičiuoti vidutinį įvertinimą?

* `Total_Number_of_Reviews`
  * Bendras apžvalgų skaičius, kurį šis viešbutis gavo - nėra aišku (be kodo rašymo), ar tai reiškia apžvalgas, esančias duomenų rinkinyje.
* `Additional_Number_of_Scoring`
  * Tai reiškia, kad buvo pateiktas įvertinimas, tačiau apžvalgininkas neparašė teigiamos ar neigiamos apžvalgos

**Apžvalgų stulpeliai**

- `Reviewer_Score`
  - Tai skaitinė reikšmė, turinti daugiausiai 1 dešimtainį skaičių tarp minimalių ir maksimalių reikšmių 2.5 ir 10
  - Nėra paaiškinta, kodėl mažiausias galimas įvertinimas yra 2.5
- `Negative_Review`
  - Jei apžvalgininkas nieko neparašė, šiame lauke bus "**No Negative**"
  - Atkreipkite dėmesį, kad apžvalgininkas gali parašyti teigiamą apžvalgą neigiamų apžvalgų stulpelyje (pvz., "šiame viešbutyje nėra nieko blogo")
- `Review_Total_Negative_Word_Counts`
  - Didesnis neigiamų žodžių skaičius rodo žemesnį įvertinimą (neanalizuojant sentimentų)
- `Positive_Review`
  - Jei apžvalgininkas nieko neparašė, šiame lauke bus "**No Positive**"
  - Atkreipkite dėmesį, kad apžvalgininkas gali parašyti neigiamą apžvalgą teigiamų apžvalgų stulpelyje (pvz., "šiame viešbutyje nėra nieko gero")
- `Review_Total_Positive_Word_Counts`
  - Didesnis teigiamų žodžių skaičius rodo aukštesnį įvertinimą (neanalizuojant sentimentų)
- `Review_Date` ir `days_since_review`
  - Galima taikyti šviežumo ar senumo matą apžvalgai (senesnės apžvalgos gali būti ne tokios tikslios kaip naujesnės, nes viešbučio valdymas pasikeitė, buvo atlikti renovacijos darbai, pridėtas baseinas ir pan.)
- `Tags`
  - Tai trumpi aprašymai, kuriuos apžvalgininkas gali pasirinkti, kad apibūdintų svečio tipą (pvz., keliautojas vienas ar šeima), kambario tipą, buvimo trukmę ir kaip buvo pateikta apžvalga. 
  - Deja, šių žymų naudojimas yra problematiškas, žr. skyrių žemiau, kuriame aptariamas jų naudingumas

**Apžvalgininkų stulpeliai**

- `Total_Number_of_Reviews_Reviewer_Has_Given`
  - Tai gali būti veiksnys rekomendacijų modelyje, pavyzdžiui, jei galėtumėte nustatyti, kad produktyvesni apžvalgininkai, turintys šimtus apžvalgų, dažniau rašo neigiamas nei teigiamas apžvalgas. Tačiau konkretaus apžvalgininko apžvalga nėra identifikuojama unikaliu kodu, todėl jos negalima susieti su apžvalgų rinkiniu. Yra 30 apžvalgininkų, turinčių 100 ar daugiau apžvalgų, tačiau sunku suprasti, kaip tai gali padėti rekomendacijų modeliui.
- `Reviewer_Nationality`
  - Kai kurie žmonės gali manyti, kad tam tikros tautybės yra labiau linkusios pateikti teigiamą ar neigiamą apžvalgą dėl nacionalinio polinkio. Būkite atsargūs, kurdami tokius anekdotinius požiūrius į savo modelius. Tai yra nacionaliniai (ir kartais rasiniai) stereotipai, o kiekvienas apžvalgininkas buvo individas, kuris rašė apžvalgą remdamasis savo patirtimi. Ji galėjo būti filtruota per daugelį lęšių, tokių kaip ankstesni viešbučių apsilankymai, kelionės atstumas ir asmeninis temperamentas. Manyti, kad jų tautybė buvo apžvalgos įvertinimo priežastis, yra sunku pateisinti.

##### Pavyzdžiai

| Vidutinis įvertinimas | Bendras apžvalgų skaičius | Apžvalgininko įvertinimas | Neigiama <br />Apžvalga                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Teigiama apžvalga                 | Žymos                                                                                      |
| --------------------- | ------------------------ | ------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------------------------------------------------------------------------------------- |
| 7.8                  | 1945                    | 2.5                       | Šiuo metu tai nėra viešbutis, o statybvietė. Buvau terorizuojamas nuo ankstyvo ryto ir visą dieną nepriimtinu statybų triukšmu, ilsėdamasis po ilgos kelionės ir dirbdamas kambaryje. Žmonės dirbo visą dieną, pvz., su gręžtuvais gretimuose kambariuose. Paprašiau pakeisti kambarį, bet tylus kambarys nebuvo prieinamas. Dar blogiau, man buvo per daug apmokestinta. Išsiregistravau vakare, nes turėjau ankstyvą skrydį, ir gavau tinkamą sąskaitą. Po dienos viešbutis be mano sutikimo padarė dar vieną mokestį, viršijantį užsakymo kainą. Tai siaubinga vieta. Nesikankinkite užsisakydami čia. | Nieko. Siaubinga vieta. Venkite. | Verslo kelionė. Pora. Standartinis dvivietis kambarys. Buvo 2 naktis. |

Kaip matote, šis svečias neturėjo malonaus apsilankymo šiame viešbutyje. Viešbutis turi gerą vidutinį įvertinimą - 7.8 ir 1945 apžvalgas, tačiau šis apžvalgininkas suteikė jam 2.5 ir parašė 115 žodžių apie tai, kaip neigiamas buvo jų apsilankymas. Jei jie nieko neparašė teigiamų apžvalgų stulpelyje, galite manyti, kad nebuvo nieko teigiamo, tačiau jie parašė 7 įspėjamuosius žodžius. Jei tiesiog skaičiuotume žodžius, o ne jų prasmę ar sentimentus, galėtume turėti iškreiptą apžvalgininko ketinimų vaizdą. Keista, kad jų įvertinimas 2.5 yra painus, nes jei viešnagė buvo tokia bloga, kodėl suteikti bet kokius taškus? Tyrinėdami duomenų rinkinį atidžiai, pamatysite, kad mažiausias galimas įvertinimas yra 2.5, o ne 0. Didžiausias galimas įvertinimas yra 10.

##### Žymos

Kaip minėta aukščiau, iš pirmo žvilgsnio atrodo, kad idėja naudoti `Tags` duomenų kategorijoms skirstyti yra prasminga. Deja, šios žymos nėra standartizuotos, o tai reiškia, kad tam tikrame viešbutyje pasirinkimai gali būti *Vienvietis kambarys*, *Dviejų lovų kambarys* ir *Dvivietis kambarys*, tačiau kitame viešbutyje jie yra *Deluxe vienvietis kambarys*, *Klasikinis karalienės kambarys* ir *Executive karaliaus kambarys*. Tai gali būti tie patys dalykai, tačiau yra tiek daug variacijų, kad pasirinkimas tampa:

1. Bandymas pakeisti visus terminus į vieną standartą, kuris yra labai sudėtingas, nes neaišku, koks būtų konversijos kelias kiekvienu atveju (pvz., *Klasikinis vienvietis kambarys* atitinka *Vienvietį kambarį*, tačiau *Superior karalienės kambarys su kiemo sodu arba miesto vaizdu* yra daug sunkiau priskirti)

1. Galime taikyti NLP metodą ir matuoti tam tikrų terminų, pvz., *Vienas*, *Verslo keliautojas* arba *Šeima su mažais vaikais*, dažnį, kaip jie taikomi kiekvienam viešbučiui, ir įtraukti tai į rekomendaciją  

Žymos paprastai (bet ne visada) yra vienas laukas, kuriame yra 5–6 kableliais atskirtos reikšmės, atitinkančios *Kelionės tipą*, *Svečių tipą*, *Kambario tipą*, *Naktų skaičių* ir *Įrenginį, kuriuo pateikta apžvalga*. Tačiau kadangi kai kurie apžvalgininkai neužpildo kiekvieno lauko (jie gali palikti vieną tuščią), reikšmės ne visada yra ta pačia tvarka.

Pavyzdžiui, paimkime *Grupės tipą*. Šiame lauke `Tags` stulpelyje yra 1025 unikalios galimybės, ir, deja, tik kai kurios iš jų nurodo grupę (kai kurios yra kambario tipas ir pan.). Jei filtruojate tik tas, kurios mini šeimą, rezultatai apima daugybę *Šeimos kambario* tipo rezultatų. Jei įtraukiate terminą *su*, t. y. skaičiuojate *Šeima su* reikšmes, rezultatai yra geresni, su daugiau nei 80 000 iš 515 000 rezultatų, kuriuose yra frazė "Šeima su mažais vaikais" arba "Šeima su vyresniais vaikais".

Tai reiškia, kad žymų stulpelis nėra visiškai nenaudingas, tačiau reikės šiek tiek darbo, kad jis būtų naudingas.

##### Vidutinis viešbučio įvertinimas

Duomenų rinkinyje yra keletas keistumų ar neatitikimų, kurių negaliu paaiškinti, tačiau jie čia iliustruojami, kad būtumėte informuoti apie juos, kai kuriate savo modelius. Jei juos išsiaiškinsite, praneškite mums diskusijų skyriuje!

Duomenų rinkinyje yra šie stulpeliai, susiję su vidutiniu įvertinimu ir apžvalgų skaičiumi: 

1. Hotel_Name
2. Additional_Number_of_Scoring
3. Average_Score
4. Total_Number_of_Reviews
5. Reviewer_Score  

Viešbutis, turintis daugiausiai apžvalgų šiame duomenų rinkinyje, yra *Britannia International Hotel Canary Wharf* su 4789 apžvalgomis iš 515 000. Tačiau jei pažvelgsime į `Total_Number_of_Reviews` reikšmę šiam viešbučiui, ji yra 9086. Galite manyti, kad yra daug daugiau įvertinimų be apžvalgų, todėl galbūt turėtume pridėti `Additional_Number_of_Scoring` stulpelio reikšmę. Ta reikšmė yra 2682, ir pridėjus ją prie 4789 gauname 7471, kuris vis dar yra 1615 mažesnis nei `Total_Number_of_Reviews`. 

Jei paimsite `Average_Score` stulpelį, galite manyti, kad tai yra vidurkis apžvalgų, esančių duomenų rinkinyje, tačiau Kaggle aprašymas yra "*Vidutinis viešbučio įvertinimas,
Atsižvelgiant į galimybę, kad šis viešbutis gali būti išskirtinis atvejis, ir kad galbūt dauguma reikšmių sutampa (bet kai kurios dėl tam tikrų priežasčių nesutampa), mes parašysime trumpą programą, kad ištirtume duomenų rinkinio reikšmes ir nustatytume teisingą jų naudojimą (arba nenaudojimą).

> 🚨 Pastaba apie atsargumą
>
> Dirbdami su šiuo duomenų rinkiniu, rašysite kodą, kuris apskaičiuoja kažką iš teksto, nereikalaujant, kad patys skaitytumėte ar analizuotumėte tekstą. Tai yra NLP esmė – interpretuoti prasmę ar nuotaiką, nereikalaujant žmogaus įsikišimo. Tačiau gali būti, kad perskaitysite kai kurias neigiamas apžvalgas. Raginčiau to nedaryti, nes jums to nereikia. Kai kurios iš jų yra kvailos arba nesvarbios neigiamos viešbučių apžvalgos, tokios kaip „Oras nebuvo geras“, kas yra už viešbučio ar bet kieno kontrolės ribų. Tačiau kai kurios apžvalgos turi ir tamsiąją pusę. Kartais neigiamos apžvalgos yra rasistinės, seksistinės ar diskriminuojančios pagal amžių. Tai yra apmaudu, bet tikėtina, kai duomenų rinkinys surinktas iš viešos svetainės. Kai kurie apžvalgininkai palieka apžvalgas, kurios gali būti nemalonios, nepatogios ar sukelti neigiamas emocijas. Geriau leisti kodui įvertinti nuotaiką, nei skaityti jas pačiam ir jaustis blogai. Vis dėlto, tokių apžvalgų yra mažuma, bet jos vis tiek egzistuoja.

## Užduotis – Duomenų tyrimas
### Duomenų įkėlimas

Pakanka vizualiai apžiūrėti duomenis, dabar parašysite kodą ir gausite atsakymus! Šiame skyriuje naudojama pandas biblioteka. Pirmoji jūsų užduotis – užtikrinti, kad galite įkelti ir perskaityti CSV duomenis. Pandas biblioteka turi greitą CSV įkėlimo funkciją, o rezultatas patalpinamas į duomenų rėmelį, kaip ir ankstesnėse pamokose. CSV, kurį įkeliame, turi daugiau nei pusę milijono eilučių, bet tik 17 stulpelių. Pandas suteikia daug galingų būdų sąveikauti su duomenų rėmeliu, įskaitant galimybę atlikti operacijas kiekvienoje eilutėje.

Nuo šios pamokos dalies bus pateikiami kodo fragmentai, paaiškinimai apie kodą ir diskusijos apie tai, ką reiškia rezultatai. Naudokite pridėtą _notebook.ipynb_ savo kodui.

Pradėkime nuo duomenų failo įkėlimo, kurį naudosite:

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

Kai duomenys įkelti, galime atlikti tam tikras operacijas su jais. Laikykite šį kodą savo programos viršuje kitai daliai.

## Duomenų tyrimas

Šiuo atveju duomenys jau yra *švarūs*, tai reiškia, kad jie paruošti darbui ir neturi simbolių kitomis kalbomis, kurie galėtų sutrikdyti algoritmus, tikinčius, kad yra tik angliški simboliai.

✅ Gali tekti dirbti su duomenimis, kuriems reikalingas pradinis apdorojimas prieš taikant NLP technikas, bet ne šį kartą. Jei reikėtų, kaip tvarkytumėte ne angliškus simbolius?

Skirkite akimirką, kad įsitikintumėte, jog įkėlus duomenis galite juos tyrinėti naudodami kodą. Labai lengva norėti susitelkti ties `Negative_Review` ir `Positive_Review` stulpeliais. Jie užpildyti natūraliu tekstu, kurį jūsų NLP algoritmai gali apdoroti. Bet palaukite! Prieš pasinerdami į NLP ir nuotaikų analizę, turėtumėte sekti žemiau pateiktą kodą, kad įsitikintumėte, ar duomenų rinkinyje pateiktos reikšmės atitinka reikšmes, kurias apskaičiuojate naudodami pandas.

## Duomenų rėmelio operacijos

Pirmoji užduotis šioje pamokoje yra patikrinti, ar šie teiginiai yra teisingi, parašant kodą, kuris tiria duomenų rėmelį (jo nepakeičiant).

> Kaip ir daugelis programavimo užduočių, yra keletas būdų tai atlikti, bet geras patarimas yra daryti tai paprasčiausiu, lengviausiu būdu, ypač jei tai bus lengviau suprasti, kai grįšite prie šio kodo ateityje. Dirbant su duomenų rėmeliais, yra išsamus API, kuris dažnai turės būdą efektyviai atlikti tai, ko norite.

Traktuokite šiuos klausimus kaip programavimo užduotis ir pabandykite atsakyti į juos, nesinaudodami sprendimu.

1. Išspausdinkite duomenų rėmelio *formą* (forma – tai eilučių ir stulpelių skaičius)
2. Apskaičiuokite apžvalgininkų tautybių dažnio skaičiavimą:
   1. Kiek skirtingų reikšmių yra stulpelyje `Reviewer_Nationality` ir kokios jos?
   2. Kokia apžvalgininkų tautybė yra dažniausia duomenų rinkinyje (išspausdinkite šalį ir apžvalgų skaičių)?
   3. Kokios yra kitos 10 dažniausiai pasitaikančių tautybių ir jų dažnio skaičiavimas?
3. Koks viešbutis buvo dažniausiai apžvelgtas kiekvienos iš 10 dažniausiai apžvelgiančių tautybių?
4. Kiek apžvalgų yra kiekvienam viešbučiui (viešbučių apžvalgų dažnio skaičiavimas) duomenų rinkinyje?
5. Nors duomenų rinkinyje yra stulpelis `Average_Score` kiekvienam viešbučiui, taip pat galite apskaičiuoti vidutinį balą (gaunant visų apžvalgininkų balų vidurkį duomenų rinkinyje kiekvienam viešbučiui). Pridėkite naują stulpelį prie savo duomenų rėmelio su stulpelio antrašte `Calc_Average_Score`, kuriame yra apskaičiuotas vidurkis.
6. Ar yra viešbučių, kurių `Average_Score` ir `Calc_Average_Score` reikšmės (suapvalintos iki 1 dešimtainio skaičiaus) yra vienodos?
   1. Pabandykite parašyti Python funkciją, kuri priima Series (eilutę) kaip argumentą ir palygina reikšmes, išspausdindama pranešimą, kai reikšmės nesutampa. Tada naudokite `.apply()` metodą, kad apdorotumėte kiekvieną eilutę su funkcija.
7. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Negative_Review` reikšmes „No Negative“
8. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Positive_Review` reikšmes „No Positive“
9. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Positive_Review` reikšmes „No Positive“ **ir** stulpelio `Negative_Review` reikšmes „No Negative“

### Kodo atsakymai

1. Išspausdinkite duomenų rėmelio *formą* (forma – tai eilučių ir stulpelių skaičius)

   ```python
   print("The shape of the data (rows, cols) is " + str(df.shape))
   > The shape of the data (rows, cols) is (515738, 17)
   ```

2. Apskaičiuokite apžvalgininkų tautybių dažnio skaičiavimą:

   1. Kiek skirtingų reikšmių yra stulpelyje `Reviewer_Nationality` ir kokios jos?
   2. Kokia apžvalgininkų tautybė yra dažniausia duomenų rinkinyje (išspausdinkite šalį ir apžvalgų skaičių)?

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

   3. Kokios yra kitos 10 dažniausiai pasitaikančių tautybių ir jų dažnio skaičiavimas?

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

3. Koks viešbutis buvo dažniausiai apžvelgtas kiekvienos iš 10 dažniausiai apžvelgiančių tautybių?

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

4. Kiek apžvalgų yra kiekvienam viešbučiui (viešbučių apžvalgų dažnio skaičiavimas) duomenų rinkinyje?

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
   
   Galite pastebėti, kad *duomenų rinkinyje suskaičiuoti* rezultatai nesutampa su `Total_Number_of_Reviews` reikšme. Neaišku, ar ši reikšmė duomenų rinkinyje atspindėjo bendrą viešbučio apžvalgų skaičių, bet ne visos buvo surinktos, ar tai buvo kitas skaičiavimas. `Total_Number_of_Reviews` nėra naudojamas modelyje dėl šio neaiškumo.

5. Nors duomenų rinkinyje yra stulpelis `Average_Score` kiekvienam viešbučiui, taip pat galite apskaičiuoti vidutinį balą (gaunant visų apžvalgininkų balų vidurkį duomenų rinkinyje kiekvienam viešbučiui). Pridėkite naują stulpelį prie savo duomenų rėmelio su stulpelio antrašte `Calc_Average_Score`, kuriame yra apskaičiuotas vidurkis. Išspausdinkite stulpelius `Hotel_Name`, `Average_Score` ir `Calc_Average_Score`.

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

   Galite taip pat stebėtis `Average_Score` reikšme ir kodėl ji kartais skiriasi nuo apskaičiuoto vidutinio balo. Kadangi negalime žinoti, kodėl kai kurios reikšmės sutampa, o kitos turi skirtumą, šiuo atveju saugiausia naudoti apžvalgų balus, kuriuos turime, kad patys apskaičiuotume vidurkį. Vis dėlto, skirtumai paprastai yra labai maži, štai viešbučiai su didžiausiu nukrypimu nuo duomenų rinkinio vidurkio ir apskaičiuoto vidurkio:

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

   Kadangi tik 1 viešbutis turi balų skirtumą, didesnį nei 1, tikriausiai galime ignoruoti skirtumą ir naudoti apskaičiuotą vidutinį balą.

6. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Negative_Review` reikšmes „No Negative“

7. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Positive_Review` reikšmes „No Positive“

8. Apskaičiuokite ir išspausdinkite, kiek eilučių turi stulpelio `Positive_Review` reikšmes „No Positive“ **ir** stulpelio `Negative_Review` reikšmes „No Negative“

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

## Kitas būdas

Kitas būdas skaičiuoti elementus be Lambdas ir naudoti sumą eilučių skaičiavimui:

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

   Galbūt pastebėjote, kad yra 127 eilutės, kurios turi tiek „No Negative“, tiek „No Positive“ reikšmes stulpeliuose `Negative_Review` ir `Positive_Review` atitinkamai. Tai reiškia, kad apžvalgininkas suteikė viešbučiui skaitinį balą, bet atsisakė rašyti tiek teigiamą, tiek neigiamą apžvalgą. Laimei, tai yra nedidelis eilučių skaičius (127 iš 515738, arba 0,02%), todėl tikriausiai tai nesukreips mūsų modelio ar rezultatų į tam tikrą kryptį, bet galbūt nesitikėjote, kad apžvalgų duomenų rinkinyje bus eilučių be apžvalgų, todėl verta tyrinėti duomenis, kad atrastumėte tokias eilutes.

Dabar, kai ištyrėte duomenų rinkinį, kitoje pamokoje filtruosite duomenis ir pridėsite nuotaikų analizę.

---
## 🚀Iššūkis

Ši pamoka parodo, kaip matėme ankstesnėse pamokose, kaip kritiškai svarbu suprasti savo duomenis ir jų ypatumus prieš atliekant operacijas su jais. Teksto pagrindu sukurti duomenys, ypač, reikalauja atidaus nagrinėjimo. Ištyrinėkite įvairius tekstu gausius duomenų rinkinius ir pažiūrėkite, ar galite atrasti sritis, kurios galėtų įvesti šališkumą ar iškreiptą nuotaiką į modelį.

## [Po paskaitos testas](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/38/)

## Apžvalga ir savarankiškas mokymasis

Pasinaudokite [šiuo NLP mokymosi keliu](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/?WT.mc_id=academic-77952-leestott), kad atrastumėte įrankius, kuriuos galite išbandyti kurdami kalbos ir teksto pagrindu sukurtus modelius.

## Užduotis 

[NLTK](assignment.md)

---

**Atsakomybės apribojimas**:  
Šis dokumentas buvo išverstas naudojant AI vertimo paslaugą [Co-op Translator](https://github.com/Azure/co-op-translator). Nors siekiame tikslumo, prašome atkreipti dėmesį, kad automatiniai vertimai gali turėti klaidų ar netikslumų. Originalus dokumentas jo gimtąja kalba turėtų būti laikomas autoritetingu šaltiniu. Kritinei informacijai rekomenduojama naudoti profesionalų žmogaus vertimą. Mes neprisiimame atsakomybės už nesusipratimus ar klaidingus interpretavimus, atsiradusius dėl šio vertimo naudojimo.