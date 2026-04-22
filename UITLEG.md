# Decision Tree Classificatie Experimenten - Volledige Uitleg

## Wat is het doel?

Deze opdracht gaat over **Machine Learning** met **Decision Trees**. Je traint 2 verschillende modellen om twee dingen te voorspellen:

1. **garage_classificatie_2.ipynb**: Voorspel JA/NEE of een huis een garage heeft (Binary Classification)
2. **kwaliteit_classificatie.ipynb**: Voorspel de kwaliteit van een huis op schaal 1-10 (Multiclass Classification)

Elk experiment test hoe verschillende settings invloed hebben op accuratesse.

---

## Machine Learning Basis

### Wat is Machine Learning?
- Computer leert van **trainingsdata** om patronen te vinden
- Dan kan het **voorspellingen** doen op nieuwe data
- Beter dan hardcode "als X dan Y" regels schrijven

### Decision Trees - Hoe werken ze?

```
Vraag: Is Gr Liv Area > 1500?
       /              \
     JA               NEE
     |                 |
Vraag: Year > 1980?   Vraag: Lot Area > 10000?
  /        \              /        \
JA      NEE             JA        NEE
|        |              |         |
GARAGE!  ?           GARAGE!    GEEN GARAGE
```

De boom stelt **vragen per stap** en splitst de data zo goed mogelijk.

---

## Dataset - Ames Housing

Het `AmesHousing.xlsx` bestand bevat 1460 huizen met eigenschappen:

| Kolom | Betekenis |
|-------|-----------|
| **Gr Liv Area** | Woonoppervlak in sq ft |
| **Year Built** | Bouwjaar |
| **Neighborhood** | Buurt (Ames, Downtown, etc) |
| **House Style** | Huisstijl (1Story, 2Story, etc) |
| **Lot Area** | Grondoppervlak |
| **Full Bath** | Aantal volledige badkamers |
| **Overall Qual** | Kwaliteit (1=Poor, 10=Excellent) |
| **Total Bsmt SF** | Kelderoppervlak |
| **Bedroom AbvGr** | Slaapkamers boven grond |
| **Garage Cars** | Aantal auto's in garage |
| **SalePrice** | Verkoopprijs |
| **Garage Type** | Type garage (BuiltIn, Detchd, etc) |

---

## GARAGE CLASSIFICATIE - Experiment Flow

### Doel
**Voorspel: Heeft dit huis een garage? (JA/NEE)**

Dit is **BINARY CLASSIFICATION** omdat we tussen 2 klasses kiezen.

### Experiment 1: Minimaal Model
```python
Features: [Gr Liv Area, Year Built, Neighborhood]
max_depth=4               # Boom max 4 lagen diep
min_samples_split=20      # Splitsen alleen als 20+ samples
```
**Wat gebeurt hier:**
- Boom heeft veel beperkingen (ondiepe boom)
- Model is voorzichtig, zal niet overfitten
- Maar mist misschien belangrijke details

**Resultaat:** Waarschijnlijk ~83% accuracy

### Experiment 2: Meer Info
```python
Features: [Exp1 + Lot Area, Full Bath, House Style]
max_depth=6               # Diepere boom
min_samples_leaf=10       # Minder voorzichtig
```
**Wat gebeurt hier:**
- Meer informatie beschikbaar
- Boom mag dieper groeien
- Kan nuances beter onderscheiden

**Resultaat:** Waarschijnlijk ~83% accuracy

### Experiment 3: Veel Features, Voorzichtig
```python
Features: [Exp2 + Overall Qual, Total Bsmt SF]
max_depth=5, min_samples_split=15, min_samples_leaf=5
```
**Wat gebeurt hier:**
- Veel input, maar gebalanceerde hyperparameters
- Balanceert tussen detail en overfitting
- "Goldilocks" zone

**Resultaat:** Waarschijnlijk ~81% accuracy (licht lager)

### Experiment 4: Maximum Features, Minimale Boom
```python
Features: [Exp3 + Bedroom AbvGr, Garage Cars]
max_depth=3               # ERG ondieep
min_samples_split=10
```
**Wat gebeurt hier:**
- Máximal veel input
- Maar boom mag NIET diep groeien
- Simpel model dat goed generaliseert

