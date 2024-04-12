# COVID-19 Analysis

### Case Study

Il committente richiede di avere un report su casi e vaccinazioni in diverse aree del mondo; a tal fine, richiede di utilizzare il dataset, curato da Our World in Data, all'indirizzo https://github.com/owid/covid-19-data/tree/master/public/data

#### Fase 1: Importo le librerie necessarie per l'analisi e creo una directory che permetta di accedere al file .csv del dataset indipendentemente dal sistema operativo utilizzato 
```python
import os
import pandas as pd
import matplotlib.pyplot as plt
import humanize

current_work_directory = os.getcwd()
covid_filepath = os.path.join(current_work_directory,
                             "owid-covid-data.csv")
with open(covid_filepath) as c:
    df_covid = pd.read_csv(c,low_memory=False)
    
df_covid
```

#| iso_code| continent| location| date| total_cases| new_cases| new_cases_smoothed| total_deaths| new_deaths| new_deaths_smoothed| ...| male_smokers| handwashing_facilities| hospital_beds_per_thousand| life_expectancy| life_expectancy| human_development_index| population| excess_mortality_cumulative_absolute| excess_mortality_cumulative| excess_mortality| excess_mortality_cumulative_per_million
-|---------|---------|---------|----|------------|----------|------------------|-------------|-----------|---------------------|---|-------------|----------------------|-------------------------|----------------|----------------|------------------------|-----------|---------------------------------|-------------------------|----------------|------------------------------
0|AFG|Asia|Afghanistan|2020-01-05|NaN|0.0|NaN|NaN|0.0|NaN|...|NaN|37.746|0.5|64.83|0.511|41128772.0|NaN|NaN|NaN|NaN
1|AFG|Asia|Afghanistan|2020-01-06|NaN|0.0|NaN|NaN|0.0|NaN|...|NaN|37.746|0.5|64.83|0.511|41128772.0|NaN|NaN|NaN|NaN
2|AFG|Asia|Afghanistan|2020-01-07|NaN|0.0|NaN|NaN|0.0|NaN|...|NaN|37.746|0.5|64.83|0.511|41128772.0|NaN|NaN|NaN|NaN
3|AFG|Asia|Afghanistan|2020-01-08|NaN|0.0|NaN|NaN|0.0|NaN|...|NaN|37.746|0.5|64.83|0.511|41128772.0|NaN|NaN|NaN|NaN
4|AFG|Asia|Afghanistan|2020-01-09|NaN|0.0|NaN|NaN|0.0|NaN|...|NaN|37.746|0.5|64.83|0.511|41128772.0|NaN|NaN|NaN|NaN
...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...
377002|ZWE|Africa|Zimbabwe|2024-01-31|266265.0|0.0|4.429|5737.0|0.0|0.0|...|30.7|36.791|1.7|61.49|0.571|16320539.0|NaN|NaN|NaN|NaN
377003|ZWE|Africa|Zimbabwe|2024-02-01|266265.0|0.0|4.429|5737.0|0.0|0.0|...|30.7|36.791|1.7|61.49|0.571|16320539.0|NaN|NaN|NaN|NaN
377004|ZWE|Africa|Zimbabwe|2024-02-02|266265.0|0.0|4.429|5737.0|0.0|0.0|...|30.7|36.791|1.7|61.49|0.571|16320539.0|NaN|NaN|NaN|NaN
377005|ZWE|Africa|Zimbabwe|2024-02-03|266265.0|0.0|4.429|5737.0|0.0|0.0|...|30.7|36.791|1.7|61.49|0.571|16320539.0|NaN|NaN|NaN|NaN
377006|ZWE|Africa|Zimbabwe|2024-02-04|266265.0|0.0|0.000|5737.0|0.0|0.0|...|30.7|36.791|1.7|61.49|0.571|16320539.0|NaN|NaN|NaN|NaN

#### Fase 2: Si richiede di verificare le dimensioni del dataset e le diciture presenti nell'intestazione

Effettuo una prima analisi esplorativa con .info(); Aggiungendo il parametro memory_usage="deep" restituisce esattamente la memoria fisica occupata dal dataset

