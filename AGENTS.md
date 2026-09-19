# Arbeidsføringer for innleveringen

## Formål og kilder

Dette er en individuell maskinlæringsoppgave med tre obligatoriske deler. Les `readme.txt` og oppgaveteksten i den aktuelle notebooken før endringer. Notebookene er autoritative for beståttkravene. Gjennomføringen er beskrevet i `SLAGPLAN.md`.

Kommuniser med brukeren på norsk. Behold oppgaveteksten, filnavnene og de eksisterende variabelnavnene. Bruk gjerne engelsk i nye notebook-svar og kodekommentarer for å følge malen. Forklar metodevalg slik at studenten kan forstå og etterprøve arbeidet.

## Filer og beståttkrav

| Fil | Arbeid | Beståttkrav |
| --- | --- | --- |
| `ex1.ipynb` | Fyll TODO-cellene for standard XGBoost og hyperparametersøk. | Standardmodell: F1 > 0,65. Tunet modell: F1 > 0,65 og F1 > standardmodellens F1. |
| `ex2a.ipynb` | Besvar fem EDA-spørsmål med påstand og støttende statistikk eller figur. | Minst tre korrekte svar med dokumentasjon. Arbeidsmålet er alle fem. |
| `ex2b.ipynb` | Implementer `feature_engineering(data_in)` med utgangspunkt i EDA. | Forbedring i minst tre av fem mål: accuracy, precision, recall, F1 og AUC-ROC. |

Fullfør `ex2a.ipynb` før `ex2b.ipynb`. Alle tre deler må bestås.

## Data og innlesing

- Bevar alle originale CSV-filer uendret. Radrekkefølgen kobler input til klassetilhørighet. Ikke sorter eller filtrer X og y uavhengig.
- `ex1_train.csv` og `ex1_test.csv`: henholdsvis 8 800 og 2 200 rader, 28 numeriske kolonner uten header. Bruk `header=None`, også for de tilhørende klassefilene. Etikettene er 0 og 1, lagret som desimaltall.
- `ex2_train.csv` og `ex2_test.csv`: henholdsvis 712 og 179 rader med header. Kolonner: `Pclass`, `Name`, `Sex`, `Age`, `SibSp`, `Parch`, `Fare`, `Embarked`. Klassefilene har kolonnen `Survived`.
- Ved gjennomgang hadde ex1 ingen tomme eller ikke-endelige numeriske verdier. Ex2 manglet `Age` i 140 treningsrader og 37 testrader, samt `Embarked` i 2 treningsrader. Øvrige ex2-felt var utfylt.
- Kontroller form, etiketter og samsvar mellom X og y ved innlesing. Ikke tolk nullverdier som manglende data uten dokumentasjon.

## Metode og datalekkasje

- Bruk treningsdata til EDA, modellvalg, feature-valg og hyperparametersøk. Bruk kryssvalidering eller en separat valideringsdel fra treningssettet til sammenligninger under utvikling.
- Hold testetikettene utenfor utviklingen. Testsettet brukes til sluttmåling av de valgte løsningene, ikke til gjentatt tilpasning mot beståttgrensene.
- Lær medianer, typetall, kategorier og andre datadrevne transformasjoner bare fra treningsdata. I kryssvalidering må dette gjøres på treningsdelen i hver fold.
- Bruk samme lærte transformasjon på validering/test. Sørg for identiske feature-kolonner og kolonnerekkefølge, numeriske verdier og håndtering av ukjente kategorier.
- Bevar signaturen `feature_engineering(data_in)` og de eksisterende kallene. Eventuelle hjelpeobjekter må initialiseres eksplisitt fra treningsdata og fungere ved kjøring fra ren kernel. Unngå skjult tilstand som avhenger av hvilket datasett funksjonen kalles på først.
- Behold `RandomForestClassifier(n_estimators=100, random_state=42)` for både baseline og forbedret modell. Oppgave 3 skal vise effekten av feature engineering.
- Behold baseline-koden som sammenligningsgrunnlag. Den medfølgende `preprocess` beregner imputering separat for hvert datasett og lager dummyvariabler separat. Ta høyde for disse svakhetene i den nye løsningen, og forklar eventuelle nødvendige endringer i sammenligningsopplegget.
- I oppgave 1 skal prediksjonene hete `y_pred_default` og `y_pred_tuned`. Bruk den oppgitte parametergriden først og F1 som søkemål. Behold standardmodellens læringsparametere som standard.
- Bruk fast seed ved stokastiske valg og oppgi pakkeversjoner. Tilpass antall parallelle jobber til maskinen, særlig når både søk og modell kan bruke flere tråder.

## Notebook-kvalitet og kontroll

- Gjør målrettede endringer i TODO-/svarcellene og nødvendige hjelpeceller. Bevar oppgavetekst, struktur og eksisterende evaluering.
- Underbygg EDA-svar med beregnede resultater fra de leverte treningsdataene. Oppgi gruppestørrelser sammen med overlevelsesandeler. Skill mellom sammenheng og årsak, og kommenter små grupper.
- Beregn AUC-ROC med sannsynligheten for klasse 1. Sammenlign beståttkrav med uavrundede verdier. Likhet teller ikke som forbedring.
- `evaluate_result` bruker den globale variabelen `X_test_processed`. Sørg for at denne tilhører modellen som evalueres, og lagre baseline-resultatene før variabelen overskrives.
- Kjør hver notebook fra ren kernel, fra øverst til nederst, med prosjektmappen som arbeidsmappe. Ingen notebook skal kreve at en annen notebooks kernel allerede har kjørt.
- Lagrede notebook-utskrifter er historiske resultater, ikke bevis på at dagens kode virker. Rapporter bare resultater som faktisk er beregnet, og oppgi hva som ikke er kjørt.
- Prioriter full notebook-kjøring og enkle kontroller av data, features og mål fremfor et omfattende separat testoppsett.
- Ikke erklær oppgaven bestått før alle krav er bekreftet. Ved manglende forbedring: rapporter det ærlig og vurder videre arbeid med treningsbasert validering.