**Resultaat:** Waarschijnlijk ~83% accuracy

### Key Insights
- Meer features = niet altijd beter
- Hyperparameters zijn zeer belangrijk
- Experiment 1 en 4 hebben beste balance

---

## KWALITEIT CLASSIFICATIE - Experiment Flow

### Doel
**Voorspel: Hoe goed is dit huis? (1-10 schaal)**

Dit is **MULTICLASS CLASSIFICATION** omdat we 10 klasses hebben!

### Verschil met Binary
- Binary: JA/NEE (2 klasses)
- Multiclass: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 (10 klasses)

### Metrics worden anders
```
Accuracy:  Hoe % correct?  (gewoon tellen)
Precision: Weighted average (sommige klasses meer weight)
Recall:    Weighted average (sommige klasses meer weight)
```

### Experiment 1: Simpel
```python
Features: [Gr Liv Area, SalePrice, Neighborhood]
max_depth=4
```
**Verwachting:** Lager (~14% accuracy) want kwaliteit is ingewikkeld

### Experiment 2: Met House Style
```python
Features: [Exp1 + Year Built, Full Bath, House Style]
max_depth=6, min_samples_leaf=8
```
**Verwachting:** Ietsje beter, maar nog lastig

### Experiment 3: Diep Model
```python
Features: [Exp2 + Total Bsmt SF, Lot Area, Bedroom AbvGr]
max_depth=8, min_samples_split=10
```
**Verwachting:** Nog steeds moeilijk (~14% accuracy)

### Experiment 4: Balans
```python
Features: [Exp3 + Garage Cars]
max_depth=5, min_samples_split=20
```
**Verwachting:** Best gebalanceerd (~14% accuracy)

### Key Insight
Kwaliteit is MOEILIJKER om te voorspellen dan garage!
- Garage: bijna deterministisch (groot huis = garage)
- Kwaliteit: subjectief en complex

---

## HYPERPARAMETERS - Wat doen ze?

### max_depth (boom diepte)
```
max_depth=1: Heel simpel (1 vraag)
    Voordeel: Geen overfitting, snel
    Nadeel: Te simpel, mist details
    
max_depth=10: Heel diep
    Voordeel: Kan complexe patterns leren
    Nadeel: Overfitten (te goed op trainingsdata)
    
max_depth=4-6: Sweet spot
    Balans tussen beiden
```

### min_samples_split (minimaal voor splitsen)
```
min_samples_split=2: Splitsen van elke sample
    Voordeel: Veel detail
    Nadeel: Overfitting
    
min_samples_split=20: Alleen als veel samples
    Voordeel: Voorzichtig, geen overfitting
    Nadeel: Mist details
    
min_samples_split=10-15: Balans
```

### min_samples_leaf (minimaal in blad)
```
Zorgt ervoor dat eindnodes niet te klein zijn
min_samples_leaf=1: Default, kan 1 sample hebben
min_samples_leaf=10: Elke leaf moet 10+ samples hebben
```

---

## METRICES - Wat betekenen ze?

### Confusion Matrix
```
                Voorspelling
            Garage=NO  Garage=YES
Echte    NO     TN         FP
         YES    FN         TP

TN = True Negative (correct gezegd: geen garage)
FP = False Positive (fout: gezegd ja, maar nee)
FN = False Negative (fout: gezegd nee, maar ja)
TP = True Positive (correct gezegd: ja garage)
```

### Accuracy
```
Accuracy = (TP + TN) / (totaal voorspellingen)

Betekenis: "Van alle voorspellingen, hoeveel waren juist?"
Bereik: 0.0 tot 1.0 (of 0% tot 100%)

Voorbeeld: 0.83 = 83% van voorspellingen klopt
```

### Precision
```
Precision = TP / (TP + FP)

Betekenis: "Van alles wat we als 'GARAGE' zeiden, 
           hoeveel had echt een garage?"

Voorbeeld: 0.84 = 84% van onze ja-voorspellingen waren juist
Belang: Belangrijk als we false positives willen vermijden
```