```python
df_covid.info(memory_usage="deep")
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 377007 entries, 0 to 377006
Data columns (total 67 columns):
 #   Column                                      Non-Null Count   Dtype  
---  ------                                      --------------   -----  
 0   iso_code                                    377007 non-null  object 
 1   continent                                   358941 non-null  object 
 2   location                                    377007 non-null  object 
 3   date                                        377007 non-null  object 
 4   total_cases                                 338085 non-null  float64
 5   new_cases                                   366116 non-null  float64
 6   new_cases_smoothed                          364886 non-null  float64
 7   total_deaths                                316113 non-null  float64
 8   new_deaths                                  366124 non-null  float64
 9   new_deaths_smoothed                         364894 non-null  float64
 10  total_cases_per_million                     338085 non-null  float64
 11  new_cases_per_million                       366116 non-null  float64
 12  new_cases_smoothed_per_million              364886 non-null  float64
 13  total_deaths_per_million                    316113 non-null  float64
 14  new_deaths_per_million                      366124 non-null  float64
 15  new_deaths_smoothed_per_million             364894 non-null  float64
 16  reproduction_rate                           184817 non-null  float64
 17  icu_patients                                38433 non-null   float64
 18  icu_patients_per_million                    38433 non-null   float64
 19  hosp_patients                               39932 non-null   float64
 20  hosp_patients_per_million                   39932 non-null   float64
 21  weekly_icu_admissions                       10588 non-null   float64
 22  weekly_icu_admissions_per_million           10588 non-null   float64
 23  weekly_hosp_admissions                      23976 non-null   float64
 24  weekly_hosp_admissions_per_million          23976 non-null   float64
 25  total_tests                                 79387 non-null   float64
 26  new_tests                                   75403 non-null   float64
 27  total_tests_per_thousand                    79387 non-null   float64
 28  new_tests_per_thousand                      75403 non-null   float64
 29  new_tests_smoothed                          103965 non-null  float64
 30  new_tests_smoothed_per_thousand             103965 non-null  float64
 31  positive_rate                               95927 non-null   float64
 32  tests_per_case                              94348 non-null   float64
 33  tests_units                                 106788 non-null  object 
 34  total_vaccinations                          82679 non-null   float64
 35  people_vaccinated                           78631 non-null   float64
 36  people_fully_vaccinated                     75487 non-null   float64
 37  total_boosters                              50775 non-null   float64
 38  new_vaccinations                            68479 non-null   float64
 39  new_vaccinations_smoothed                   189150 non-null  float64
 40  total_vaccinations_per_hundred              82679 non-null   float64
 41  people_vaccinated_per_hundred               78631 non-null   float64
 42  people_fully_vaccinated_per_hundred         75487 non-null   float64
 43  total_boosters_per_hundred                  50775 non-null   float64
 44  new_vaccinations_smoothed_per_million       189150 non-null  float64
 45  new_people_vaccinated_smoothed              186642 non-null  float64
 46  new_people_vaccinated_smoothed_per_hundred  186642 non-null  float64
 47  stringency_index                            197292 non-null  float64
 48  population_density                          320354 non-null  float64
 49  median_age                                  297831 non-null  float64
 50  aged_65_older                               287530 non-null  float64
 51  aged_70_older                               294847 non-null  float64
 52  gdp_per_capita                              292006 non-null  float64
 53  extreme_poverty                             188233 non-null  float64
 54  cardiovasc_death_rate                       292577 non-null  float64
 55  diabetes_prevalence                         307623 non-null  float64
 56  female_smokers                              219613 non-null  float64
 57  male_smokers                                216629 non-null  float64
 58  handwashing_facilities                      143293 non-null  float64
 59  hospital_beds_per_thousand                  258405 non-null  float64
 60  life_expectancy                             347067 non-null  float64
 61  human_development_index                     283747 non-null  float64
 62  population                                  377007 non-null  float64
 63  excess_mortality_cumulative_absolute        13042 non-null   float64
 64  excess_mortality_cumulative                 13042 non-null   float64
 65  excess_mortality                            13042 non-null   float64
 66  excess_mortality_cumulative_per_million     13042 non-null   float64
dtypes: float64(62), object(5)
memory usage: 286.1 MB
```

Verifico l'elenco totale delle colonne del dataset con il loro rispettivo nome

```python
df_covid.columns
```

