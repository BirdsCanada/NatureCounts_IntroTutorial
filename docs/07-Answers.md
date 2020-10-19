# Exercise Answers {#Ans7}



## Chapter 3 {#Ans7.1}

Exercise 1: If you are interesting in doing a research project on Snowy Owls in Quebec, which three collections are you most likely to consider using (i.e., which have the most data)?


```r
search_species("Snowy Owl")
```

```
##   species_id scientific_name english_name        french_name taxon_group
## 1       7450 Bubo scandiacus    Snowy Owl Harfang des neiges       BIRDS
```

```r
search_region("Quebec", type = "statprov")
```

```
##   country_code statprov_code statprov_name_es statprov_name statprov_name_fr
## 1           CA            QC           Quebec        Québec           Québec
```

```r
nc_count(species=7450, region = list(statprov="QC"))
```

```
## Without a username, using 'show = "all"'
```

```
## Using filters: species (7450); statprov (QC)
```

```
##               collection nrecords
## 1                    CBC      387
## 2           CMMN-DET-OOT       15
## 3            EBIRD-CA-QC    29529
## 4                   GBBC       77
## 5              NESTWATCH        1
## 6              OISEAUXQC      706
## 7                    PFW        3
## 8   QCATLAS_NORTH_BE_RAW       83
## 9  QCATLAS_NORTH_BE_SUMM       67
## 10        QCATLAS1BE_RAW        2
## 11       QCATLAS1BE_SUMM        5
## 12         QCATLAS2BE_DO        5
## 13        QCATLAS2BE_RAW       71
## 14       QCATLAS2BE_SUMM       60
## 15            QCATLAS2RC        3
```
Answer: EBird-CA-QC, OISEAUXQC, CBC

Exercise 2: How many records of Gadwal are in the British Columbia Coastal Waterbird Survey collection? What if you are only interested in records from 2010-2019, how many records are available?


```r
search_species("Gadwal")
```

```
##   species_id                      scientific_name
## 1        390                      Mareca strepera
## 2      40151 Mareca strepera x Anas platyrhynchos
## 3      40152   Spatula clypeata x Mareca strepera
## 4      41251         Mareca strepera x Anas acuta
## 5      41471             Mareca strepera strepera
## 6      41472               Mareca strepera couesi
##                           english_name                         french_name
## 1                              Gadwall                      Canard chipeau
## 2           Gadwall x Mallard (hybrid) Hybride Canard chipeau x C. colvert
## 3 Northern Shoveler x Gadwall (hybrid) Hybride Canard souchet x C. chipeau
## 4  Gadwall x Northern Pintail (hybrid)   Hybride Canard chipeau x C. pilet
## 5                     Gadwall (Common)           Canard chipeau (strepera)
## 6                    Gadwall (Coues's)             Canard chipeau (couesi)
##   taxon_group
## 1       BIRDS
## 2       BIRDS
## 3       BIRDS
## 4       BIRDS
## 5       BIRDS
## 6       BIRDS
##  [ reached 'max' / getOption("max.print") -- omitted 3 rows ]
```

```r
collections<-meta_collections()
nc_count(species=390, collections = "BCCWS")
```

```
## Without a username, using 'show = "all"'
```

```
## Using filters: collections (BCCWS); species (390)
```

```
##   collection nrecords
## 1      BCCWS      702
```

```r
nc_count(species=390, collections = "BCCWS", years =c(2010,2019))
```

```
## Without a username, using 'show = "all"'
```

```
## Using filters: collections (BCCWS); species (390); start_year (2010); end_year (2019)
```

```
##   collection nrecords
## 1      BCCWS      389
```
Answer: 702, 389

## Chapter 4 {#Ans7.2}

Exercise 1: You (user “sample”) are from the Northwest Territories and interested in learning more about birds in your region. First, you identify the NatureCounts dataset most suitable for this exercise using the nc_count() function (i.e., which has the most data). Next, you decide to focus your download to only include Blackpoll Warbler data collected over the past 5 years (2015-2020). How many observation records did you download?


```r
search_region("Northwest Territories", type = "statprov")
```

```
##   country_code statprov_code         statprov_name_es         statprov_name
## 1           CA            NT Territorios del Noroeste Northwest Territories
##            statprov_name_fr
## 1 Territoires-du-Nord-Ouest
```

```r
nc_count(region = list(statprov="NT"), username="sample")
```

```
## Using filters: statprov (NT)
```

```
##   access collection nrecords
## 1    yes   ABATLAS2        5
## 2    yes ABBIRDRECS      520
## 3    yes        BBS     5692
## 4    yes  BBS50-CAN    51194
## 5    yes EBUTTERFLY       25
## 6    yes        PFW      917
```

```r
search_species("Blackpoll warbler")
```

```
##   species_id              scientific_name
## 1      16820            Setophaga striata
## 2      42154   Setophaga castanea/striata
## 3      46697 Setophaga castanea x striata
##                                english_name
## 1                         Blackpoll Warbler
## 2            Bay-breasted/Blackpoll Warbler
## 3 Bay-breasted x Blackpoll Warbler (hybrid)
##                                   french_name taxon_group
## 1                              Paruline rayée       BIRDS
## 2        Paruline à poitrine baie ou P. rayée       BIRDS
## 3 Hybride Paruline à poitrine baie x P. rayée       BIRDS
```

