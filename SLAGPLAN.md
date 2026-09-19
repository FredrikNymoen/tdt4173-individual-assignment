# Slagplan

## Status etter gjennomgang

`readme.txt`, alle celler og lagrede tekstresultater i de tre notebookene, samt alle åtte CSV-filer er gjennomgått. TODO-cellene i ex1 og svarcellene i ex2a er uutfylte. `feature_engineering` i ex2b bruker fortsatt bare den medfølgende `preprocess`.

Dette dokumentet planlegger gjennomføringen. Modellene er ikke trent eller testet på nytt, og originalfilene er ikke endret.

| Datasett | Treningsrader | Testrader | Input-kolonner | Klassefordeling i trening |
| --- | ---: | ---: | ---: | --- |
| ex1 | 8 800 | 2 200 | 28 | 0: 4 134, 1: 4 666 |
| ex2 | 712 | 179 | 8 | 0: 444, 1: 268 |

Ex1 har ingen header. Ex2 har header og navn med komma i siterte felt, så det må leses med en ordentlig CSV-parser. Manglende verdier i ex2: alder 140/37 i trening/test og ombordstigningshavn 2/0.

## 1. Klargjør kjørbart miljø

- Finn en fungerende Python-installasjon og Jupyter-kernel. Den innledende kommandosjekken bekreftet ikke en brukbar Python-installasjon.
- Kontroller nødvendige pakker: pandas, numpy, scikit-learn, xgboost, seaborn, matplotlib og Jupyter. Opprett prosjektmiljø ved behov, og noter de faktiske versjonene som brukes.
- Kontroller innlesing, radantall, feature-antall og samsvar mellom input og etiketter. Bruk endimensjonal y ved modelltrening.
- Notebook-metadata viser eldre Python-versjoner (3.8.3 og 3.9.21). Dette er historikk, ikke et eksplisitt versjonskrav. Verifiser kompatibilitet i det valgte miljøet.

Ferdig når alle importer og datafiler kan leses fra prosjektmappen.

## 2. Løs ex1: standardmodell og tuning

1. Fyll første TODO med standard `XGBClassifier`, trening og `y_pred_default`.
2. Sett opp `GridSearchCV` på treningsdata med `scoring='f1'` og stratifikasjon, for eksempel fem fold med stokking og fast seed.
3. Start med alle 27 kombinasjoner i oppgavens grid: `n_estimators=[100, 200, 300]`, `max_depth=[3, 6, 9]` og `learning_rate=[0.01, 0.1, 0.2]`. Fem fold gir 135 treningskjøringer pluss refit.
4. Bruk den refittede `best_estimator_` til `y_pred_tuned`. Vis beste parametere og beste gjennomsnittlige validerings-F1.
5. Når modellvalget er ferdig, kjør den eksisterende testevalueringen av begge modellene. Registrer alle fire mål og F1-differansen.

Ferdig når standardmodellens F1 > 0,65 og den tunede modellens F1 er større enn både 0,65 og standardmodellens F1. Hvis kravet ikke nås, dokumenter resultatet uten å velge nye parametere etter testscore.

## 3. Løs ex2a: besvar alle fem spørsmål

Analyser `ex2_train.csv` sammen med `ex2_class_train.csv`. Hvert svar skal ha en kort konklusjon og en tabell eller figur som dokumenterer den. Vis antall passasjerer og overlevelsesandel per gruppe.

| Spørsmål | Analyse | Mulig kobling til feature engineering |
| --- | --- | --- |
| 1. Overlevde førsteklassepassasjerer oftere? | Sammenlign overlevelsesandel for hver `Pclass`. | Behold klasse som forklaringsvariabel. |
| 2. Henger overlevelse sammen med `Embarked`? | Sammenlign C, Q og S. Vis manglende verdier eksplisitt. | Robust kategorikoding og imputering. |
| 3. Hvordan henger alder sammen med overlevelse? | Bruk tydelig definerte aldersgrupper og suppler med fordeling etter overlevelse. Oppgi hvor mange som mangler alder. | Vurder barneindikator, aldersgrupper og indikator for manglende alder. |
| 4. Henger familiestørrelse sammen med overlevelse? | Definer `FamilySize = SibSp + Parch + 1`. Sammenlign alene, små og store familier med oppgitte grenser. | `FamilySize`, `IsAlone` og eventuelt familiegrupper. |
| 5. Henger titler sammen med overlevelse? | Trekk tittel fra `Name`, og vis andel og antall per tittel. Vær forsiktig med sjeldne titler. | `Title` med dokumentert samling av sjeldne kategorier. |

Dette er analyseforslag, ikke ferdige konklusjoner. Svarene må følge de beregnede resultatene. Unngå årsakspåstander og bastante slutninger om grupper med svært få personer.

Ferdig når alle fem spørsmål har etterprøvbare svar. Det formelle minimumskravet er tre korrekte svar.

## 4. Løs ex2b: mål effekten av feature engineering

1. Etabler baseline med oppgavens preprocessing og uendrede Random Forest-parametere. Lagre de fem målene før variabler og modell overskrives.
2. Velg et lite antall feature-forslag som støttes av ex2a. Start med `Title`, `FamilySize` og `IsAlone`. Vurder aldersfeatures dersom analysen begrunner dem.
3. Implementer konsistent imputering og kategorikoding med parametere lært på treningsdata. Bevar `feature_engineering(data_in)` og dokumenter nødvendige hjelpeobjekter i notebooken.
4. Kontroller at funksjonen bevarer rader og indeks, ikke muterer input og gir samme numeriske kolonner i samme rekkefølge for trening og test. Kontroller manglende verdier, uendelige verdier og ukjente kategorier.
5. Sammenlign et begrenset antall begrunnede varianter med stratifikasjon på treningsdata. Lær imputering og kategorier på nytt innen hver fold. Hold Random Forest-parametrene faste.
6. Velg løsning ut fra valideringen og kjør sluttevaluering på testsettet. Vis baseline, ny verdi og differanse for hvert av de fem målene. Tell strenge forbedringer med uavrundede verdier.

De eksisterende lagrede utskriftene viser samme resultat for baseline og plassholderen:

| Mål | Lagret verdi, ikke verifisert på nytt |
| --- | ---: |
| Accuracy | 0,8101 |
| Precision | 0,7778 |
| Recall | 0,7568 |
| F1 | 0,7671 |
| AUC-ROC | 0,8736 |

Ferdig når den nye løsningen slår den faktisk kjørte baselinen på minst tre mål. Forbedring er ikke garantert før målingen er gjort.

## 5. Sluttkontroll og leveranse

- Kjør hver notebook fra ren kernel i rekkefølgen `ex1`, `ex2a`, `ex2b`, og lagre faktiske utdata.
- Kontroller at ingen påkrevde TODO-er eller svarplassholdere står igjen, og at kjøringen ikke krever skjult tilstand.
- Se over tabeller og figurer for leselige akser, gruppedefinisjoner og samsvar med svarteksten.
- Bekreft begge F1-kravene i ex1, dokumenterte svar i ex2a og minst tre forbedrede mål i ex2b.
- Kontroller at originaldata og oppgavetekst er bevart. Noter miljøversjoner og eventuelle begrensninger.
- Lever de tre ferdige notebookene med en kort, faktisk resultatoppsummering. Behold `AGENTS.md` og denne planen som arbeidsdokumentasjon.