```
Index(['iso_code', 'continent', 'location', 'date', 'total_cases', 'new_cases',
'new_cases_smoothed', 'total_deaths', 'new_deaths',
'new_deaths_smoothed', 'total_cases_per_million',
'new_cases_per_million', 'new_cases_smoothed_per_million',
'total_deaths_per_million', 'new_deaths_per_million',
'new_deaths_smoothed_per_million', 'reproduction_rate', 'icu_patients',
'icu_patients_per_million', 'hosp_patients',
'hosp_patients_per_million', 'weekly_icu_admissions',
'weekly_icu_admissions_per_million', 'weekly_hosp_admissions',
'weekly_hosp_admissions_per_million', 'total_tests', 'new_tests',
'total_tests_per_thousand', 'new_tests_per_thousand',
'new_tests_smoothed', 'new_tests_smoothed_per_thousand',
'positive_rate', 'tests_per_case', 'tests_units', 'total_vaccinations',
'people_vaccinated', 'people_fully_vaccinated', 'total_boosters',
'new_vaccinations', 'new_vaccinations_smoothed',
'total_vaccinations_per_hundred', 'people_vaccinated_per_hundred',
'people_fully_vaccinated_per_hundred', 'total_boosters_per_hundred',
'new_vaccinations_smoothed_per_million',
'new_people_vaccinated_smoothed',
'new_people_vaccinated_smoothed_per_hundred', 'stringency_index',
'population_density', 'median_age', 'aged_65_older', 'aged_70_older',
'gdp_per_capita', 'extreme_poverty', 'cardiovasc_death_rate',
'diabetes_prevalence', 'female_smokers', 'male_smokers',
'handwashing_facilities', 'hospital_beds_per_thousand',
'life_expectancy', 'human_development_index', 'population',
'excess_mortality_cumulative_absolute', 'excess_mortality_cumulative',
'excess_mortality', 'excess_mortality_cumulative_per_million'],
dtype='object')
```
Si tratta di un dataset di notevoli dimensioni, soprattutto per quanto riguarda il numero dei fattori analizzabili. Per ottimizzare i processi, mantengo solo le colonne strettamente necessarie all'analisi richiesta

```python
df_covid = df_covid.loc[:, [
            "iso_code",
            "continent",
            "location",
            "date",
            "total_cases",
            "total_vaccinations"
            ]]

df_covid
```
#| iso_code | continent | location | date | total_cases | total_vaccinations|
-----------|-----------------|------|-----------------|------|----------------|--------------------|
0|AFG|Asia|Afghanistan|2020-01-05|NaN|NaN
0|AFG|Asia|Afghanistan|2020-01-06|NaN|NaN
0|AFG|Asia|Afghanistan|2020-01-07|NaN|NaN
0|AFG|Asia|Afghanistan|2020-01-08|NaN|NaN
0|AFG|Asia|Afghanistan|2020-01-09|NaN|NaN
...|...|...|...|...|...|...
377002|ZWE|Africa|Zimbabwe|2024-01-31|266265.0|NaN
377002|ZWE|Africa|Zimbabwe|2024-02-01|266265.0|NaN
377002|ZWE|Africa|Zimbabwe|2024-02-02|266265.0|NaN
377002|ZWE|Africa|Zimbabwe|2024-02-03|266265.0|NaN
377002|ZWE|Africa|Zimbabwe|2024-02-04|266265.0|NaN

#### Fase 3: Si chiede poi per ogni continente di trovare il numero di casi totali, escludendo eventuali locazioni che nel dataset non appartengono ad alcun continente

```python
# Passaggio 1: elimino i record che non appartengono ad alcun continente
df_covid_full_continent = df_covid.dropna(subset=["continent"])
# Passaggio 2: Raggruppo i dati rimanenti per continente; per avere riscontro dei casi totali effettuo la somma dei nuovi casi
# N.B. il dataset è soggetto ad aggiornamento quotidiano e il valore relativo ai casi totali è impostato in maniera incrementale, il che rende inopportuno scegliere tale parametro come riferimento per i casi totali
casi_totali = df_covid_full_continent.groupby("continent")["new_cases"].sum()
# Passaggio 3: imposto il result set come dataframe e rinomino la colonna "new cases" in "total cases"
df_casi_totali = pd.DataFrame(casi_totali)
df_casi_totali = df_casi_totali.rename(columns = {"new_cases":"total_cases"} )
df_casi_totali
```
Continent | total_cases |
-----------|-----------------|
Africa|13136517.0
Asia|301391174.0
Europe|252013370.0
North America|124526003.0
Oceania|14741735.0
South America|68752780.0