```r
BLPW<-nc_data_dl(collections="BBS50-CAN", species = 16820, years =c(2015, 2020), username="sample", info="tutorial example")
```

```
## Using filters: collections (BBS50-CAN); species (16820); fields_set (BMDE2.00-min); start_year (2015); end_year (2020)
```

```
## Collecting available records...
```

```
##   access collection nrecords
## 1    yes  BBS50-CAN     1756
## Total records: 1,756
```

```
## 
## Downloading records for each collection:
```

```
##   BBS50-CAN
```

```
##     Records 1 to 1756 / 1756
```

Answer: BBS50-CAN, 1756

Exercise 2: You (user “sample”) are birding in the Beaverhill Lake Important Bird Area (iba) in May. You think you hear a Bobolink! You are curious if this species has been detected here in the month of May. You choose to download the records you have authorization to freely access from NatureCounts. How many observation records did you download? What year are these records from?


```r
search_region("Beaverhill Lake", type = "iba") 
```

```
##   iba_site     iba_name_fr latitude statprov area_ha alt_min        iba_name
## 1    AB001 Beaverhill Lake 53.45019       AB  208.47     668 Beaverhill Lake
##   nearest_town bcr ncc_region alt_max longitude status
## 1      Tofield  11         66     670 -112.5287      G
```

```r
search_species("Bobolink")
```

```
##   species_id       scientific_name english_name    french_name taxon_group
## 1      19520 Dolichonyx oryzivorus     Bobolink Goglu des prés       BIRDS
```

```r
#May = doy 122-152

BOBO<-nc_data_dl(region = list(iba="AB001"), species= 19520, doy=c(122,152),  username="sample", info="tutorial example")
```

```
## Using filters: species (19520); fields_set (BMDE2.00-min); start_doy (122); end_doy (152); iba (IBA.AB001)
```

```
## Collecting available records...
```

```
##   access collection nrecords
## 1    yes   ABATLAS1        3
## Total records: 3
```

```
## 
## Downloading records for each collection:
```

```
##   ABATLAS1
```

```
##     Records 1 to 3 / 3
```
Answer: 3, 1988 & 1987

## Chapter 5 {#Ans7.3}

Exercise 1: You (user “sample”) are doing a research project using the fall migration monitoring data collected at Vaseux Lake Bird Observatory, British Columbia. You request the open access data from 2017-2020. After you request this subset of the collection, you need to determine the number of unique days Gray catbirds were records in each year?


```r
collections<-meta_collections()
search_species("Gray Catbird") 
```

```
##   species_id        scientific_name english_name  french_name taxon_group
## 1      15900 Dumetella carolinensis Gray Catbird Moqueur chat       BIRDS
```

```r
VLBO <- nc_data_dl(collections = "CMMN-DET-VLBO", species=15900, years = c(2017, 2020), username = "sample", info = "tutorial example")
```

```
## Using filters: collections (CMMN-DET-VLBO); species (15900); fields_set (BMDE2.00-min); start_year (2017); end_year (2020)
```

```
## Collecting available records...
```

```
##   access    collection nrecords
## 1    yes CMMN-DET-VLBO      107
## Total records: 107
```

```
## 
## Downloading records for each collection:
```

```
##   CMMN-DET-VLBO
```

```
##     Records 1 to 107 / 107
```

```r
VLBO<-format_dates(VLBO) #This step is not strictly required.

GRCA<-VLBO %>% group_by(survey_year) %>% summarize(day_distinct = n_distinct(doy)) #Could have alternatively counted the number of district `SamplingEventIdentifier` 
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```
Answer: 2017 = 54 2018 = 53

Exercise 2: Building off the same dataset used in Exercise 1, you now want to zero-fill the dataframe to ensure it is complete for your study on Gray catbirds. How many records (rows) are in the zero-fill Gray catbird dataframe?


```r
#You will pull all the data for this exercise, not just the records for Gray catbird. This ensures the zero-fill works correctly

VLBO <- nc_data_dl(collections = "CMMN-DET-VLBO", years = c(2017, 2020), username = "sample", info = "tutorial example")
```

```
## Using filters: collections (CMMN-DET-VLBO); fields_set (BMDE2.00-min); start_year (2017); end_year (2020)
```

```
## Collecting available records...
```

```
##   access    collection nrecords
## 1    yes CMMN-DET-VLBO     6664
## Total records: 6,664
```

```
## 
## Downloading records for each collection:
```

```
##   CMMN-DET-VLBO
```

```
##     Records 1 to 5000 / 6664
```

```
##     Records 5001 to 6664 / 6664
```

```r
VLBO<-format_zero_fill(VLBO)
```

```
##  - Consider summarizing multiple observations per set of 'by' before zero-filling to increase speed
```

```
##  - Converted 'fill' column (ObservationCount) from character to numeric
```

```r
GRCA<-VLBO %>% filter(species_id==15900)

#The number of records is the number of rows in the resulting dataframe. 
```
Answer: 152
