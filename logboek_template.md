# Logboek Template - DEAI Classificatie

Gebruik 1 regel per run/experiment.

## Blad 1: Garage (binair)

| Run | Model | Target | Features (aantal + namen) | Categorische features | Encoding | Test size | Hyperparameters | Accuracy | Precision | Recall | F1 | Opmerking / conclusie |
|---|---|---|---|---|---|---|---|---:|---:|---:|---:|---|
| 1 | DecisionTreeClassifier | Garage | 3: Neighborhood, Gr Liv Area, Year Built | Neighborhood | One-hot | 0.20 | max_depth=5, min_samples_leaf=10 |  |  |  |  | Initiele run |
| 2 | DecisionTreeClassifier | Garage | 4: Neighborhood, Gr Liv Area, Year Built, Lot Area | Neighborhood | One-hot | 0.20 | max_depth=8, min_samples_leaf=6, min_samples_split=12 |  |  |  |  | Experiment 2 |
| 3 | DecisionTreeClassifier | Garage |  |  | One-hot | 0.20 |  |  |  |  |  | Experiment 3 |
| 4 | DecisionTreeClassifier | Garage |  |  | One-hot | 0.20 |  |  |  |  |  | Experiment 4 |

## Blad 2: Kwaliteit (multi-class)

| Run | Model | Target | Features (aantal + namen) | Categorische features | Encoding | Test size | Hyperparameters | Accuracy | Precision (weighted) | Recall (weighted) | F1 (weighted) | Opmerking / conclusie |
|---|---|---|---|---|---|---|---|---:|---:|---:|---:|---|
| 1 | DecisionTreeClassifier | Overall Qual | 3: Neighborhood, Gr Liv Area, Year Built | Neighborhood | One-hot | 0.20 | max_depth=6, min_samples_leaf=8 |  |  |  |  | Initiele run |
| 2 | DecisionTreeClassifier | Overall Qual | 4: Neighborhood, Gr Liv Area, Year Built, Total Bsmt SF | Neighborhood | One-hot | 0.20 | max_depth=10, min_samples_leaf=4, min_samples_split=12 |  |  |  |  | Experiment 2 |
| 3 | DecisionTreeClassifier | Overall Qual |  |  | One-hot | 0.20 |  |  |  |  |  | Experiment 3 |
| 4 | DecisionTreeClassifier | Overall Qual |  |  | One-hot | 0.20 |  |  |  |  |  | Experiment 4 |

## Blad 3: Inspiratiepad (voor stap 8)

| Van run | Observatie uit metrics | Nieuwe keuze (features/hyperparameters) | Waarom deze keuze? | Verbetering t.o.v. vorige run |
|---|---|---|---|---|
| 1 -> 2 |  |  |  |  |
| 2 -> 3 |  |  |  |  |
| 3 -> 4 |  |  |  |  |

## Korte invulregels

- Vul target en features exact in zoals in je notebook.
- Noem altijd welke feature(s) categorisch zijn.
- Schrijf hyperparameters volledig uit, met waarden.
- Noteer per run kort wat beter/slechter ging en wat je daarna verandert.