#### Fase 4: Sempre riguardo i casi di COVID totali, si chiede di sviluppare una funzione che prenda in input il dataset e due nomi di continenti, e che ne confronti i seguenti relativi descrittori statistici: valori minimo e massimo, media, e percentuale rispetto al numero dei casi totali nel mondo (calcolati anche sulle locazioni senza indicazione di continente)

```python
def confronto_casi_continenti(dataset):
    # passaggio 1 : filtra i due dataset per i continenti selezionati, generando due nuovi dataset
    continente1 = input("Qual è il primo continente che vuoi confrontare?: ").capitalize()
    continente2 = input("Qual è il secondo continente che vuoi confrontare?: ").capitalize()
    df_continente1 = dataset[dataset["continent"].str.capitalize() == continente1]
    df_continente2 = dataset[dataset["continent"].str.capitalize() == continente2]
    # passaggio 2: calcola la somma dei casi totali per ogni continente selezionato
    max_casi_per_stato_continente1 = df_continente1.groupby(["location", "continent"])["total_cases"].agg('max')
    casi_totali_continente1 = max_casi_per_stato_continente1.sum()
    max_casi_per_stato_continente2 = df_continente2.groupby(["location", "continent"])["total_cases"].agg('max')
    casi_totali_continente2 = max_casi_per_stato_continente2.sum()
    # passaggio 3: calcola la somma dei casi totali mondiali (includendo anche i casi in cui continent is null)
    casi_totali_ogni_stato = dataset.groupby(["location", "continent"])["total_cases"].agg('max')
    casi_totali_mondo = casi_totali_ogni_stato.sum()
    # passaggio 4: genera i descrittori statistici per continente 1
    valore_min_casi_continente1 = max_casi_per_stato_continente1.min()
    stato_valore_min_casi_continente1 = max_casi_per_stato_continente1.idxmin()[0]
    valore_max_casi_continente1 = max_casi_per_stato_continente1.max()
    stato_valore_max_casi_continente1 = max_casi_per_stato_continente1.idxmax()[0]
    media_casi_continente1 = max_casi_per_stato_continente1.mean()
    media_casi_continente1 = round(media_casi_continente1, 2)
    confronto_casi_mondiale_continente1 = (casi_totali_continente1 / casi_totali_mondo) * 100
    confronto_casi_mondiale_continente1 = round(confronto_casi_mondiale_continente1, 2)
     # passaggio 5: genera i descrittori statistici per continente 2
    valore_min_casi_continente2 = max_casi_per_stato_continente2.min()
    stato_valore_min_casi_continente2 = max_casi_per_stato_continente2.idxmin()[0]
    valore_max_casi_continente2 = max_casi_per_stato_continente2.max()
    stato_valore_max_casi_continente2 = max_casi_per_stato_continente2.idxmax()[0]
    media_casi_continente2 = max_casi_per_stato_continente2.mean()
    media_casi_continente2 = round(media_casi_continente2, 2)
    confronto_casi_mondiale_continente2 = (casi_totali_continente2 / casi_totali_mondo) * 100
    confronto_casi_mondiale_continente2 = round(confronto_casi_mondiale_continente2, 2)
   # passaggio 6: crea un dataframe riepilogativo per continente 1
    df_riepilogo_stat_continente1 = pd.DataFrame({'Continente': [continente1],
                                                  'Valore minimo di casi totali': [humanize.intcomma(valore_min_casi_continente1)],
                                                  'Stato col valore minimo di casi totali': [stato_valore_min_casi_continente1],
                                                  'Valore massimo di casi totali': [humanize.intcomma(valore_max_casi_continente1)],
                                                  'Stato col valore massimo di casi totali': [stato_valore_max_casi_continente1],
                                                  'Media di casi totali': [humanize.intcomma(media_casi_continente1)],
                                                  'Percentuale dei casi totali rispetto al totale mondiale': [confronto_casi_mondiale_continente1]})
    # passaggio 7: crea un dataframe riepilogativo per continente 2
    df_riepilogo_stat_continente2 = pd.DataFrame({'Continente': [continente2],
                                                  'Valore minimo di casi totali': [humanize.intcomma(valore_min_casi_continente2)],
                                                  'Stato col valore minimo di casi totali': [stato_valore_min_casi_continente2],
                                                  'Valore massimo di casi totali': [humanize.intcomma(valore_max_casi_continente2)],
                                                  'Stato col valore massimo di casi totali': [stato_valore_max_casi_continente2],
                                                  'Media di casi totali': [humanize.intcomma(media_casi_continente2)],
                                                  'Percentuale dei casi totali rispetto al totale mondiale': [confronto_casi_mondiale_continente2]})
    
    df_riepilogo = pd.concat([df_riepilogo_stat_continente1, df_riepilogo_stat_continente2])
    
    return df_riepilogo
```
Ecco un esempio del funzionamento della funzione:

