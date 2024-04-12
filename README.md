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
![alt text](https://github.com/simonepetrini/Covid19_Analysis/blob/img/Covid1.png?raw=True)

#### Fase 2: Si richiede di verificare le dimensioni del dataset e le diciture presenti nell'intestazione

Effettuo una prima analisi esplorativa con .info(); Aggiungendo il parametro memory_usage="deep" restituisce esattamente la memoria fisica occupata dal dataset

```python
df_covid.info(memory_usage="deep")
```
![alt text](https://github.com/simonepetrini/Covid19_Analysis/blob/img/Covid2.png?raw=True)
![alt text](https://github.com/simonepetrini/Covid19_Analysis/blob/img/Covid3.png?raw=True)

Verifico l'elenco totale delle colonne del dataset con il loro rispettivo nome

```python
df_covid.columns
```

![alt text](https://github.com/simonepetrini/Covid19_Analysis/blob/img/Covid4.png?raw=True)

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

![alt text](https://github.com/simonepetrini/Covid19_Analysis/blob/img/Covid5.png?raw=True)