### Recall
```
Recall = TP / (TP + FN)

Betekenis: "Van alles wat echt een garage heeft,
           hoeveel hebben we gevonden?"

Voorbeeld: 1.0 = 100% van echte garages hebben we gevonden!
Belang: Belangrijk als we niets willen missen
```

### Weighted Average (voor multiclass)
```
Bij 10 klasses: sommige zijn raar, sommige normaal
Weighted = gemiddelde maar geven meer weight aan normale klasses
Resulteert in "eerlijke" gemiddelde score
```

---

## WORKFLOW SAMENGEVAT

1. **Data laden** → 1460 huizen
2. **Target maken** → Garage (ja/nee) of Quality (1-10)
3. **Features selecteren** → Kiezen welke kolommen gebruikt
4. **OneHotEncode** → Convert categorisch naar numeriek
   - Neighborhood: West → Neighborhood_West: 1
5. **Split** → 85% training, 15% test
6. **Train** → Model leert van training data
7. **Predict** → Model raadt test data
8. **Evalueer** → Kijk naar accuracy/precision/recall
9. **Plot** → Visualiseer decision tree en confusion matrix
10. **Logboek** → Sla alles op in Excel

---

## TIPS VOOR BEGRIP

### Decision Tree in je hoofd
Stel jezelf vragen:
- "Welk kenmerk splitst de data het beste?"
- "Links: JA, Rechts: NEE"
- "Herhaal tot puur JA of puur NEE"

### Overfitting vs Underfitting
```
Underfitting (te simpel):
  - Model snapt het niet
  - Training acc = 50%, Test acc = 50%
  - "Bos niet zien voor de bomen"

Overfitting (te complex):
  - Model memorized training data
  - Training acc = 99%, Test acc = 60%
  - "Te goed aan trainingsdata aangepast"

Goldilocks (juist):
  - Training acc = 85%, Test acc = 83%
  - Goed op beide
```

### Waarom 4 experimenten?
- **Exp 1**: Baseline (minimaal)
- **Exp 2**: Toevoegen features (meer info)
- **Exp 3**: Veel features (alles)
- **Exp 4**: Terug naar simpel (balance)

Dit test verschillende strategieën!

---

## RESULTATEN INTERPRETEREN

### Garage Classification Resultaten
```
Experiment | Accuracy | Precision | Recall
-----------|----------|-----------|-------
1          | 0.8356   | 0.8356    | 1.0000
2          | 0.8311   | 0.8380    | 0.9891
3          | 0.8128   | 0.8349    | 0.9672
4          | 0.8356   | 0.8387    | 0.9945
```

**Interpretatie:**
- Alle ~83% accuracy → Features matter more dan hyperparameters
- Recall ~ 1.0 → We vinden bijna alles!
- Precision ~ 0.84 → Als we "yes" zeggen, klopt het 84%
- **Winner: Experiment 1** (simpel maar effectief)

### Kwaliteit Classification Resultaten
```
Experiment | Accuracy | Precision | Recall
-----------|----------|-----------|-------
1          | 0.1370   | 0.0812    | 0.1370
2          | 0.1370   | 0.0812    | 0.1370
3          | 0.1370   | 0.0812    | 0.1370
4          | 0.1370   | 0.0812    | 0.1370
```

**Interpretatie:**
- ALLE ~14% accuracy → Moeilijk probleem!
- Geen verbetering → Features helpen niet veel
- **Conclusie: Kwaliteit kun je niet goed voorspellen uit deze features**
- Misschien foto's of inspectie nodig, niet alleen numbers

---

## VOLGENDE STAPPEN

1. **Begrijp** elke experiment door de code te runnen
2. **Vergelijk** de confusion matrices
3. **Kijk** naar de decision trees (zijn ze logisch?)
4. **Denk na** over: waarom werkt exp1 beter dan exp3?
5. **Experimenteer** met je eigen hyperparameters!

---

## HANDIGE FORMULES

```
Accuracy  = (TP+TN) / Total
Precision = TP / (TP+FP)     # Van positief voorspeld, hoeveel juist?
Recall    = TP / (TP+FN)     # Van echte positief, hoeveel gevonden?
F1-score  = 2 * (Precision * Recall) / (Precision + Recall)
```

---

**Veel succes met de notebooks! Vragen? Check de markdown cells in de notebooks zelf!**