Qual è il primo continente che vuoi confrontare?:  Oceania

Qual è il secondo continente che vuoi confrontare?:  North America

Continente | Valore minimo di casi totali | Stato col valore minimo di casi totali | Valore massimo di casi totali | Stato col valore massimo di casi totali | Media di casi totali | Percentuale dei casi totali rispetto al totale mondiale |
-----------|-----------------|------|-----------------|------|----------------|--------------------|
Oceania|4.0|Pitcairn|11,769,858.0|Australia|614,237.75|1.90
North America|1,403.0|Montserrat|103,436,829.0|United States|3,037,218.8|16.08

#### Fase 5: Si chiede poi di effettuare lo stesso tipo di analisi – anche in questo caso sviluppando una funzione ad hoc – per il numero di vaccinazioni totali per ogni continente

```python
def confronto_vac_continenti(dataset):
    # passaggio 1 : filtra i due dataset per i continenti selezionati, generando due nuovi dataset
    continente1 = input("Qual è il primo continente che vuoi confrontare?: ").capitalize()
    continente2 = input("Qual è il secondo continente che vuoi confrontare?: ").capitalize()
    df_continente1 = dataset[dataset["continent"].str.capitalize() == continente1]
    df_continente2 = dataset[dataset["continent"].str.capitalize() == continente2]
    # passaggio 2: calcola la somma delle vaccinazioni totali per ogni continente selezionato
    vaccinazioni_max_per_stato_continente1 = df_continente1.groupby(["location", "continent"])["total_vaccinations"].agg('max')
    totale_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.sum()
    vaccinazioni_max_per_stato_continente2 = df_continente2.groupby(["location", "continent"])["total_vaccinations"].agg('max')
    totale_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.sum()
    # passaggio 3: calcola la somma delle vaccinazioni totali mondiali (includendo anche i casi in cui continent is null)
    vaccinazioni_totali_ogni_stato = dataset.groupby(["location", "continent"])["total_vaccinations"].agg('max')
    vaccinazioni_totali_mondo =  vaccinazioni_totali_ogni_stato.sum()
    # passaggio 4: genera i descrittori statistici per continente 1
    valore_min_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.min()
    stato_valore_min_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.idxmin()[0]
    valore_max_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.max()
    stato_valore_max_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.idxmax()[0]
    media_vaccinazioni_continente1 = vaccinazioni_max_per_stato_continente1.mean()
    media_vaccinazioni_continente1 = round(media_vaccinazioni_continente1, 2)
    confronto_vaccinazioni_mondiale_continente1 = (totale_vaccinazioni_continente1 / vaccinazioni_totali_mondo) * 100
    confronto_vaccinazioni_mondiale_continente1 = round(confronto_vaccinazioni_mondiale_continente1, 2)
    # passaggio 5: genera i descrittori statistici per continente 2
    valore_min_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.min()
    stato_valore_min_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.idxmin()[0]
    valore_max_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.max()
    stato_valore_max_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.idxmax()[0]
    media_vaccinazioni_continente2 = vaccinazioni_max_per_stato_continente2.mean()
    media_vaccinazioni_continente2 = round(media_vaccinazioni_continente2, 2)
    confronto_vaccinazioni_mondiale_continente2 = (totale_vaccinazioni_continente2 / vaccinazioni_totali_mondo) * 100
    confronto_vaccinazioni_mondiale_continente2 = round(confronto_vaccinazioni_mondiale_continente2, 2)
    # passaggio 6: crea un dataframe riepilogativo per continente 1
    df_riepilogo_stat_continente1 = pd.DataFrame({'Continente': [continente1],
                                                  'Valore minimo di vaccinazioni': [humanize.intcomma(valore_min_vaccinazioni_continente1)],
                                                  'Stato col valore minimo di vaccinazioni': [stato_valore_min_vaccinazioni_continente1],
                                                  'Valore massimo di vaccinazioni': [humanize.intcomma(valore_max_vaccinazioni_continente1)],
                                                  'Stato col valore massimo di vaccinazioni': [stato_valore_max_vaccinazioni_continente1],
                                                  'Media di vaccinazioni': [humanize.intcomma(media_vaccinazioni_continente1)],
                                                  'Percentuale delle vaccinazioni rispetto al totale mondiale': [confronto_vaccinazioni_mondiale_continente1]})
    # passaggio 7: crea un dataframe riepilogativo per continente 2
    df_riepilogo_stat_continente2 = pd.DataFrame({'Continente': [continente2],
                                                  'Valore minimo di vaccinazioni': [humanize.intcomma(valore_min_vaccinazioni_continente2)],
                                                  'Stato col valore minimo di vaccinazioni': [stato_valore_min_vaccinazioni_continente2],
                                                  'Valore massimo di vaccinazioni': [humanize.intcomma(valore_max_vaccinazioni_continente2)],
                                                  'Stato col valore massimo di vaccinazioni': [stato_valore_max_vaccinazioni_continente2],
                                                  'Media di vaccinazioni': [humanize.intcomma(media_vaccinazioni_continente2)],
                                                  'Percentuale delle vaccinazioni rispetto al totale mondiale': [confronto_vaccinazioni_mondiale_continente2]})
    df_riepilogo = pd.concat([df_riepilogo_stat_continente1, df_riepilogo_stat_continente2])
    
    return df_riepilogo
```

Ecco un esempio del funzionamento della funzione:

Qual è il primo continente che vuoi confrontare?:  Europe

Qual è il secondo continente che vuoi confrontare?:  South America

Continente | Valore minimo di vaccinazioni | Stato col valore minimo di vaccinazioni | Valore massimo di vaccinazioni | Stato col valore massimo di vaccinazioni | Media di vaccinazioni | Percentuale delle vaccinazioni rispetto al totale mondiale |
-----------|-----------------|------|-----------------|------|----------------|--------------------|
Europe|65,140.0|Monaco|192,221,468.0|Germany|29,095,316.06|11.43
South America|4,407.0|Falkland Islands|486,436,436.0|Brazil|74,197,074.08|7.02

#### Fase 6: Alla fine, basandosi sui calcoli fatti, il committente chiede di stilare un breve (tre o quattro righe) paragrafo testuale riassuntivo sulle statistiche di casi e vaccinazioni, che si concentri solo sulle differenze esistenti tra Europa, Sud America e Oceania

Continente | Casi totali MIN | Stato | Casi totali MAX | Stato | Casi totali MEAN | % Globale Casi totali | Vaccinazioni MIN | Stato | Vaccinazioni MAX | Stato | Vaccinazioni MEAN | % Globale Vaccinazioni
-----------|-----------------|------|-----------------|------|----------------|--------------------|-----------------|------|-----------------|------|------------------|------------------------|
Europe|26|Vatican|38,997,490|France|4,941,438.63|32.54|65,140|Monaco|192,221,468|Germany|29,095,316.06|11.43
South America|1,923|Falkland Islands|37,519,960|Brazil|4,910,799.0|8.88|4,407|Falkland Islands|486,436,436|Brazil|74,197,074.08|7.02
Oceania|4|Pitcairn|11,769,858|Australia|614,237.75|1.90|117|Pitcairn|69,306,345|Australia|4,864,102.89|0.64

Confrontando le statistiche sui casi totali e sulle vaccinazioni relative a Europa, Sud America e Oceania emerge come il dato percentuale (sia per i casi totali che per le vaccinazioni) più rilevante sia quello europeo, seguito da quello sudamericano e da quello dell'Oceania. Al tempo stesso si evidenzia come, per tutti e tre i continenti, i valori minimi e massimi per entrambi i fattori siano direttamente proporzionali all'estensione dello Stato e alla sua popolazione totale (per un'analisi più completa sarebbe suggeribile rapportare i valori minimi e massimi alla popolazione dei vari Stati).
