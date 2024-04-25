

## EasyMultiProfiler: An Efficient and Convenient R package in Multi-omics Down-Stream Analysis and Visualization
<a href="https://github.com/liubingdong/EasyMultiProfier/blob/main/man/figures/logo.png"><img src="https://github.com/liubingdong/EasyMultiProfier/blob/main/man/figures/logo.png" width=150 align="right" ></a>
![](https://img.shields.io/badge/R%20language->=4.3.0-brightgreen.svg)
![](https://img.shields.io/badge/Mac%20OSX%20&%20Windows-Available-brightgreen.svg)
![](https://img.shields.io/badge/Release%20version-0.1.0-brightgreen.svg)
[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/liubingdong/EasyMultiProfier)

The EasyMultiProfiler package aims to offer a user-friendly and efficient multi-omics data analysis tool on the R platform. It facilitates various essential tasks related to microbiome, genome, and metabolite downstream analysis, providing a seamless workflow from start to finish.

### Install

**Easily install**
```R
if (!requireNamespace("pak", quietly=TRUE)) install.packages("pak")
pak::pkg_install("liubingdong/EasyMultiProfiler")
library(EasyMultiProfiler)
```
NOTE 1: For some region with unstable network, users could utilize the local mirrors to avoid unexperted errors before installation.
```R
## For china main land users could use this
local({r <- getOption("repos")
r["CRAN"] <- "https://mirrors.tuna.tsinghua.edu.cn/CRAN/"
options(repos=r)}
)
options(BioC_mirror="https://mirrors.ustc.edu.cn/bioc/")
options("download.file.method"="libcurl")
options("url.method"="libcurl")
```

NOTE 2: For different R versions on windows platform, users need install appropriate rtools to supprot necessary compile envrioment (eg. for R 4.3.x need RTool4.3, for R 4.4.x need RTool4.4, [click here ~ 400MB](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)). Afterward, simply restat R and re-try below:

```R
pak::pkg_install("liubingdong/EasyMultiProfiler")
```

**Completely install** 

Due to the inclusion of many polular analysis tools, the EMP package relies on dependencies distributed across GitHub, CRAN, and Bioconductor repositories. Therefore, users may encounter dependency issues during installation in different network environments. If installation errors occur, we suggest manually installing any missing dependencies based on the error prompts. Thank you for your patience during installation, and we do believe EMP could largely speed up your research work.

```R
# In the step, please type in : 1 2 3 4 5 6 7 
setRepositories(addURLs = c(BioCsoft = "https://bioconductor.org/packages/3.18/bioc",
                  BioCann = "https://bioconductor.org/packages/3.18/data/annotation"))  
options(timeout = 600000000) 
install.packages("remotes") # remotes (>= 2.5.0)
install.packages("BiocManager") # BiocManager (>= 1.30.22)
BiocManager::install("base64enc") # base64enc (>= 0.1.3)
BiocManager::install("WGCNA") # WGCNA (>= 1.72.5)
BiocManager::install("clusterProfiler") # clusterProfiler (>= 4.10.0)
remotes::install_github("liubingdong/EasyMultiProfiler")
library(EasyMultiProfiler)
```

More installation error and solution: [**Click this**](https://github.com/liubingdong/EasyMultiProfiler/blob/main/tutorial_related/Installation.md)


### Import data 

[**demo data files is in the EMP_demo_data.7z**](https://github.com/liubingdong/EasyMultiProfiler/tree/main/tutorial_related/EMP_demo_data.7z) 

1. Completely import model

   NOTE: The Completely import model is designed for users handling multiple omics data in a corhort simultaneously, enabling efficient and rapid execution of various multi-omics analysis methods from start to finish.

   ```R
   meta_data <- read.table('col.txt',header = T,row.names = 1)
   
   dfmap <-read.table('dfmap.txt',header = T) |> 
     dplyr::mutate(assay = factor(assay)) ## factor is necessary
   
   tax_data <- EMP_taxonomy_import('tax.txt')
   ko_data <- EMP_function_import('ko.txt',type = 'ko')
   ec_data <- EMP_function_import('ec.txt',type = 'ec')
   metbol_data <- EMP_normal_import('metabol.txt',
                                    dfmap = dfmap,assay  = 'untarget_metabol')
   
   geno_data <- EMP_normal_import('tran.txt',dfmap = dfmap,assay  = 'host_gene')
   
   #### Note that the naming here must be consistent with that of dfmap
   objlist <- list("taxonomy" = tax_data,
                   "geno_ko" = ko_data,
                   "geno_ec" = ec_data,
                   "untarget_metabol" = metbol_data,
                   "host_gene" = geno_data)
   
   MAE <- MultiAssayExperiment::MultiAssayExperiment(objlist, meta_data, dfmap)
   ```

2. Quickly import model ( Only for one single omics data)

   NOTE: This function is specifically designed to enable users with single-omics datasets to efficiently apply various analysis methods in the EasyMultiProfiler package easily.

   ```R
   meta_data <- read.table('col.txt',header = T,row.names = 1)
   MAE <- EMP_easy_import('tran.txt',coldata = meta_data,type = 'normal')
   ```

### Prepare the demo data from EasyMultiProfiler

```R 
Data(MAE)
```

### Data Extract :

This module is designed to assist users in extracting relevant omics data and its associated information from MAE objects for subsequent downstream analysis.

#### EMP_assay_extract

1. Extract one assay data of the existed experiments from MultiAssaayExperiment

```R
MAE |>
    EMP_assay_extract('taxonomy')
```
    # A tibble: 20 × 444
       primary Archaea;Candidatus_Thermo…¹ Archaea;Euryarchaeot…² Bacteria;Actinobacte…³ Bacteria;Actinobacte…⁴
       <chr>                         <int>                  <int>                  <int>                  <int>
     1 P11774                            0                      0                      0                      0
     2 P31579                            0                      0                      0                      5
     3 P33531                            0                      0                      0                      4
     4 P36987                            0                      0                      0                      3
     5 P40725                            0                      0                      0                      0
     6 P40923                            0                      0                      0                      7
     7 P51467                            0                      0                      0                      0
     8 P51956                            0                     43                      0                      0
     9 P52630                            0                      0                      0                      0
    10 P54631                            0                      0                      0                      0
    11 P60426                            0                      0                      0                      4
    12 P66101                            0                      0                      0                      0
    13 P68071                            0                      0                      0                      5
    14 P69489                            0                     18                      0                      0
    15 P70597                            0                      0                      0                      0
    16 P75656                            0                      0                      0                      0
    17 P84567                            0                      0                      0                      0
    18 P94346                            0                      0                      3                     14
    19 P95158                            2                      0                      0                      4
    20 P96518                            0                      0                      0                      2
    # ℹ abbreviated names:
    #   ¹`Archaea;Candidatus_Thermoplasmatota;Thermoplasmata;Methanomassiliicoccales;Methanomassiliicoccaceae;Methanomassiliicoccus;Candidatus_Methanomassiliicoccus_intestinalis;`,
    #   ²`Archaea;Euryarchaeota;Methanobacteria;Methanobacteriales;Methanobacteriaceae;Methanobrevibacter;Methanobrevibacter_smithii;`,
    #   ³`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_meyeri;`,
    #   ⁴`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_odontolytica;`
    # ℹ 439 more variables:
    #   `Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_adolescentis;` <int>, …
    # ℹ Use `colnames()` to see all variable names

```R
MAE |>
    EMP_assay_extract('geno_ko')
```
    # A tibble: 20 × 7,630
       primary K00001 K00002 K00003 K00004 K00005 K00007 K00008 K00009 K00010 K00011 K00012 K00013 K00014
       <chr>    <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>
     1 P11774     119      2    290      2     33      0    191    184     15      0    301    475    458
     2 P31579      94      2    183     12     40      6    265    191      8      4    516    362    621
     3 P33531      36      3    208      2     28      0    259    150      7      0    227    390    360
     4 P36987      40      2    193      4     40      0    254    287     16      0    536    442    340
     5 P40725     185     17    280     13     27      0    242    199     15      1    293    270    333
     6 P40923     191      2    326     10     54      1    198    190     27      0    287    332    364
     7 P51467     286      9    216     16     58      0    310    259     36     14    165    357    354
     8 P51956     270      8    269      6     35      8    248    181      7      3    333    444    446
     9 P52630     169      6    356     12     57      0    216    176     31      2    293    335    435
    10 P54631     137      6    237      3     70      9    213    143     11      3    231    360    407
    11 P60426     140      2    276      5     42      1    237    191     13      3    316    360    369
    12 P66101     160     13    278     23     45      0    304    198     26      0    254    328    399
    13 P68071      64      1    378      1    104      0    451    176    102      0    380    390    351
    14 P69489     221     15    376      8    123      4    294    207     71      1    199    414    373
    15 P70597     111      8    227     11     55      0    231    147     11      1    197    362    380
    16 P75656      90      4    265      8     30      3    458    473     26      2    280    431    363
    17 P84567      81      6    229     17     53      0    316    290     11      0    268    341    354
    18 P94346     155     11    288     10     34      0    290    233     23      1    443    495    470
    19 P95158     100      6    270     10     43      0    477    140      5      1    270    310    432
    20 P96518      67      1    233     13     28      0    209    317     14      0    207    330    274
    # ℹ 7,616 more variables: K00015 <int>, K00016 <int>, K00018 <int>, K00020 <int>, K00021 <int>,
    #   K00024 <int>, K00026 <int>, K00027 <int>, K00029 <int>, K00030 <int>, K00031 <int>, K00032 <int>,
    #   K00033 <int>, K00034 <int>, K00036 <int>, K00038 <int>, K00039 <int>, K00040 <int>, K00041 <int>,
    #   K00042 <int>, K00043 <int>, K00045 <int>, K00048 <int>, K00052 <int>, K00053 <int>, K00054 <int>,
    #   K00055 <int>, K00057 <int>, K00058 <int>, K00059 <int>, K00060 <int>, K00064 <int>, K00066 <int>,
    #   K00067 <int>, K00069 <int>, K00073 <int>, K00074 <int>, K00075 <int>, K00077 <int>, K00082 <int>,
    #   K00086 <int>, K00087 <int>, K00088 <int>, K00090 <int>, K00091 <int>, K00094 <int>, K00096 <int>, …
    # ℹ Use `colnames()` to see all variable names


2. Search for specific feature according to the rowdata

```R
MAE |>
    	EMP_assay_extract('geno_ec',pattern = '1.1.1.1',pattern_ref = 'feature',exact = T)
```
    # A tibble: 20 × 2
       primary `1.1.1.1`
       <chr>       <int>
     1 P11774        412
     2 P31579        327
     3 P33531        285
     4 P36987        163
     5 P40725        433
     6 P40923        497
     7 P51467        599
     8 P51956        534
     9 P52630        478
    10 P54631        329
    11 P60426        430
    12 P66101        415
    13 P68071        252
    14 P69489        501
    15 P70597        300
    16 P75656        286
    17 P84567        219
    18 P94346        408
    19 P95158        377
    20 P96518        229

```R
MAE |>
    	EMP_assay_extract('geno_ko',pattern = 'mtlD',pattern_ref = 'Name',exact = F)
```
    # A tibble: 20 × 2
       primary K00009
       <chr>    <int>
     1 P11774     184
     2 P31579     191
     3 P33531     150
     4 P36987     287
     5 P40725     199
     6 P40923     190
     7 P51467     259
     8 P51956     181
     9 P52630     176
    10 P54631     143
    11 P60426     191
    12 P66101     198
    13 P68071     176
    14 P69489     207
    15 P70597     147
    16 P75656     473
    17 P84567     290
    18 P94346     233
    19 P95158     140
    20 P96518     317


### EMP_rowdata_extract

1. Extract the rowdata of one existed experiment from MultiAssaayExperiment

```R
MAE |>
  EMP_rowdata_extract('taxonomy')
```
    # A tibble: 443 × 9
       feature                                            Kindom Phylum Class Order Family Genus Species Strain
       <chr>                                              <chr>  <chr>  <chr> <chr> <chr>  <chr> <chr>   <chr> 
     1 Archaea;Candidatus_Thermoplasmatota;Thermoplasmat… Archa… Candi… Ther… Meth… Metha… Meth… Candid… Candi…
     2 Archaea;Euryarchaeota;Methanobacteria;Methanobact… Archa… Eurya… Meth… Meth… Metha… Meth… Methan… Metha…
     3 Bacteria;Actinobacteria;Actinomycetia;Actinomycet… Bacte… Actin… Acti… Acti… Actin… Scha… Schaal… Schaa…
     4 Bacteria;Actinobacteria;Actinomycetia;Actinomycet… Bacte… Actin… Acti… Acti… Actin… Scha… Schaal… Schaa…
     5 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
     6 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
     7 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
     8 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
     9 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
    10 Bacteria;Actinobacteria;Actinomycetia;Bifidobacte… Bacte… Actin… Acti… Bifi… Bifid… Bifi… Bifido… Bifid…
    # ℹ 433 more rows
    # ℹ Use `print(n = ...)` to see more rows

```R
MAE |>
  EMP_rowdata_extract('geno_ko')  
```
    # A tibble: 7,629 × 2
       feature Name                                                                                            
       <chr>   <chr>                                                                                           
     1 K00001  E1.1.1.1, adh; alcohol dehydrogenase [EC:1.1.1.1]                                               
     2 K00002  AKR1A1, adh; alcohol dehydrogenase (NADP+) [EC:1.1.1.2]                                         
     3 K00003  hom; homoserine dehydrogenase [EC:1.1.1.3]                                                      
     4 K00004  BDH, butB; (R,R)-butanediol dehydrogenase / meso-butanediol dehydrogenase / diacetyl reductase …
     5 K00005  gldA; glycerol dehydrogenase [EC:1.1.1.6]                                                       
     6 K00007  dalD; D-arabinitol 4-dehydrogenase [EC:1.1.1.11]                                                
     7 K00008  SORD, gutB; L-iditol 2-dehydrogenase [EC:1.1.1.14]                                              
     8 K00009  mtlD; mannitol-1-phosphate 5-dehydrogenase [EC:1.1.1.17]                                        
     9 K00010  iolG; myo-inositol 2-dehydrogenase / D-chiro-inositol 1-dehydrogenase [EC:1.1.1.18 1.1.1.369]   
    10 K00011  AKR1B; aldehyde reductase [EC:1.1.1.21]                                                         
    # ℹ 7,619 more rows
    # ℹ Use `print(n = ...)` to see more rows

2. Extract the rowdata of  all the experiments in the MultiAssaayExperiment

```R
MAE |>
  EMP_rowdata_extract() -> total_row_data
dim(total_row_data)
```
    [1] 11679    18

### EMP_coldata_extract

1. Extract the coldata/meta-data/sample-info/patient-info of one existed experiment in the MultiAssaayExperiment

```R
MAE |>
  EMP_coldata_extract('taxonomy')
```

    # A tibble: 20 × 16
       primary Group   Status      Age Region Sex   Height Weight   BMI Education_Years  PHQ9  GAD7   SAS   SDS
       <chr>   <chr>   <chr>     <int> <chr>  <chr>  <dbl>  <dbl> <dbl>           <int> <int> <int> <int> <int>
     1 P31579  Group_A No           26 Guang… M       1.7    58    20.1              16     4     2    31    40
     2 P66101  Group_A No           24 Guang… M       1.73   66    22.0              18     0     0    44    34
     3 P75656  Group_A No           27 Guang… M       1.82   88    26.6              17    10     5    36    49
     4 P96518  Group_A No           25 Guang… F       1.6    50    19.5              16     4     5    NA    NA
     5 P40923  Group_A No           26 Paris  M       1.82   85    25.7              18     4     3    35    38
     6 P36987  Group_A No           26 Paris  F       1.55   45    18.7              18     4     6    46    36
     7 P40725  Group_A Mild         27 Paris  F       1.57   47    19.1              17     3     3    45    31
     8 P52630  Group_A Mild         30 Paris  M       1.78   70    22.1              19     0     1    41    40
     9 P94346  Group_A Mild         27 Paris  M       1.67   48    17.2              19     1     0    35    33
    10 P54631  Group_A Mild         60 Guang… F       1.53   49    20.9              12    11    NA    39    39
    11 P70597  Group_B Mild         40 Paris  M       1.7    62    21.4              16    12    17    54    60
    12 P51467  Group_B Mild         56 Paris  F       1.5    49    21.8               9    NA    NA    44    44
    13 P60426  Group_B Mild         39 Guang… F       1.6    59    23.0               9     5     3    53    44
    14 P68071  Group_B Signific…    25 Guang… M       1.72   58    19.6              16     3     0    29    31
    15 P69489  Group_B Signific…    34 Guang… F       1.56   55.8  22.9              16     4     4    35    45
    16 P95158  Group_B Signific…    40 Guang… M       1.68   52    18.4              19    13    12    46    40
    17 P84567  Group_B Signific…    38 Guang… F       1.62   62    23.6              16     4     6    33    44
    18 P33531  Group_B Signific…    36 Guang… M       1.73   56    18.7              16     3     0    29    34
    19 P51956  Group_B Signific…    28 Guang… M       1.72   60    20.3              12     6     7    49    54
    20 P11774  Group_B Signific…    29 Paris  F       1.5    50    22.2              21    11     7    31    33
    # ℹ 2 more variables: HAMA <int>, HAMD <int>

2. Extract the coldata of all the experiments in the MultiAssaayExperiment

```R
MAE |>
  EMP_coldata_extract(action = 'get')  ### when action = get, output is a tibble.
MAE |>
  EMP_coldata_extract(action = 'add')  ### when action = add, output is a EMPT object 
```

### Data Preparation :

This module is designed to assist users in various mainstream data preprocessing steps, including standardization, batch correction, filtering analysis, feature convert, and feature collapse, etc.

#### EMP_adjust_abudance

combat_seq method 

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',
               collapse_by='row',na_string = c("NA", "null", "","-"),
               estimate_group = 'MS2kegg',method = 'mean',collapse_sep = '+') |>
  EMP_collapse(collapse_by='col',estimate_group = 'Group',method = 'mean',collapse_sep = '+')
```
    Primary changed!
    Current primary: Group
    # A tibble: 2 × 193
      primary C00020 C00022 C00025 C00047 C00051 C00062 C00064 C00072 C00073 C00074 C00078 C00079 C00082 C00084
      <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    1 Group_A  7637.  4512.  4583. 11187.  2604. 45390. 18038.  3933. 10956. 15073. 4.58e5 1.04e6 62086. 1.17e5
    2 Group_B  7951.  5265.  4689. 10651.  3506. 51619. 17809.  3344. 11833. 13258. 5.10e5 1.11e6 74118. 1.15e5
    # ℹ 178 more variables: C00086 <dbl>, C00090 <dbl>, C00106 <dbl>, C00114 <dbl>, C00133 <dbl>,
    #   C00135 <dbl>, C00144 <dbl>, C00146 <dbl>, C00148 <dbl>, C00156 <dbl>, C00157 <dbl>, C00158 <dbl>,
    #   C00180 <dbl>, C00183 <dbl>, C00188 <dbl>, C00195 <dbl>, C00207 <dbl>, C00212 <dbl>, C00219 <dbl>,
    #   C00242 <dbl>, C00245 <dbl>, C00249 <dbl>, C00255 <dbl>, C00262 <dbl>, C00294 <dbl>, C00299 <dbl>,
    #   C00300 <dbl>, C00318 <dbl>, C00327 <dbl>, C00328 <dbl>, C00346 <dbl>, C00350 <dbl>, C00362 <dbl>,
    #   C00366 <dbl>, C00387 <dbl>, C00407 <dbl>, C00416 <dbl>, C00422 <dbl>, C00463 <dbl>, C00472 <dbl>,
    #   C00486 <dbl>, C00499 <dbl>, C00500 <dbl>, C00548 <dbl>, C00550 <dbl>, C00588 <dbl>, C00633 <dbl>, …
    # ℹ Use `colnames()` to see all variable names


combat method 

```R
MAE |>
  EMP_assay_extract(experiment='geno_ko') |>
  EMP_adjust_abudance(.factor_unwanted = 'Region',.factor_of_interest = 'Group',
                      method = 'combat',action = 'add') 
```
    # A tibble: 20 × 7,630
       primary K00001 K00002 K00003 K00004 K00005 K00007 K00008 K00009 K00010  K00011 K00012 K00013 K00014
       <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>   <dbl>  <dbl>  <dbl>  <dbl>
     1 P11774   112.    1.83   288.   1.74   30.7 0.234    190.   187.  13.1   0.0334   300.   459.   471.
     2 P31579    97.7   2.20   187.  11.8    39.9 4.44     260.   194.   9.07  3.85     502.   363.   592.
     3 P33531    40.3   3.20   212.   2.25   29.8 0        258.   153.   8.05  0.0354   229.   391.   362.
     4 P36987    37.4   1.83   192.   3.61   40.5 0.0907   269.   305.  14.1   0.0266   534.   428.   333.
     5 P40725   174.   16.4    278.  13.0    25.2 0.0907   254.   197.  13.0   0.971    293.   280.   325.
     6 P40923   180.    1.83   324.   9.81   58.1 1.79     201.   187.  28.2   0.0266   287.   335.   361.
     7 P51467   270.    8.57   215.  16.9    60.5 0.234    335.   280.  41.6  12.2      165.   359.   350.
     8 P51956   262.    7.93   270.   6.05   36.4 5.37     248.   181.   8.05  2.93     328.   446.   439.
     9 P52630   159.    5.68   354.  11.9    62.0 0.0907   223.   170.  33.9   1.89     293.   337.   443.
    10 P54631   139.    6.04   239.   3.38   65.6 6.36     215.   150.  11.9   2.91     236.   361.   405.
    11 P60426   142.    2.21   276.   5.13   42.8 0.792    238.   190.  13.7   2.93     312.   361.   370.
    12 P66101   160.   12.4    277.  21.5    44.3 0.0545   294.   200.  25.2   0.0310   257.   329.   398.
    13 P68071    68.8   1.20   371.   1.24   95.7 0        419.   176.  82.5   0.0354   372.   391.   354.
    14 P69489   218.   14.3    369.   7.87  111.  2.88     288.   204.  60.2   1.02     202.   416.   374.
    15 P70597   104.    7.60   226.  11.3    56.7 0.234    238.   143.   8.64  0.984    197.   363.   380.
    16 P75656    93.8   4.14   265.   8.18   30.9 2.40     421.   435.  25.2   1.97     282.   433.   366.
    17 P84567    85.6   6.07   232.  15.7    52.6 0        307.   275.  11.9   0.0354   268.   342.   357.
    18 P94346   146.   10.5    286.   9.81   33.3 0.0907   314.   238.  22.9   0.971    442.   472.   484.
    19 P95158   104.    6.07   271.   9.66   43.7 0        440.   144.   6.05  1.02     269.   311.   427.
    20 P96518    71.3   1.19   235.  12.7    29.1 0.0545   211.   304.  14.7   0.0310   212.   331.   284.
    # ℹ 7,616 more variables: K00015 <dbl>, K00016 <dbl>, K00018 <dbl>, K00020 <dbl>, K00021 <dbl>,
    #   K00024 <dbl>, K00026 <dbl>, K00027 <dbl>, K00029 <dbl>, K00030 <dbl>, K00031 <dbl>, K00032 <dbl>,
    #   K00033 <dbl>, K00034 <dbl>, K00036 <dbl>, K00038 <dbl>, K00039 <dbl>, K00040 <dbl>, K00041 <dbl>,
    #   K00042 <dbl>, K00043 <dbl>, K00045 <dbl>, K00048 <dbl>, K00052 <dbl>, K00053 <dbl>, K00054 <dbl>,
    #   K00055 <dbl>, K00057 <dbl>, K00058 <dbl>, K00059 <dbl>, K00060 <dbl>, K00064 <dbl>, K00066 <dbl>,
    #   K00067 <dbl>, K00069 <dbl>, K00073 <dbl>, K00074 <dbl>, K00075 <dbl>, K00077 <dbl>, K00082 <dbl>,
    #   K00086 <dbl>, K00087 <dbl>, K00088 <dbl>, K00090 <dbl>, K00091 <dbl>, K00094 <dbl>, K00096 <dbl>, …
    # ℹ Use `colnames()` to see all variable names

limma_remove_batch_effect

```R
MAE |>
  EMP_assay_extract(experiment='geno_ko') |>
  EMP_adjust_abudance(.factor_unwanted = 'Region',.factor_of_interest = 'Group',
                      method = 'limma_remove_batch_effect') 
```
    # A tibble: 20 × 7,630
       primary K00001 K00002 K00003 K00004 K00005 K00007 K00008 K00009 K00010 K00011 K00012 K00013 K00014
       <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
     1 P11774   2417.   17.1  8612.   17.1   404.   5.85  4739.  4493.  142.    5.85  9084. 17480. 16589.
     2 P31579   2657.   23.2  4720.  121.    498.  20.0   5695.  4460.   89.0  35.2  20270. 12540. 25928.
     3 P33531    698.   32.3  5671.   19.2   306.   1.92  5515.  3162.   76.4   6.61  6248. 13964. 11855.
     4 P36987    621.   18.8  5719.   35.3   621.   5.85  8473. 10093.  181.    5.85 24751. 18760. 12871.
     5 P40725   5124.  187.   9271.  132.    346.   5.85  7523.  5687.  159.   11.6   9894.  8800. 11887.
     6 P40923   4935.   17.5 10607.   88.9   827.  11.2   5195.  4898.  320.    5.85  8836. 10888. 12424.
     7 P51467   8121.   73.3  5435.  149.    846.   5.85  9115.  7046.  438.  126.    3701. 11159. 11025.
     8 P51956  11469.   86.0  7869.   51.8   399.  27.0   4979.  3971.   74.0  26.2  10388. 16155. 15485.
     9 P52630   4156.   49.2 12070.  112.    895.   5.85  5900.  4404.  386.   17.5   9129. 11062. 16093.
    10 P54631   5137.   70.8  7749.   29.0  1227.  35.8   4727.  3348.  145.   29.1   7263. 14120. 16037.
    11 P60426   4813.   23.6  8751.   44.9   548.   4.23  5001.  4595.  164.   27.4  10330. 12818. 12647.
    12 P66101   6143.  172.   9334.  301.    635.   1.92  7541.  5106.  427.    6.61  7975. 11841. 14938.
    13 P68071   1514.   15.0 13037.   12.3  1852.   1.92 11939.  3881. 2563.    6.61 12772. 13645. 11171.
    14 P69489   7897.  171.  11657.   66.8  2119.  11.8   5823.  4410. 1389.   11.9   4560. 13393. 10982.
    15 P70597   2597.   75.9  7206.  112.    965.   5.85  7388.  3874.  112.   11.8   5883. 14068. 15083.
    16 P75656   2580.   43.1  8278.   75.9   347.   9.92 12896. 16894.  407.   19.6   8709. 16641. 12385.
    17 P84567   2456.   71.2  7431.  212.    837.   1.92  8378.  9265.  146.    6.61  9051. 13158. 13222.
    18 P94346   3437.   94.5  8327.   84.2   411.   5.85  8410.  6148.  243.   11.0  15443. 18111. 16812.
    19 P95158   3070.   67.1  8720.  101.    581.   1.92 14023.  3034.   55.3  12.7   8479. 10637. 16309.
    20 P96518   1912.   15.9  7748.  152.    353.   1.92  4713. 10707.  201.    6.61  6360. 12769.  9313.
    # ℹ 7,616 more variables: K00015 <dbl>, K00016 <dbl>, K00018 <dbl>, K00020 <dbl>, K00021 <dbl>,
    #   K00024 <dbl>, K00026 <dbl>, K00027 <dbl>, K00029 <dbl>, K00030 <dbl>, K00031 <dbl>, K00032 <dbl>,
    #   K00033 <dbl>, K00034 <dbl>, K00036 <dbl>, K00038 <dbl>, K00039 <dbl>, K00040 <dbl>, K00041 <dbl>,
    #   K00042 <dbl>, K00043 <dbl>, K00045 <dbl>, K00048 <dbl>, K00052 <dbl>, K00053 <dbl>, K00054 <dbl>,
    #   K00055 <dbl>, K00057 <dbl>, K00058 <dbl>, K00059 <dbl>, K00060 <dbl>, K00064 <dbl>, K00066 <dbl>,
    #   K00067 <dbl>, K00069 <dbl>, K00073 <dbl>, K00074 <dbl>, K00075 <dbl>, K00077 <dbl>, K00082 <dbl>,
    #   K00086 <dbl>, K00087 <dbl>, K00088 <dbl>, K00090 <dbl>, K00091 <dbl>, K00094 <dbl>, K00096 <dbl>, …
    # ℹ Use `colnames()` to see all variable names


#### EMP_feature_convert

1. For gene ID convert

```R
MAE |>
  EMP_feature_convert(experiment = 'host_gene',
                      from = 'SYMBOL',to='ENTREZID',species = 'Human')
```

    Feature changed!
    Current feature: ENTREZID
    # A tibble: 20 × 309
       primary   `1` `100` `10005` `10006` `100128640` `100132116` `100507098` `100526760` `10057` `10058`
       <chr>   <dbl> <dbl>   <dbl>   <dbl>       <dbl>       <dbl>       <dbl>       <dbl>   <dbl>   <dbl>
     1 P11774    104   353     172    1309           2           2          29           0     816     207
     2 P31579     25   145     183    1690          24           0          18           0    2172     607
     3 P33531     20   590     184    2352          18           0           4           0    2236     503
     4 P36987     49   136     168    1831           7           0          23           0    1157     344
     5 P40725     17   120     219    1715          16           0           7           0    1449     431
     6 P40923     27   249     236    1707          17           0           8           0    2496     639
     7 P51467     37   141     182    1578          18           1           9           0    1661     333
     8 P51956     40   695     266    2482          14           0           4           0    1176     394
     9 P52630     61   154     205    1231           3           1           9           0     694     434
    10 P54631    102    51     125     882          23           1          85           0     343     416
    11 P60426     23   110     160    2377           8           0          12           0     986     329
    12 P66101    155   357     175    1158          11           1          33           0    1139     436
    13 P68071     24    86     189    1362           9           2          17           0    1527     431
    14 P69489     72   104     129     843          23           0           0           0     545     119
    15 P70597     31   119     192     935          37           3         131           0     579     298
    16 P75656     63   195     226    1140          19           1          45           0     864     320
    17 P84567     49   136     229    2022          16           0          37           1    1369     531
    18 P94346      9  1274     311    1636          10           0           1           0    1193     418
    19 P95158     35    88     233    1712          26           0           7           0    2627     724
    20 P96518     18    68     196    2162          25           1           5           0    2350     539
    # ℹ 298 more variables: `10060` <dbl>, `10061` <dbl>, `100873982` <dbl>, `10096` <dbl>, `10097` <dbl>,
    #   `101` <dbl>, `10120` <dbl>, `10121` <dbl>, `10152` <dbl>, `10157` <dbl>, `102` <dbl>, `10257` <dbl>,
    #   `103` <dbl>, `10347` <dbl>, `10349` <dbl>, `10350` <dbl>, `10351` <dbl>, `104` <dbl>, `10449` <dbl>,
    #   `105` <dbl>, `107` <dbl>, `10863` <dbl>, `10880` <dbl>, `10965` <dbl>, `11033` <dbl>, `11057` <dbl>,
    #   `11086` <dbl>, `11093` <dbl>, `11095` <dbl>, `11096` <dbl>, `11173` <dbl>, `11174` <dbl>,
    #   `11194` <dbl>, `113179` <dbl>, `11332` <dbl>, `116236` <dbl>, `116285` <dbl>, `116983` <dbl>,
    #   `122970` <dbl>, `1238` <dbl>, `123876` <dbl>, `1244` <dbl>, `125981` <dbl>, `127550` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

The built-in database only supports Human, Mouse, Pig, Zebrafish.Other species could utilize OrgDb to convert. More OrgDb is on the  https://bioconductor.org/packages/release/BiocViews.html#___OrgDb

```R
library(org.Hs.eg.db)
MAE |>
	EMP_feature_convert(experiment = 'host_gene',
                      from = 'SYMBOL',to='ENTREZID',OrgDb = org.Hs.eg.db)
```
    Feature changed!
    Current feature: ENTREZID
    # A tibble: 20 × 309
       primary   `1` `100` `10005` `10006` `100128640` `100132116` `100507098` `100526760` `10057` `10058`
       <chr>   <dbl> <dbl>   <dbl>   <dbl>       <dbl>       <dbl>       <dbl>       <dbl>   <dbl>   <dbl>
     1 P11774    104   353     172    1309           2           2          29           0     816     207
     2 P31579     25   145     183    1690          24           0          18           0    2172     607
     3 P33531     20   590     184    2352          18           0           4           0    2236     503
     4 P36987     49   136     168    1831           7           0          23           0    1157     344
     5 P40725     17   120     219    1715          16           0           7           0    1449     431
     6 P40923     27   249     236    1707          17           0           8           0    2496     639
     7 P51467     37   141     182    1578          18           1           9           0    1661     333
     8 P51956     40   695     266    2482          14           0           4           0    1176     394
     9 P52630     61   154     205    1231           3           1           9           0     694     434
    10 P54631    102    51     125     882          23           1          85           0     343     416
    11 P60426     23   110     160    2377           8           0          12           0     986     329
    12 P66101    155   357     175    1158          11           1          33           0    1139     436
    13 P68071     24    86     189    1362           9           2          17           0    1527     431
    14 P69489     72   104     129     843          23           0           0           0     545     119
    15 P70597     31   119     192     935          37           3         131           0     579     298
    16 P75656     63   195     226    1140          19           1          45           0     864     320
    17 P84567     49   136     229    2022          16           0          37           1    1369     531
    18 P94346      9  1274     311    1636          10           0           1           0    1193     418
    19 P95158     35    88     233    1712          26           0           7           0    2627     724
    20 P96518     18    68     196    2162          25           1           5           0    2350     539
    # ℹ 298 more variables: `10060` <dbl>, `10061` <dbl>, `100873982` <dbl>, `10096` <dbl>, `10097` <dbl>,
    #   `101` <dbl>, `10120` <dbl>, `10121` <dbl>, `10152` <dbl>, `10157` <dbl>, `102` <dbl>, `10257` <dbl>,
    #   `103` <dbl>, `10347` <dbl>, `10349` <dbl>, `10350` <dbl>, `10351` <dbl>, `104` <dbl>, `10449` <dbl>,
    #   `105` <dbl>, `107` <dbl>, `10863` <dbl>, `10880` <dbl>, `10965` <dbl>, `11033` <dbl>, `11057` <dbl>,
    #   `11086` <dbl>, `11093` <dbl>, `11095` <dbl>, `11096` <dbl>, `11173` <dbl>, `11174` <dbl>,
    #   `11194` <dbl>, `113179` <dbl>, `11332` <dbl>, `116236` <dbl>, `116285` <dbl>, `116983` <dbl>,
    #   `122970` <dbl>, `1238` <dbl>, `123876` <dbl>, `1244` <dbl>, `125981` <dbl>, `127550` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

2. For compound ID convert

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |> 
  EMP_feature_convert(from = 'KEGG',to='HMDB')
```

    Feature changed!
    Current feature: HMDB
    # A tibble: 20 × 156
       primary HMDB0000044 HMDB0000045 HMDB0000050 HMDB0000054 HMDB0000062 HMDB0000063 HMDB0000064 HMDB0000086
       <chr>         <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>
     1 P11774        8312.       2829.      13681.      73604.     534837.      44341.      58819.      18153.
     2 P31579       16374.       4455.      52320.      36705.     386599.      70195.      55289.       9013.
     3 P33531        7972.       1931.       4607.      39147.     395293.     120531.      54143.      24327.
     4 P36987        5249.        648.       5329.      47638.     328869.      77031.      23400.      10924.
     5 P40725        2862.       7720.      31434.      66494.     457751.      53195.      19021.      17065.
     6 P40923        5931.      21170.      22479.      48830.     402642.      76685.      52618.      11527.
     7 P51467        3495.       6158.      24623.      31065.     465288.      80604.      36012.      16964.
     8 P51956        7839.      50742.      43385.      85064.     426901.      10398.      19274.      17974.
     9 P52630       10666.      37824.      49717.      73837.     378795.      46820.      23161.      15082.
    10 P54631        6208.      26915.      60767.      82448.     567961.      63562.      43942.      16620.
    11 P60426       21521.        907.       4365.      68293.     340561.      87363.      54173.      13137.
    12 P66101        5217.      15791.      28656.      28158.     482112.      86366.      59158.      17455.
    13 P68071        2073.      59918.      54820.      42314.     361882.      53941.      22761.      17956.
    14 P69489        4013.       6731.      12290.      80134.     207098.      58653.      22664.       5865.
    15 P70597        4402.      11586.      17126.      62476.     455030.      35494.     103808.      12490.
    16 P75656        1893.      13741.      34612.      61186.     280577.      52536.      20290.      11389.
    17 P84567        2706.      13948.      52228.      71937.     345109.      63856.      16603.      11778.
    18 P94346        3472.       6905.      18784.     107870.     514065.      71083.      40752.       7959.
    19 P95158        4539.       4275.       9433.      97749.     453433.      98261.      30669.      15013.
    20 P96518       20780.      17569.      78337.      35443.     314801.      69174.      19556.      10258.
    # ℹ 147 more variables: HMDB0000094 <dbl>, HMDB0000097 <dbl>, HMDB0000125 <dbl>, HMDB0000132 <dbl>,
    #   HMDB0000133 <dbl>, HMDB0000138 <dbl>, HMDB0000148 <dbl>, HMDB0000157 <dbl>, HMDB0000158 <dbl>,
    #   HMDB0000159 <dbl>, HMDB0000162 <dbl>, HMDB0000167 <dbl>, HMDB0000172 <dbl>, HMDB0000177 <dbl>,
    #   HMDB0000182 <dbl>, HMDB0000195 <dbl>, HMDB0000197 <dbl>, HMDB0000201 <dbl>, HMDB0000207 <dbl>,
    #   HMDB0000220 <dbl>, HMDB0000222 <dbl>, HMDB0000224 <dbl>, HMDB0000228 <dbl>, HMDB0000243 <dbl>,
    #   HMDB0000244 <dbl>, HMDB0000248 <dbl>, HMDB0000251 <dbl>, HMDB0000259 <dbl>, HMDB0000263 <dbl>,
    #   HMDB0000267 <dbl>, HMDB0000269 <dbl>, HMDB0000277 <dbl>, HMDB0000289 <dbl>, HMDB0000294 <dbl>, …
    # ℹ Use `colnames()` to see all variable names


#### EMP_collapse

merge assay data accoding to duplicate coldata.

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',collapse_by='col',
               estimate_group = 'Group',method = 'mean',collapse_sep = '+') 
```

    Primary changed!
    Current primary: Group
    # A tibble: 2 × 711
      primary `neg-M103T139` `neg-M107T179` `neg-M109T160` `neg-M121T165` `neg-M124T53` `neg-M124T82`
      <chr>            <dbl>          <dbl>          <dbl>          <dbl>         <dbl>         <dbl>
    1 Group_A         39150.         12918.          1855.         19756.        14589.         7424.
    2 Group_B         38593.         15305.          2398.         20058.        14789.         8001.
    # ℹ 704 more variables: `neg-M127T49` <dbl>, `neg-M128T56` <dbl>, `neg-M128T94` <dbl>,
    #   `neg-M129T177_2` <dbl>, `neg-M131T175` <dbl>, `neg-M135T84` <dbl>, `neg-M137T165` <dbl>,
    #   `neg-M137T198` <dbl>, `neg-M138T200` <dbl>, `neg-M140T82` <dbl>, `neg-M142T151` <dbl>,
    #   `neg-M146T50` <dbl>, `neg-M149T332` <dbl>, `neg-M149T393` <dbl>, `neg-M151T176` <dbl>,
    #   `neg-M154T47` <dbl>, `neg-M159T163` <dbl>, `neg-M159T206` <dbl>, `neg-M163T113` <dbl>,
    #   `neg-M165T165` <dbl>, `neg-M165T172` <dbl>, `neg-M167T104` <dbl>, `neg-M167T137` <dbl>,
    #   `neg-M167T53_2` <dbl>, `neg-M167T82_2` <dbl>, `neg-M173T163` <dbl>, `neg-M173T174` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

merge assay data accoding to duplicate rowdata.

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',
               collapse_by='row',na_string = c("NA", "null", "","-"),
               estimate_group = 'MS2kegg',method = 'mean',collapse_sep = '+') 
```
    Feature changed!
    Current feature: MS2kegg
    # A tibble: 20 × 193
       primary C00020 C00022 C00025 C00047 C00051 C00062 C00064 C00072 C00073 C00074  C00078   C00079  C00082
       <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>   <dbl>    <dbl>   <dbl>
     1 P11774   1415.  3795.  4236. 12149.  2239. 59061. 21173.  4156. 11413. 20678. 541915. 1223794.  91879.
     2 P31579   2227.  3832.  4050. 10506.  1934. 35926. 17334.  8187. 10395. 15518. 505144. 1128146.  70890.
     3 P33531    966.  3145.  2992. 10510.  3408. 59354. 17487.  3986. 12350. 18519. 523381. 1158779.  65998.
     4 P36987    324.  4183.  5733. 10461.  1541. 43060. 13803.  2624. 13584.  8587. 491480. 1096061.  88491.
     5 P40725   3860.  6964.  6399. 10737.  1860. 38626. 18778.  1431.  9352. 11043. 404328.  892090.  48350.
     6 P40923  10585.  5793.  4923.  9659.  3047. 53740. 20079.  2966. 10335. 12735. 408912.  959190.  54829.
     7 P51467   3079.  4969.  3068. 12787.  3447. 54442. 19200.  1747. 12293.  7972. 509996. 1177841.  79512.
     8 P51956  25371.  4440.  4962. 12709.  2827. 49674. 17476.  3920. 13579. 21248. 464563. 1039763.  65581.
     9 P52630  18912.  4181.  3478. 10147.  2879. 52478. 19740.  5333.  9692. 22683. 414831. 1008052.  48300.
    10 P54631  13458.  5175.  4890. 14991.  3230. 49776. 20910.  3104. 14971. 19855. 506225. 1237135.  73692.
    11 P60426    454.  3386.  2279.  8223.  2607. 43362. 15493. 10760. 10593. 16501. 480899.  713110.  61110.
    12 P66101   7896.  6787.  3634. 15846.  3276. 56558. 22646.  2608. 12632. 13185. 425531. 1156224.  75003.
    13 P68071  29959. 10392.  7918.  8997.  4449. 55901. 18802.  1036. 12133. 12854. 552633. 1139685.  54882.
    14 P69489   3365.  2453.  2031. 10045.  3632. 55539. 13736.  2007. 10491. 11197. 418425.  940845.  56897.
    15 P70597   5793.  5961.  6859. 10381.  2767. 49030. 17924.  2201. 12535.  5113. 572520. 1160175. 104174.
    16 P75656   6870.  3676.  4561.  9445.  1642. 37131. 15911.   946.  9419. 13083. 414705.  909463.  53954.
    17 P84567   6974.  7851.  5728.  8317.  6927. 37661. 19135.  1353. 10799. 12491. 547850. 1230595.  67497.
    18 P94346   3452.  2180.  5329. 13437.  4401. 44692. 20312.  1736. 12129. 13754. 581643. 1084619.  62743.
    19 P95158   2138.  6259.  6817. 12396.  2756. 52168. 17661.  2269. 12140.  6010. 484683. 1321838.  93646.
    20 P96518   8785.  2350.  2830.  6637.  2232. 41909. 10865. 10390.  7046. 20289. 424039.  935910.  44605.
    # ℹ 179 more variables: C00084 <dbl>, C00086 <dbl>, C00090 <dbl>, C00106 <dbl>, C00114 <dbl>,
    #   C00133 <dbl>, C00135 <dbl>, C00144 <dbl>, C00146 <dbl>, C00148 <dbl>, C00156 <dbl>, C00157 <dbl>,
    #   C00158 <dbl>, C00180 <dbl>, C00183 <dbl>, C00188 <dbl>, C00195 <dbl>, C00207 <dbl>, C00212 <dbl>,
    #   C00219 <dbl>, C00242 <dbl>, C00245 <dbl>, C00249 <dbl>, C00255 <dbl>, C00262 <dbl>, C00294 <dbl>,
    #   C00299 <dbl>, C00300 <dbl>, C00318 <dbl>, C00327 <dbl>, C00328 <dbl>, C00346 <dbl>, C00350 <dbl>,
    #   C00362 <dbl>, C00366 <dbl>, C00387 <dbl>, C00407 <dbl>, C00416 <dbl>, C00422 <dbl>, C00463 <dbl>,
    #   C00472 <dbl>, C00486 <dbl>, C00499 <dbl>, C00500 <dbl>, C00548 <dbl>, C00550 <dbl>, C00588 <dbl>, …
    # ℹ Use `colnames()` to see all variable names


combie two collapse method

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',
               collapse_by='row',na_string = c("NA", "null", "","-"),
               estimate_group = 'MS2kegg',method = 'mean',collapse_sep = '+') |>
  EMP_collapse(collapse_by='col',estimate_group = 'Group',
               method = 'mean',collapse_sep = '+')
```
    Primary changed!
    Current primary: Group
    # A tibble: 2 × 193
      primary C00020 C00022 C00025 C00047 C00051 C00062 C00064 C00072 C00073 C00074 C00078 C00079 C00082 C00084
      <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    1 Group_A  7637.  4512.  4583. 11187.  2604. 45390. 18038.  3933. 10956. 15073. 4.58e5 1.04e6 62086. 1.17e5
    2 Group_B  7951.  5265.  4689. 10651.  3506. 51619. 17809.  3344. 11833. 13258. 5.10e5 1.11e6 74118. 1.15e5
    # ℹ 178 more variables: C00086 <dbl>, C00090 <dbl>, C00106 <dbl>, C00114 <dbl>, C00133 <dbl>,
    #   C00135 <dbl>, C00144 <dbl>, C00146 <dbl>, C00148 <dbl>, C00156 <dbl>, C00157 <dbl>, C00158 <dbl>,
    #   C00180 <dbl>, C00183 <dbl>, C00188 <dbl>, C00195 <dbl>, C00207 <dbl>, C00212 <dbl>, C00219 <dbl>,
    #   C00242 <dbl>, C00245 <dbl>, C00249 <dbl>, C00255 <dbl>, C00262 <dbl>, C00294 <dbl>, C00299 <dbl>,
    #   C00300 <dbl>, C00318 <dbl>, C00327 <dbl>, C00328 <dbl>, C00346 <dbl>, C00350 <dbl>, C00362 <dbl>,
    #   C00366 <dbl>, C00387 <dbl>, C00407 <dbl>, C00416 <dbl>, C00422 <dbl>, C00463 <dbl>, C00472 <dbl>,
    #   C00486 <dbl>, C00499 <dbl>, C00500 <dbl>, C00548 <dbl>, C00550 <dbl>, C00588 <dbl>, C00633 <dbl>, …
    # ℹ Use `colnames()` to see all variable names


#### EMP_decostand

Transfer data into relative format.

```R
MAE |>
  EMP_decostand(experiment = 'taxonomy',method = 'relative') 
```

    # A tibble: 20 × 444
       primary Archaea;Candidatus_Thermo…¹ Archaea;Euryarchaeot…² Bacteria;Actinobacte…³ Bacteria;Actinobacte…⁴
       <chr>                         <dbl>                  <dbl>                  <dbl>                  <dbl>
     1 P11774                     0                       0                     0                      0       
     2 P31579                     0                       0                     0                      0.000500
     3 P33531                     0                       0                     0                      0.0004  
     4 P36987                     0                       0                     0                      0.000300
     5 P40725                     0                       0                     0                      0       
     6 P40923                     0                       0                     0                      0.000701
     7 P51467                     0                       0                     0                      0       
     8 P51956                     0                       0.00430               0                      0       
     9 P52630                     0                       0                     0                      0       
    10 P54631                     0                       0                     0                      0       
    11 P60426                     0                       0                     0                      0.000400
    12 P66101                     0                       0                     0                      0       
    13 P68071                     0                       0                     0                      0.0005  
    14 P69489                     0                       0.00180               0                      0       
    15 P70597                     0                       0                     0                      0       
    16 P75656                     0                       0                     0                      0       
    17 P84567                     0                       0                     0                      0       
    18 P94346                     0                       0                     0.000300               0.00140 
    19 P95158                     0.000200                0                     0                      0.000400
    20 P96518                     0                       0                     0                      0.000200
    # ℹ abbreviated names:
    #   ¹`Archaea;Candidatus_Thermoplasmatota;Thermoplasmata;Methanomassiliicoccales;Methanomassiliicoccaceae;Methanomassiliicoccus;Candidatus_Methanomassiliicoccus_intestinalis;`,
    #   ²`Archaea;Euryarchaeota;Methanobacteria;Methanobacteriales;Methanobacteriaceae;Methanobrevibacter;Methanobrevibacter_smithii;`,
    #   ³`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_meyeri;`,
    #   ⁴`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_odontolytica;`
    # ℹ 439 more variables:
    #   `Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_adolescentis;` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

Transfer data into centered log ratio ("clr") format.

```R
MAE |>
  EMP_decostand(experiment = 'geno_ko',method = 'clr',pseudocount=0.0001)
```

    # A tibble: 20 × 7,630
       primary K00001 K00002 K00003 K00004 K00005 K00007 K00008 K00009 K00010  K00011 K00012 K00013 K00014
       <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>   <dbl>  <dbl>  <dbl>  <dbl>
     1 P11774    5.58  1.49    6.47   1.49   4.29 -8.41    6.05   6.01   3.51 -8.41     6.50   6.96   6.92
     2 P31579    4.66  0.805   5.32   2.60   3.80  1.90    5.69   5.36   2.19  1.50     6.36   6.00   6.54
     3 P33531    4.55  2.07    6.31   1.66   4.30 -8.24    6.53   5.98   2.92 -8.24     6.39   6.94   6.86
     4 P36987    5.02  2.03    6.59   2.72   5.02 -7.88    6.87   6.99   4.10 -7.88     7.62   7.42   7.16
     5 P40725    4.98  2.59    5.39   2.32   3.05 -9.45    5.25   5.05   2.47 -0.241    5.44   5.36   5.57
     6 P40923    5.50  0.941   6.04   2.55   4.24  0.248   5.54   5.50   3.54 -8.96     5.91   6.05   6.15
     7 P51467    5.70  2.24    5.42   2.82   4.10 -9.17    5.78   5.60   3.63  2.68     5.15   5.92   5.91
     8 P51956    5.37  1.85    5.37   1.56   3.33  1.85    5.28   4.97   1.72  0.870    5.58   5.87   5.87
     9 P52630    5.93  2.60    6.68   3.29   4.85 -8.41    6.18   5.97   4.24  1.50     6.48   6.62   6.88
    10 P54631    4.97  1.84    5.52   1.15   4.30  2.25    5.41   5.02   2.45  1.15     5.49   5.94   6.06
    11 P60426    4.70  0.453   5.38   1.37   3.50 -0.240   5.23   5.01   2.32  0.858    5.52   5.65   5.67
    12 P66101    5.28  2.77    5.84   3.34   4.02 -9.00    5.93   5.50   3.47 -9.00     5.75   6.00   6.20
    13 P68071    6.46  2.30    8.24   2.30   6.94 -6.91    8.41   7.47   6.93 -6.91     8.24   8.27   8.16
    14 P69489    4.86  2.17    5.39   1.54   4.28  0.849   5.15   4.80   3.73 -0.537    4.76   5.49   5.38
    15 P70597    5.37  2.74    6.08   3.06   4.67 -8.55    6.10   5.65   3.06  0.658    5.94   6.55   6.60
    16 P75656    4.63  1.52    5.71   2.21   3.54  1.23    6.26   6.29   3.39  0.828    5.77   6.20   6.03
    17 P84567    4.95  2.35    5.99   3.39   4.53 -8.65    6.31   6.23   2.95 -8.65     6.15   6.39   6.42
    18 P94346    5.08  2.44    5.70   2.34   3.56 -9.17    5.71   5.49   3.17  0.0375   6.13   6.24   6.19
    19 P95158    5.06  2.25    6.05   2.76   4.22 -8.76    6.62   5.40   2.06  0.455    6.05   6.19   6.52
    20 P96518    5.11  0.904   6.35   3.47   4.24 -8.31    6.25   6.66   3.54 -8.31     6.24   6.70   6.52
    # ℹ 7,616 more variables: K00015 <dbl>, K00016 <dbl>, K00018 <dbl>, K00020 <dbl>, K00021 <dbl>,
    #   K00024 <dbl>, K00026 <dbl>, K00027 <dbl>, K00029 <dbl>, K00030 <dbl>, K00031 <dbl>, K00032 <dbl>,
    #   K00033 <dbl>, K00034 <dbl>, K00036 <dbl>, K00038 <dbl>, K00039 <dbl>, K00040 <dbl>, K00041 <dbl>,
    #   K00042 <dbl>, K00043 <dbl>, K00045 <dbl>, K00048 <dbl>, K00052 <dbl>, K00053 <dbl>, K00054 <dbl>,
    #   K00055 <dbl>, K00057 <dbl>, K00058 <dbl>, K00059 <dbl>, K00060 <dbl>, K00064 <dbl>, K00066 <dbl>,
    #   K00067 <dbl>, K00069 <dbl>, K00073 <dbl>, K00074 <dbl>, K00075 <dbl>, K00077 <dbl>, K00082 <dbl>,
    #   K00086 <dbl>, K00087 <dbl>, K00088 <dbl>, K00090 <dbl>, K00091 <dbl>, K00094 <dbl>, K00096 <dbl>, …
    # ℹ Use `colnames()` to see all variable names

Transfer data into logformat.

```R
MAE |>
  EMP_decostand(experiment = 'geno_ec',method = 'log',logbase = 2) 
```

    # A tibble: 20 × 2,425
       primary `1.1.1.1` `1.1.1.100` `1.1.1.102` `1.1.1.103` `1.1.1.105` `1.1.1.108` `1.1.1.11` `1.1.1.122`
       <chr>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>      <dbl>       <dbl>
     1 P11774       9.69        4.17           0        4.17        0           3.32       0           0   
     2 P31579       9.35        2.58           0        7.17        0           3          3.58        3.32
     3 P33531       9.15        4.32           0        6.46        0           4.32       0           0   
     4 P36987       8.35        2.58           0        7.61        0           0          0           0   
     5 P40725       9.76        4              0        6.09        0           1          0           0   
     6 P40923       9.96        2.58           0        5           0           0          1           0   
     7 P51467      10.2         2              0        6.55        0           1          0           0   
     8 P51956      10.1         5.17           0        5           0           2          4           0   
     9 P52630       9.90        4.17           0        4.32        2.58        1          0           0   
    10 P54631       9.36        4.32           0        4.46        0           3.32       4.17        0   
    11 P60426       9.75        3              0        5.64        0           2.58       1           0   
    12 P66101       9.70        4.32           1        5.25        0           2.58       0           0   
    13 P68071       8.98        2              0        7.17        0           3.81       0           0   
    14 P69489       9.97        4.46           0        5.17        0           1          3           0   
    15 P70597       9.23        3.81           0        5.25        0           4.81       0           0   
    16 P75656       9.16        3.58           0        8.52        0           0          2.58        0   
    17 P84567       8.77        4.58           0        7.54        0           0          0           0   
    18 P94346       9.67        3              0        5.09        0           2          0           0   
    19 P95158       9.56        5              0        5           0           1          0           0   
    20 P96518       8.84        2              0        7.46        2.58        0          0           0   
    # ℹ 2,416 more variables: `1.1.1.130` <dbl>, `1.1.1.132` <dbl>, `1.1.1.133` <dbl>, `1.1.1.135` <dbl>,
    #   `1.1.1.136` <dbl>, `1.1.1.137` <dbl>, `1.1.1.14` <dbl>, `1.1.1.141` <dbl>, `1.1.1.157` <dbl>,
    #   `1.1.1.169` <dbl>, `1.1.1.17` <dbl>, `1.1.1.18` <dbl>, `1.1.1.193` <dbl>, `1.1.1.2` <dbl>,
    #   `1.1.1.202` <dbl>, `1.1.1.205` <dbl>, `1.1.1.21` <dbl>, `1.1.1.215` <dbl>, `1.1.1.219` <dbl>,
    #   `1.1.1.22` <dbl>, `1.1.1.23` <dbl>, `1.1.1.25` <dbl>, `1.1.1.251` <dbl>, `1.1.1.26` <dbl>,
    #   `1.1.1.261` <dbl>, `1.1.1.262` <dbl>, `1.1.1.264` <dbl>, `1.1.1.267` <dbl>, `1.1.1.27` <dbl>,
    #   `1.1.1.270` <dbl>, `1.1.1.271` <dbl>, `1.1.1.276` <dbl>, `1.1.1.28` <dbl>, `1.1.1.281` <dbl>, …
    # ℹ Use `colnames()` to see all variable names


#### EMP_impute

1. For coldata

```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_impute(assay=F,coldata=T,rowdata=F)
```

    # A tibble: 20 × 2,425
       primary `1.1.1.1` `1.1.1.100` `1.1.1.102` `1.1.1.103` `1.1.1.105` `1.1.1.108` `1.1.1.11` `1.1.1.122`
       <chr>       <int>       <int>       <int>       <int>       <int>       <int>      <int>       <int>
     1 P11774        412           9           0           9           0           5          0           0
     2 P31579        327           3           0          72           0           4          6           5
     3 P33531        285          10           0          44           0          10          0           0
     4 P36987        163           3           0          98           0           0          0           0
     5 P40725        433           8           0          34           0           1          0           0
     6 P40923        497           3           0          16           0           0          1           0
     7 P51467        599           2           0          47           0           1          0           0
     8 P51956        534          18           0          16           0           2          8           0
     9 P52630        478           9           0          10           3           1          0           0
    10 P54631        329          10           0          11           0           5          9           0
    11 P60426        430           4           0          25           0           3          1           0
    12 P66101        415          10           1          19           0           3          0           0
    13 P68071        252           2           0          72           0           7          0           0
    14 P69489        501          11           0          18           0           1          4           0
    15 P70597        300           7           0          19           0          14          0           0
    16 P75656        286           6           0         184           0           0          3           0
    17 P84567        219          12           0          93           0           0          0           0
    18 P94346        408           4           0          17           0           2          0           0
    19 P95158        377          16           0          16           0           1          0           0
    20 P96518        229           2           0          88           3           0          0           0
    # ℹ 2,416 more variables: `1.1.1.130` <int>, `1.1.1.132` <int>, `1.1.1.133` <int>, `1.1.1.135` <int>,
    #   `1.1.1.136` <int>, `1.1.1.137` <int>, `1.1.1.14` <int>, `1.1.1.141` <int>, `1.1.1.157` <int>,
    #   `1.1.1.169` <int>, `1.1.1.17` <int>, `1.1.1.18` <int>, `1.1.1.193` <int>, `1.1.1.2` <int>,
    #   `1.1.1.202` <int>, `1.1.1.205` <int>, `1.1.1.21` <int>, `1.1.1.215` <int>, `1.1.1.219` <int>,
    #   `1.1.1.22` <int>, `1.1.1.23` <int>, `1.1.1.25` <int>, `1.1.1.251` <int>, `1.1.1.26` <int>,
    #   `1.1.1.261` <int>, `1.1.1.262` <int>, `1.1.1.264` <int>, `1.1.1.267` <int>, `1.1.1.27` <int>,
    #   `1.1.1.270` <int>, `1.1.1.271` <int>, `1.1.1.276` <int>, `1.1.1.28` <int>, `1.1.1.281` <int>, …
    # ℹ Use `colnames()` to see all variable names

Support formula, such as only impute SAS and SDS

```R
MAE |>
  EMP_coldata_extract(action ='add') |>
  EMP_impute(.formula = SAS+SDS ~ .) 
```

    # A tibble: 20 × 12
       primary   Age Height Weight   BMI Education_Years  PHQ9  GAD7   SAS   SDS  HAMA  HAMD
       <chr>   <dbl>  <dbl>  <dbl> <dbl>           <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
     1 P31579     26   1.7    58    20.1              16     4     2    31    40     0     0
     2 P66101     24   1.73   66    22.0              18     0     0    44    34     2     1
     3 P75656     27   1.82   88    26.6              17    10     5    36    49     0     0
     4 P96518     25   1.6    50    19.5              16     4     5    NA    NA    NA    NA
     5 P40923     26   1.82   85    25.7              18     4     3    35    38     1     2
     6 P36987     26   1.55   45    18.7              18     4     6    46    36     2     1
     7 P40725     27   1.57   47    19.1              17     3     3    45    31    NA    NA
     8 P52630     30   1.78   70    22.1              19     0     1    41    40     0     0
     9 P94346     27   1.67   48    17.2              19     1     0    35    33     0     1
    10 P54631     60   1.53   49    20.9              12    11    NA    39    39     8     8
    11 P70597     40   1.7    62    21.4              16    12    17    54    60     3     6
    12 P51467     56   1.5    49    21.8               9    NA    NA    44    44     7     8
    13 P60426     39   1.6    59    23.0               9     5     3    53    44     4     4
    14 P68071     25   1.72   58    19.6              16     3     0    29    31     4     3
    15 P69489     34   1.56   55.8  22.9              16     4     4    35    45     3     3
    16 P95158     40   1.68   52    18.4              19    13    12    46    40    11    11
    17 P84567     38   1.62   62    23.6              16     4     6    33    44     7     6
    18 P33531     36   1.73   56    18.7              16     3     0    29    34     5     2
    19 P51956     28   1.72   60    20.3              12     6     7    49    54     9     9
    20 P11774     29   1.5    50    22.2              21    11     7    31    33     1     1

2. For assay

```R
MAE |>
  EMP_coldata_extract(action ='add') |>
  EMP_impute(.formula = SAS+SDS ~ .) 
```

    # A tibble: 20 × 12
       primary   Age Height Weight   BMI Education_Years  PHQ9  GAD7   SAS   SDS  HAMA  HAMD
       <chr>   <dbl>  <dbl>  <dbl> <dbl>           <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
     1 P31579     26   1.7    58    20.1              16     4     2    31    40     0     0
     2 P66101     24   1.73   66    22.0              18     0     0    44    34     2     1
     3 P75656     27   1.82   88    26.6              17    10     5    36    49     0     0
     4 P96518     25   1.6    50    19.5              16     4     5    NA    NA    NA    NA
     5 P40923     26   1.82   85    25.7              18     4     3    35    38     1     2
     6 P36987     26   1.55   45    18.7              18     4     6    46    36     2     1
     7 P40725     27   1.57   47    19.1              17     3     3    45    31    NA    NA
     8 P52630     30   1.78   70    22.1              19     0     1    41    40     0     0
     9 P94346     27   1.67   48    17.2              19     1     0    35    33     0     1
    10 P54631     60   1.53   49    20.9              12    11    NA    39    39     8     8
    11 P70597     40   1.7    62    21.4              16    12    17    54    60     3     6
    12 P51467     56   1.5    49    21.8               9    NA    NA    44    44     7     8
    13 P60426     39   1.6    59    23.0               9     5     3    53    44     4     4
    14 P68071     25   1.72   58    19.6              16     3     0    29    31     4     3
    15 P69489     34   1.56   55.8  22.9              16     4     4    35    45     3     3
    16 P95158     40   1.68   52    18.4              19    13    12    46    40    11    11
    17 P84567     38   1.62   62    23.6              16     4     6    33    44     7     6
    18 P33531     36   1.73   56    18.7              16     3     0    29    34     5     2
    19 P51956     28   1.72   60    20.3              12     6     7    49    54     9     9
    20 P11774     29   1.5    50    22.2              21    11     7    31    33     1     1

3. For rowdata (Not Not recommended)

```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_impute(assay=F,coldata=F,rowdata=T)
```


#### EMP_identify_assay

1. Taking into account the specifics of microbial data, we introduce two parameters: 'minnum' for the minimum relative abundance and 'min ratio' for the minimum occurrence ratio of core species within a group. Initially, any abundance below the specified threshold is converted to 0 based on the minimum species relative abundance. Subsequently, the core species are required to meet the condition that their occurrence rate within at least one group exceeds the preset threshold. Any remaining species are categorized as 'rare species' and filtered out.
   
   Note: If absolute abundance is provided as input, it will be automatically converted to relative abundance for filtering purposes during calculations. However, the output will remain in absolute abundance.

```R
MAE |>
  EMP_assay_extract('taxonomy') |>
  EMP_identify_assay(estimate_group = 'Group',method = 'default',
                     min=0.01,min_ratio = 0.7) ### consider the Group
```
    0 of 20 samples were filterd out!
    429 of 443 features were filterd out!
    # A tibble: 20 × 15
       primary Bacteria;Bacteroidetes;Ba…¹ Bacteria;Bacteroidet…² Bacteria;Bacteroidet…³ Bacteria;Bacteroidet…⁴
       <chr>                         <int>                  <int>                  <int>                  <int>
     1 P11774                          215                    295                    819                    633
     2 P31579                          133                    845                    164                    718
     3 P33531                          162                    285                    117                    296
     4 P36987                          119                    274                    117                    110
     5 P40725                           66                    101                     72                    264
     6 P40923                          126                    228                    136                    129
     7 P51467                           97                     85                    122                    115
     8 P51956                           91                     87                    111                    146
     9 P52630                          333                    120                    293                    320
    10 P54631                           54                    521                   1689                    300
    11 P60426                          100                    166                    301                    112
    12 P66101                          304                    894                   2168                   1287
    13 P68071                          416                    201                     74                    217
    14 P69489                          144                    100                    122                    130
    15 P70597                          212                    252                    113                     62
    16 P75656                          149                    139                    236                    458
    17 P84567                           18                     73                    995                    349
    18 P94346                            9                    433                     87                     10
    19 P95158                           72                    273                    378                    453
    20 P96518                          113                    146                    291                    174
    # ℹ abbreviated names:
    #   ¹`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_cellulosilyticus;`,
    #   ²`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_fragilis;`,
    #   ³`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_ovatus;`,
    #   ⁴`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_thetaiotaomicron;`
    # ℹ 10 more variables:
    #   `Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_uniformis;` <int>, …

```R
MAE |>
  EMP_assay_extract('taxonomy') |>
  EMP_identify_assay(method = 'default') ### consider all samples belong to one group
```

    No group or design set. Assuming all samples belong to one group.
    0 of 20 samples were filterd out!
    384 of 443 features were filterd out!
    # A tibble: 20 × 60
       primary Bacteria;Bacteroidetes;Ba…¹ Bacteria;Bacteroidet…² Bacteria;Bacteroidet…³ Bacteria;Bacteroidet…⁴
       <chr>                         <int>                  <int>                  <int>                  <int>
     1 P11774                           63                     59                    215                    295
     2 P31579                           36                     44                    133                    845
     3 P33531                          104                     53                    162                    285
     4 P36987                          251                     22                    119                    274
     5 P40725                           91                     71                     66                    101
     6 P40923                          140                     57                    126                    228
     7 P51467                          285                     47                     97                     85
     8 P51956                           60                     49                     91                     87
     9 P52630                           18                     71                    333                    120
    10 P54631                           15                     38                     54                    521
    11 P60426                           92                     37                    100                    166
    12 P66101                           73                     86                    304                    894
    13 P68071                          147                     60                    416                    201
    14 P69489                          127                     48                    144                    100
    15 P70597                           43                     78                    212                    252
    16 P75656                          268                     40                    149                    139
    17 P84567                          621                     30                     18                     73
    18 P94346                            6                      4                      9                    433
    19 P95158                           46                     40                     72                    273
    20 P96518                           32                     57                    113                    146
    # ℹ abbreviated names:
    #   ¹`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_caccae;`,
    #   ²`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_caecimuris;`,
    #   ³`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_cellulosilyticus;`,
    #   ⁴`Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_fragilis;`
    # ℹ 55 more variables:
    #   `Bacteria;Bacteroidetes;Bacteroidia;Bacteroidales;Bacteroidaceae;Bacteroides;Bacteroides_helcogenes;` <int>, …

2. Consider the minnum counts abundance and min ratio specially for expression data, according to edgeR::filterByExpr.

```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_identify_assay(method = 'edgeR',min = 10,min_ratio = 0.7,estimate_group = 'Group') 
```

    0 of 20 samples were filterd out!
    1123 of 2424 features were filterd out!
    # A tibble: 20 × 1,302
       primary `1.1.1.1` `1.1.1.103` `1.1.1.133` `1.1.1.14` `1.1.1.157` `1.1.1.169` `1.1.1.17` `1.1.1.18`
       <chr>       <int>       <int>       <int>      <int>       <int>       <int>      <int>      <int>
     1 P11774        412           9         281        191          33         264        184         15
     2 P31579        327          72         314        265          42         250        191          8
     3 P33531        285          44         466        259         161         242        150          7
     4 P36987        163          98         221        254          35          94        287         16
     5 P40725        433          34         233        242          57         154        199         15
     6 P40923        497          16         256        198          96         231        190         27
     7 P51467        599          47         272        310          63         287        259         36
     8 P51956        534          16         332        248          56         327        181          7
     9 P52630        478          10         280        216          92         225        176         31
    10 P54631        329          11         329        213          42         223        143         11
    11 P60426        430          25         260        237         103         289        191         13
    12 P66101        415          19         273        304          83         258        198         26
    13 P68071        252          72         295        451          19         311        176        102
    14 P69489        501          18         357        294          69         374        207         71
    15 P70597        300          19         281        231          67         245        147         11
    16 P75656        286         184         256        458          61         128        473         26
    17 P84567        219          93         377        316          77         122        290         11
    18 P94346        408          17         324        290          52         242        233         23
    19 P95158        377          16         290        477          82         211        140          5
    20 P96518        229          88         206        209          57          97        317         14
    # ℹ 1,293 more variables: `1.1.1.193` <int>, `1.1.1.202` <int>, `1.1.1.205` <int>, `1.1.1.219` <int>,
    #   `1.1.1.22` <int>, `1.1.1.23` <int>, `1.1.1.25` <int>, `1.1.1.26` <int>, `1.1.1.261` <int>,
    #   `1.1.1.262` <int>, `1.1.1.267` <int>, `1.1.1.27` <int>, `1.1.1.271` <int>, `1.1.1.276` <int>,
    #   `1.1.1.28` <int>, `1.1.1.281` <int>, `1.1.1.282` <int>, `1.1.1.289` <int>, `1.1.1.29` <int>,
    #   `1.1.1.290` <int>, `1.1.1.3` <int>, `1.1.1.303` <int>, `1.1.1.308` <int>, `1.1.1.31` <int>,
    #   `1.1.1.316` <int>, `1.1.1.336` <int>, `1.1.1.343` <int>, `1.1.1.346` <int>, `1.1.1.350` <int>,
    #   `1.1.1.363` <int>, `1.1.1.367` <int>, `1.1.1.369` <int>, `1.1.1.37` <int>, `1.1.1.371` <int>, …
    # ℹ Use `colnames()` to see all variable names


#### EMP_modify_assay

For some special cases, to modify the assay data and EMP_identify_assay works better in most cases.

1. Change the expression value which is 0 into 0.0001

```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_modify_assay('==0',pseudocount=0.0001)
```
    # A tibble: 20 × 2,425
       primary `1.1.1.1` `1.1.1.100` `1.1.1.102` `1.1.1.103` `1.1.1.105` `1.1.1.108` `1.1.1.11` `1.1.1.122`
       <chr>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>      <dbl>       <dbl>
     1 P11774        412           9      0.0001           9      0.0001      5          0.0001      0.0001
     2 P31579        327           3      0.0001          72      0.0001      4          6           5     
     3 P33531        285          10      0.0001          44      0.0001     10          0.0001      0.0001
     4 P36987        163           3      0.0001          98      0.0001      0.0001     0.0001      0.0001
     5 P40725        433           8      0.0001          34      0.0001      1          0.0001      0.0001
     6 P40923        497           3      0.0001          16      0.0001      0.0001     1           0.0001
     7 P51467        599           2      0.0001          47      0.0001      1          0.0001      0.0001
     8 P51956        534          18      0.0001          16      0.0001      2          8           0.0001
     9 P52630        478           9      0.0001          10      3           1          0.0001      0.0001
    10 P54631        329          10      0.0001          11      0.0001      5          9           0.0001
    11 P60426        430           4      0.0001          25      0.0001      3          1           0.0001
    12 P66101        415          10      1               19      0.0001      3          0.0001      0.0001
    13 P68071        252           2      0.0001          72      0.0001      7          0.0001      0.0001
    14 P69489        501          11      0.0001          18      0.0001      1          4           0.0001
    15 P70597        300           7      0.0001          19      0.0001     14          0.0001      0.0001
    16 P75656        286           6      0.0001         184      0.0001      0.0001     3           0.0001
    17 P84567        219          12      0.0001          93      0.0001      0.0001     0.0001      0.0001
    18 P94346        408           4      0.0001          17      0.0001      2          0.0001      0.0001
    19 P95158        377          16      0.0001          16      0.0001      1          0.0001      0.0001
    20 P96518        229           2      0.0001          88      3           0.0001     0.0001      0.0001
    # ℹ 2,416 more variables: `1.1.1.130` <dbl>, `1.1.1.132` <dbl>, `1.1.1.133` <dbl>, `1.1.1.135` <dbl>,
    #   `1.1.1.136` <dbl>, `1.1.1.137` <dbl>, `1.1.1.14` <dbl>, `1.1.1.141` <dbl>, `1.1.1.157` <dbl>,
    #   `1.1.1.169` <dbl>, `1.1.1.17` <dbl>, `1.1.1.18` <dbl>, `1.1.1.193` <dbl>, `1.1.1.2` <dbl>,
    #   `1.1.1.202` <dbl>, `1.1.1.205` <dbl>, `1.1.1.21` <dbl>, `1.1.1.215` <dbl>, `1.1.1.219` <dbl>,
    #   `1.1.1.22` <dbl>, `1.1.1.23` <dbl>, `1.1.1.25` <dbl>, `1.1.1.251` <dbl>, `1.1.1.26` <dbl>,
    #   `1.1.1.261` <dbl>, `1.1.1.262` <dbl>, `1.1.1.264` <dbl>, `1.1.1.267` <dbl>, `1.1.1.27` <dbl>,
    #   `1.1.1.270` <dbl>, `1.1.1.271` <dbl>, `1.1.1.276` <dbl>, `1.1.1.28` <dbl>, `1.1.1.281` <dbl>, …
    # ℹ Use `colnames()` to see all variable names


2. Change the counts which is below some threshold

```R
MAE |>
  EMP_assay_extract('taxonomy') |>
  EMP_modify_assay('<10',pseudocount=0) 
```

    # A tibble: 20 × 444
       primary Archaea;Candidatus_Thermo…¹ Archaea;Euryarchaeot…² Bacteria;Actinobacte…³ Bacteria;Actinobacte…⁴
       <chr>                         <dbl>                  <dbl>                  <dbl>                  <dbl>
     1 P11774                            0                      0                      0                      0
     2 P31579                            0                      0                      0                      0
     3 P33531                            0                      0                      0                      0
     4 P36987                            0                      0                      0                      0
     5 P40725                            0                      0                      0                      0
     6 P40923                            0                      0                      0                      0
     7 P51467                            0                      0                      0                      0
     8 P51956                            0                     43                      0                      0
     9 P52630                            0                      0                      0                      0
    10 P54631                            0                      0                      0                      0
    11 P60426                            0                      0                      0                      0
    12 P66101                            0                      0                      0                      0
    13 P68071                            0                      0                      0                      0
    14 P69489                            0                     18                      0                      0
    15 P70597                            0                      0                      0                      0
    16 P75656                            0                      0                      0                      0
    17 P84567                            0                      0                      0                      0
    18 P94346                            0                      0                      0                     14
    19 P95158                            0                      0                      0                      0
    20 P96518                            0                      0                      0                      0
    # ℹ abbreviated names:
    #   ¹`Archaea;Candidatus_Thermoplasmatota;Thermoplasmata;Methanomassiliicoccales;Methanomassiliicoccaceae;Methanomassiliicoccus;Candidatus_Methanomassiliicoccus_intestinalis;`,
    #   ²`Archaea;Euryarchaeota;Methanobacteria;Methanobacteriales;Methanobacteriaceae;Methanobrevibacter;Methanobrevibacter_smithii;`,
    #   ³`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_meyeri;`,
    #   ⁴`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_odontolytica;`
    # ℹ 439 more variables:
    #   `Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_adolescentis;` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

#### EMP_rrarefy

Rarefythe data according to the lowest abundance.

```R
MAE |>
  EMP_rrarefy(experiment = 'taxonomy')
```
    Min depth: 9987
    Max depth: 10007
    Current raresize: 9987
    # A tibble: 20 × 444
       primary Archaea;Candidatus_Thermo…¹ Archaea;Euryarchaeot…² Bacteria;Actinobacte…³ Bacteria;Actinobacte…⁴
       <chr>                         <dbl>                  <dbl>                  <dbl>                  <dbl>
     1 P11774                            0                      0                      0                      0
     2 P31579                            0                      0                      0                      5
     3 P33531                            0                      0                      0                      4
     4 P36987                            0                      0                      0                      3
     5 P40725                            0                      0                      0                      0
     6 P40923                            0                      0                      0                      7
     7 P51467                            0                      0                      0                      0
     8 P51956                            0                     43                      0                      0
     9 P52630                            0                      0                      0                      0
    10 P54631                            0                      0                      0                      0
    11 P60426                            0                      0                      0                      4
    12 P66101                            0                      0                      0                      0
    13 P68071                            0                      0                      0                      5
    14 P69489                            0                     18                      0                      0
    15 P70597                            0                      0                      0                      0
    16 P75656                            0                      0                      0                      0
    17 P84567                            0                      0                      0                      0
    18 P94346                            0                      0                      3                     13
    19 P95158                            2                      0                      0                      4
    20 P96518                            0                      0                      0                      2
    # ℹ abbreviated names:
    #   ¹`Archaea;Candidatus_Thermoplasmatota;Thermoplasmata;Methanomassiliicoccales;Methanomassiliicoccaceae;Methanomassiliicoccus;Candidatus_Methanomassiliicoccus_intestinalis;`,
    #   ²`Archaea;Euryarchaeota;Methanobacteria;Methanobacteriales;Methanobacteriaceae;Methanobrevibacter;Methanobrevibacter_smithii;`,
    #   ³`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_meyeri;`,
    #   ⁴`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_odontolytica;`
    # ℹ 439 more variables:
    #   `Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_adolescentis;` <dbl>, …
    # ℹ Use `colnames()` to see all variable names


Only show the depth

```R
MAE |>
  EMP_rrarefy(experiment = 'taxonomy',only_show_depth=T) 
```
    Min depth: 9987
    Max depth: 10007
    NULL


Set a specific threshold

```R
MAE |>
  EMP_rrarefy(experiment = 'taxonomy',raresize=1000) 
```
    Min depth: 9987
    Max depth: 10007
    Current raresize: 1000
    # A tibble: 20 × 316
       primary Archaea;Euryarchaeota;Met…¹ Bacteria;Actinobacte…² Bacteria;Actinobacte…³ Bacteria;Actinobacte…⁴
       <chr>                         <dbl>                  <dbl>                  <dbl>                  <dbl>
     1 P11774                            0                      0                      0                      0
     2 P31579                            0                      0                      0                      0
     3 P33531                            0                      0                      0                      0
     4 P36987                            0                      0                      0                      1
     5 P40725                            0                      0                      0                      3
     6 P40923                            0                      0                      2                      0
     7 P51467                            0                      0                      0                      0
     8 P51956                            3                      0                      0                      0
     9 P52630                            0                      0                      0                      0
    10 P54631                            0                      0                      0                      4
    11 P60426                            0                      0                      0                      0
    12 P66101                            0                      0                      0                      0
    13 P68071                            0                      0                      0                      0
    14 P69489                            2                      0                      0                      0
    15 P70597                            0                      0                      0                      0
    16 P75656                            0                      0                      0                      0
    17 P84567                            0                      0                      0                      4
    18 P94346                            0                      1                      3                      0
    19 P95158                            0                      0                      1                      0
    20 P96518                            0                      0                      1                      0
    # ℹ abbreviated names:
    #   ¹`Archaea;Euryarchaeota;Methanobacteria;Methanobacteriales;Methanobacteriaceae;Methanobrevibacter;Methanobrevibacter_smithii;`,
    #   ²`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_meyeri;`,
    #   ³`Bacteria;Actinobacteria;Actinomycetia;Actinomycetales;Actinomycetaceae;Schaalia;Schaalia_odontolytica;`,
    #   ⁴`Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_adolescentis;`
    # ℹ 311 more variables:
    #   `Bacteria;Actinobacteria;Actinomycetia;Bifidobacteriales;Bifidobacteriaceae;Bifidobacterium;Bifidobacterium_bifidum;` <dbl>, …
    # ℹ Use `colnames()` to see all variable names

### Data Analysis :

This module is designed to aid users in conducting various popular  multi-omics analyses, including differential analysis, enrichment analysis, dimension reduction,  feature selection, and omics integration, etc.

#### EMP_alpha_analysis

```R
MAE |>
  EMP_assay_extract(experiment='taxonomy') |> 
  EMP_alpha_analysis()
```

    # A tibble: 20 × 7
       primary shannon simpson invsimpson observerd_index chao1   ACE
       <chr>     <dbl>   <dbl>      <dbl>           <dbl> <dbl> <dbl>
     1 P11774     3.63   0.948      19.0              172   172   172
     2 P31579     3.83   0.955      22.0              184   184   184
     3 P33531     3.54   0.938      16.1              168   168   168
     4 P36987     2.86   0.800       5.00             139   139   139
     5 P40725     4.05   0.953      21.3              238   238   238
     6 P40923     3.33   0.918      12.2              180   180   180
     7 P51467     3.78   0.956      22.5              195   195   195
     8 P51956     4.40   0.975      40.0              278   278   278
     9 P52630     4.12   0.974      38.5              178   178   178
    10 P54631     2.63   0.817       5.47             118   118   118
    11 P60426     3.81   0.951      20.6              191   191   191
    12 P66101     3.10   0.912      11.4               98    98    98
    13 P68071     3.57   0.939      16.3              170   170   170
    14 P69489     4.21   0.970      33.0              238   238   238
    15 P70597     3.56   0.935      15.4              146   146   146
    16 P75656     2.52   0.751       4.02             110   110   110
    17 P84567     2.86   0.855       6.91             123   123   123
    18 P94346     3.44   0.911      11.3              224   224   224
    19 P95158     3.98   0.961      25.8              231   231   231
    20 P96518     4.03   0.968      31.7              222   222   222

```R
MAE |>
  EMP_assay_extract(experiment='geno_ec') |> 
  EMP_alpha_analysis()
```
    # A tibble: 20 × 7
       primary shannon simpson invsimpson observerd_index chao1   ACE
       <chr>     <dbl>   <dbl>      <dbl>           <dbl> <dbl> <dbl>
     1 P11774     6.67   0.998       508.            1811 2004. 1930.
     2 P31579     6.73   0.998       507.            1887 2023. 1956.
     3 P33531     6.70   0.998       514.            1791 1945. 1915.
     4 P36987     6.57   0.998       448.            1754 1884. 1862.
     5 P40725     6.73   0.998       498.            2007 2112. 2102.
     6 P40923     6.71   0.998       504.            1898 2018. 2000.
     7 P51467     6.82   0.998       577.            1830 1896. 1900.
     8 P51956     6.72   0.998       520.            1970 2088. 2055.
     9 P52630     6.71   0.998       519.            1831 1948  1926.
    10 P54631     6.75   0.998       519.            1900 1969. 1961.
    11 P60426     6.78   0.998       535.            1950 2039. 2022.
    12 P66101     6.75   0.998       544.            1909 2066. 2034.
    13 P68071     6.66   0.998       495.            1582 1690. 1706.
    14 P69489     6.80   0.998       575.            1979 2097. 2071.
    15 P70597     6.74   0.998       527.            1856 1951. 1945.
    16 P75656     6.66   0.998       484.            1908 2010. 1986.
    17 P84567     6.62   0.998       427.            1925 2159. 2104.
    18 P94346     6.72   0.998       522.            1918 2009. 2017.
    19 P95158     6.68   0.998       489.            1837 1985. 1961.
    20 P96518     6.57   0.998       439.            1845 2050. 2003.


#### EMP_cluster_analysis

1. Cluster the samples according to the assay data

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_cluster_analysis()
```
<img src="tutorial_related/tutorial_figs/clust1.jpg" alt="clust1" style="zoom:100%;" />

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_cluster_analysis(h=0.15) |> ### identify the outlier samples
  EMP_filter(cluster != 1) ### filter away the outlier samples
```
<img src="tutorial_related/tutorial_figs/clust2.jpg" alt="clust2" style="zoom:100%;" />


    If any samples in the experiment have changed, the sample_cluster_result will become NULL and
    EMP_cluster_analysis should be re-run if needed.
    1 of 20 samples were filterd out!
    0 of 2424 features were filterd out!
    # A tibble: 19 × 2,425
       primary `1.1.1.1` `1.1.1.100` `1.1.1.102` `1.1.1.103` `1.1.1.105` `1.1.1.108` `1.1.1.11` `1.1.1.122`
       <chr>       <int>       <int>       <int>       <int>       <int>       <int>      <int>       <int>
     1 P11774        412           9           0           9           0           5          0           0
     2 P31579        327           3           0          72           0           4          6           5
     3 P33531        285          10           0          44           0          10          0           0
     4 P36987        163           3           0          98           0           0          0           0
     5 P40725        433           8           0          34           0           1          0           0
     6 P40923        497           3           0          16           0           0          1           0
     7 P51467        599           2           0          47           0           1          0           0
     8 P51956        534          18           0          16           0           2          8           0
     9 P52630        478           9           0          10           3           1          0           0
    10 P54631        329          10           0          11           0           5          9           0
    11 P60426        430           4           0          25           0           3          1           0
    12 P66101        415          10           1          19           0           3          0           0
    13 P69489        501          11           0          18           0           1          4           0
    14 P70597        300           7           0          19           0          14          0           0
    15 P75656        286           6           0         184           0           0          3           0
    16 P84567        219          12           0          93           0           0          0           0
    17 P94346        408           4           0          17           0           2          0           0
    18 P95158        377          16           0          16           0           1          0           0
    19 P96518        229           2           0          88           3           0          0           0
    # ℹ 2,416 more variables: `1.1.1.130` <int>, `1.1.1.132` <int>, `1.1.1.133` <int>, `1.1.1.135` <int>,
    #   `1.1.1.136` <int>, `1.1.1.137` <int>, `1.1.1.14` <int>, `1.1.1.141` <int>, `1.1.1.157` <int>,
    #   `1.1.1.169` <int>, `1.1.1.17` <int>, `1.1.1.18` <int>, `1.1.1.193` <int>, `1.1.1.2` <int>,
    #   `1.1.1.202` <int>, `1.1.1.205` <int>, `1.1.1.21` <int>, `1.1.1.215` <int>, `1.1.1.219` <int>,
    #   `1.1.1.22` <int>, `1.1.1.23` <int>, `1.1.1.25` <int>, `1.1.1.251` <int>, `1.1.1.26` <int>,
    #   `1.1.1.261` <int>, `1.1.1.262` <int>, `1.1.1.264` <int>, `1.1.1.267` <int>, `1.1.1.27` <int>,
    #   `1.1.1.270` <int>, `1.1.1.271` <int>, `1.1.1.276` <int>, `1.1.1.28` <int>, `1.1.1.281` <int>, …
    # ℹ Use `colnames()` to see all variable names



2. Cluster the samples according to the coldata

```R
MAE |> 
  EMP_coldata_extract(action = 'add') |> ### transfer the coldata to asaay
  EMP_impute(assay = T) |> ### impute missing value
  EMP_cluster_analysis(method = 'ward.D2',distance='clark',h=0.2) 
```
<img src="tutorial_related/tutorial_figs/clust3.jpg" alt="clust3" style="zoom:100%;" />

    $sample_cluster_result
    # A tibble: 20 × 2
       primary cluster
       <chr>     <int>
     1 P52630        1
     2 P96518        2
     3 P31579        3
     4 P75656        4
     5 P40725        5
     6 P94346        6
     7 P66101        7
     8 P68071        8
     9 P33531        8
    10 P40923        9
    11 P36987       10
    12 P11774       10
    13 P70597       11
    14 P54631       12
    15 P95158       12
    16 P60426       13
    17 P69489       13
    18 P51956       14
    19 P51467       14
    20 P84567       14


3. Cluster the features according to the assay data

```R
MAE |> 
  EMP_assay_extract(experiment = 'geno_ec',
                    pattern='1.1.1.1',pattern_ref='feature') |>
  EMP_cluster_analysis(rowdata = T,h=0.8)
```

    $feature_cluster_result
    # A tibble: 21 × 2
       feature   cluster
       <chr>       <int>
     1 1.1.1.102       1
     2 1.1.1.105       2
     3 1.1.1.141       3
     4 1.1.1.1         4
     5 1.1.1.17        4
     6 1.1.1.169       4
     7 1.1.1.193       4
     8 1.1.1.133       4
     9 1.1.1.14        4
    10 1.1.1.18        4
    # ℹ 11 more rows
    # ℹ Use `print(n = ...)` to see more rows


#### EMP_cor_analysis

```R
k1 <- MAE |>
  EMP_assay_extract('taxonomy') |>
  EMP_collapse(estimate_group = 'Genus',collapse_by = 'row') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.05)
k2 <- MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.05 & abs(fold_change) > 1.5)

(k1 + k2) |> EMP_cor_analysis(method = 'spearman') |>
  EMP_heatmap_plot() ###### Visualization
```
<img src="tutorial_related/tutorial_figs/cor1.jpg" alt="cor1" style="zoom:100%;" />


#### EMP_diff_analysis

t.test or wilcox.test

```R
MAE |>
  EMP_decostand(experiment = 'taxonomy',method = 'relative',pseudocount=0.0001) |>
  EMP_diff_analysis(method = 't.test',estimate_group = 'Group',p.adjust = 'fdr')
```
    # A tibble: 445 × 9
       feature                         Estimate_group pvalue   fdr sign_group method vs    fold_change   log2FC
       <chr>                           <chr>           <dbl> <dbl> <chr>      <chr>  <chr>       <dbl>    <dbl>
     1 Archaea;Candidatus_Thermoplasm… Group           0.343 0.741 Group_B    Welch… Grou…    1000      9.97   
     2 Archaea;Euryarchaeota;Methanob… Group           0.206 0.741 Group_B    Welch… Grou…    1000      9.97   
     3 Bacteria;Actinobacteria;Actino… Group           0.343 0.741 Group_A    Welch… Grou…       0.001 -9.97   
     4 Bacteria;Actinobacteria;Actino… Group           0.398 0.773 Group_A    Welch… Grou…       0.548 -0.867  
     5 Bacteria;Actinobacteria;Actino… Group           0.392 0.773 Group_A    Welch… Grou…       0.417 -1.26   
     6 Bacteria;Actinobacteria;Actino… Group           0.999 1.00  Group_A    Welch… Grou…       0.999 -0.00188
     7 Bacteria;Actinobacteria;Actino… Group           0.749 0.950 Group_A    Welch… Grou…       0.769 -0.378  
     8 Bacteria;Actinobacteria;Actino… Group           0.587 0.880 Group_B    Welch… Grou…       2.00   1.00   
     9 Bacteria;Actinobacteria;Actino… Group           0.669 0.933 Group_A    Welch… Grou…       0.723 -0.468  
    10 Bacteria;Actinobacteria;Actino… Group           0.436 0.785 Group_B    Welch… Grou…       3.06   1.62   
    # ℹ 435 more rows
    # ℹ Use `print(n = ...)` to see more rows

```R
MAE |>
  EMP_decostand(experiment = 'taxonomy',method = 'relative',pseudocount=0.0001) |>
  EMP_diff_analysis(method = 'wilcox.test',estimate_group = 'Group',p.adjust = 'fdr')
```
    # A tibble: 445 × 9
       feature                         Estimate_group pvalue   fdr sign_group method vs    fold_change   log2FC
       <chr>                           <chr>           <dbl> <dbl> <chr>      <chr>  <chr>       <dbl>    <dbl>
     1 Archaea;Candidatus_Thermoplasm… Group           0.368 0.766 Group_B    Wilco… Grou…    1000      9.97   
     2 Archaea;Euryarchaeota;Methanob… Group           0.168 0.766 Group_B    Wilco… Grou…    1000      9.97   
     3 Bacteria;Actinobacteria;Actino… Group           0.368 0.766 Group_A    Wilco… Grou…       0.001 -9.97   
     4 Bacteria;Actinobacteria;Actino… Group           0.649 0.958 Group_A    Wilco… Grou…       0.548 -0.867  
     5 Bacteria;Actinobacteria;Actino… Group           0.780 1     Group_A    Wilco… Grou…       0.417 -1.26   
     6 Bacteria;Actinobacteria;Actino… Group           1     1     Group_A    Wilco… Grou…       0.999 -0.00188
     7 Bacteria;Actinobacteria;Actino… Group           1     1     Group_A    Wilco… Grou…       0.769 -0.378  
     8 Bacteria;Actinobacteria;Actino… Group           0.957 1     Group_B    Wilco… Grou…       2.00   1.00   
     9 Bacteria;Actinobacteria;Actino… Group           0.704 0.983 Group_A    Wilco… Grou…       0.723 -0.468  
    10 Bacteria;Actinobacteria;Actino… Group           0.905 1     Group_B    Wilco… Grou…       3.06   1.62   
    # ℹ 435 more rows
    # ℹ Use `print(n = ...)` to see more rows


DESeq2

```R
MAE |>
  EMP_decostand(experiment = 'geno_ec',method = 'integer') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group)  
```

    # A tibble: 2,424 × 9
       feature   Estimate_group pvalue   fdr sign_group method vs                 fold_change  log2FC
       <chr>     <chr>           <dbl> <dbl> <fct>      <chr>  <chr>                    <dbl>   <dbl>
     1 1.1.1.1   Group          0.747  0.993 Group_A    DESeq2 Group_A vs Group_B       1.04   0.0585
     2 1.1.1.100 Group          0.194  0.765 Group_A    DESeq2 Group_A vs Group_B       1.49   0.578 
     3 1.1.1.102 Group          0.903  0.993 Group_B    DESeq2 Group_A vs Group_B       0.775 -0.369 
     4 1.1.1.103 Group          0.195  0.765 Group_B    DESeq2 Group_A vs Group_B       0.614 -0.704 
     5 1.1.1.105 Group          0.531  0.968 Group_B    DESeq2 Group_A vs Group_B       0.270 -1.89  
     6 1.1.1.108 Group          0.0846 0.591 Group_A    DESeq2 Group_A vs Group_B       2.71   1.44  
     7 1.1.1.11  Group          0.664  0.993 Group_B    DESeq2 Group_A vs Group_B       0.613 -0.706 
     8 1.1.1.122 Group          0.623  0.993 Group_B    DESeq2 Group_A vs Group_B       0.356 -1.49  
     9 1.1.1.130 Group          0.0676 0.541 Group_A    DESeq2 Group_A vs Group_B       1.93   0.950 
    10 1.1.1.132 Group          0.692  0.993 Group_A    DESeq2 Group_A vs Group_B       1.30   0.384 
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

```R
MAE |>
  EMP_decostand(experiment = 'geno_ec',method = 'integer') |>
  EMP_diff_analysis(method='DESeq2',
                    .formula = ~Region+Group)  # Eliminate the batch_effect in DESeq2
```

    # A tibble: 2,424 × 10
       feature   Estimate_group batch_effect pvalue   fdr sign_group method vs               fold_change log2FC
       <chr>     <chr>          <chr>         <dbl> <dbl> <fct>      <chr>  <chr>                  <dbl>  <dbl>
     1 1.1.1.1   Group          Region       0.152      1 Group_A    DESeq2 Group_A vs Grou…      1.20    0.264
     2 1.1.1.100 Group          Region       0.304      1 Group_B    DESeq2 Group_A vs Grou…      0.714  -0.486
     3 1.1.1.102 Group          Region       0.917      1 Group_B    DESeq2 Group_A vs Grou…      0.796  -0.329
     4 1.1.1.103 Group          Region       0.108      1 Group_B    DESeq2 Group_A vs Grou…      0.540  -0.889
     5 1.1.1.105 Group          Region       0.962      1 Group_B    DESeq2 Group_A vs Grou…      0.901  -0.150
     6 1.1.1.108 Group          Region       0.860      1 Group_B    DESeq2 Group_A vs Grou…      0.896  -0.158
     7 1.1.1.11  Group          Region       0.0305     1 Group_B    DESeq2 Group_A vs Grou…      0.0886 -3.50 
     8 1.1.1.122 Group          Region       0.672      1 Group_B    DESeq2 Group_A vs Grou…      0.397  -1.33 
     9 1.1.1.130 Group          Region       0.175      1 Group_A    DESeq2 Group_A vs Grou…      1.65    0.719
    10 1.1.1.132 Group          Region       0.816      1 Group_B    DESeq2 Group_A vs Grou…      0.840  -0.251
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

edgeR_quasi_likelihood

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_diff_analysis(method='edgeR_quasi_likelihood',
                    .formula = ~0+Group,
                    group_level = c('Group_B','Group_A')) # Set the comparison order.
```

    # A tibble: 2,424 × 9
       feature   Estimate_group pvalue   fdr sign_group method                 vs                 fold_change  log2FC
       <chr>     <chr>           <dbl> <dbl> <chr>      <chr>                  <chr>                    <dbl>   <dbl>
     1 1.1.1.1   Group          0.838  0.944 Group_B    edgeR_quasi_likelihood Group_B vs Group_A       1.02   0.0343
     2 1.1.1.100 Group          0.245  0.581 Group_B    edgeR_quasi_likelihood Group_B vs Group_A       1.50   0.584 
     3 1.1.1.102 Group          0.228  0.581 Group_A    edgeR_quasi_likelihood Group_B vs Group_A       0.549 -0.864 
     4 1.1.1.103 Group          0.200  0.581 Group_A    edgeR_quasi_likelihood Group_B vs Group_A       0.606 -0.723 
     5 1.1.1.105 Group          0.0159 0.404 Group_A    edgeR_quasi_likelihood Group_B vs Group_A       0.161 -2.64  
     6 1.1.1.108 Group          0.0834 0.472 Group_B    edgeR_quasi_likelihood Group_B vs Group_A       2.60   1.38  
     7 1.1.1.11  Group          0.532  0.788 Group_A    edgeR_quasi_likelihood Group_B vs Group_A       0.648 -0.626 
     8 1.1.1.122 Group          0.0269 0.404 Group_A    edgeR_quasi_likelihood Group_B vs Group_A       0.204 -2.30  
     9 1.1.1.130 Group          0.0964 0.477 Group_B    edgeR_quasi_likelihood Group_B vs Group_A       1.87   0.902 
    10 1.1.1.132 Group          0.151  0.547 Group_B    edgeR_quasi_likelihood Group_B vs Group_A       2.66   1.41  
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

#### EMP_dimension_analysis

PCA

```R
MAE |>
  EMP_dimension_analysis(experiment = 'taxonomy',method = 'pca') 
```

    $dimension_coordinate
    # A tibble: 20 × 4
       primary     PC1     PC2     PC3
       <chr>     <dbl>   <dbl>   <dbl>
     1 P11774  -0.159   0.304   0.109 
     2 P31579  -0.151   0.200   0.0612
     3 P33531  -0.182   0.258   0.320 
     4 P36987  -0.412  -0.282   0.0987
     5 P40725  -0.119   0.204  -0.216 
     6 P40923  -0.177   0.345   0.430 
     7 P51467  -0.126   0.105  -0.179 
     8 P51956  -0.0577  0.206  -0.163 
     9 P52630  -0.0955  0.173  -0.116 
    10 P54631  -0.388  -0.185  -0.168 
    11 P60426  -0.186   0.162   0.0472
    12 P66101  -0.0982  0.266  -0.554 
    13 P68071  -0.199   0.236   0.160 
    14 P69489  -0.0563  0.176  -0.203 
    15 P70597  -0.208  -0.0625  0.0548
    16 P75656  -0.454  -0.348   0.124 
    17 P84567  -0.354  -0.160  -0.143 
    18 P94346  -0.204   0.0187 -0.258 
    19 P95158  -0.108   0.287   0.126 
    20 P96518  -0.0985  0.159  -0.207 

    $dimension_axis
    [1] 57.57 24.58 17.85

Pcoa

```R
MAE |>
  EMP_dimension_analysis(experiment = 'taxonomy',method = 'pcoa',distance = 'bray')
```

    $dimension_coordinate
       primary        PCoA1        PCoA2       PCoA3
    1   P11774  0.004101622 -0.219926994  0.09159443
    2   P31579  0.023514158 -0.147319164  0.02071926
    3   P33531  0.054036219 -0.230044101  0.03902967
    4   P36987  0.238096960  0.148201768 -0.03073729
    5   P40725 -0.171466199  0.018713925 -0.22821785
    6   P40923 -0.017583826 -0.224562064 -0.04071953
    7   P51467 -0.285094111  0.207554032  0.04329776
    8   P51956 -0.385446565  0.095535092 -0.03733299
    9   P52630 -0.217748926  0.005531636  0.06247941
    10  P54631  0.307461883  0.167761882  0.10730142
    11  P60426  0.015935377 -0.146685650 -0.08596156
    12  P66101  0.145107584 -0.010579636  0.24540364
    13  P68071  0.003578938 -0.158887519 -0.05831650
    14  P69489 -0.373006187  0.144111252  0.08385357
    15  P70597  0.183525914  0.020369985 -0.03454309
    16  P75656  0.316845121  0.187448812  0.08189323
    17  P84567  0.297297071  0.165088352  0.03105270
    18  P94346  0.148312403  0.088701094 -0.43595252
    19  P95158  0.008474471 -0.241883088  0.06270487
    20  P96518 -0.295941908  0.130870386  0.08245137

    $dimension_axis
    [1] 34.34 17.63 13.36


Pls

```R
MAE |>
  EMP_dimension_analysis(experiment = 'untarget_metabol',
                         method = 'pls',estimate_group = 'Group')
```

    $dimension_coordinate
    # A tibble: 20 × 4
       primary        t1        t2        t3
       <chr>       <dbl>     <dbl>     <dbl>
     1 P11774  30317415.  -485647.  1112671.
     2 P31579  23073876. -3154074. -1561236.
     3 P33531  41586039.  2274052.  -218469.
     4 P36987  24001657. -3705934.    73593.
     5 P40725  29963622.   443779. -2503344.
     6 P40923  20493156. -5043751. -1217493.
     7 P51467  42762386.  3914821.   831260.
     8 P51956  32442328.   946215.   751152.
     9 P52630  23304045. -1711374.  -123850.
    10 P54631  25310943. -1882605.  -316750.
    11 P60426  26056504.  -782059.  3646996.
    12 P66101  29423188.  -896887.   629807.
    13 P68071  28259261.   378843.   908272.
    14 P69489  19440822. -2491971.  1129851.
    15 P70597  29495328.  8275661. -2098066.
    16 P75656  26520534. -3582383.  -487312.
    17 P84567  30019107.   451548.   675429.
    18 P94346  19015693. -1446437. -1516377.
    19 P95158  34532584.  1931160.   648033.
    20 P96518  22610752. -2474239. -1753448.

    $dimension_VIP
    # A tibble: 710 × 2
       feature         VIP
       <chr>         <dbl>
     1 pos-M61T41  0.00253
     2 pos-M61T147 0.00268
     3 pos-M61T51  0.00756
     4 pos-M62T41  0.00162
     5 pos-M63T552 0.00550
     6 pos-M63T551 0.00119
     7 pos-M65T551 0.00201
     8 pos-M65T148 0.00564
     9 pos-M66T41  0.00173
    10 pos-M67T178 0.00360
    # ℹ 700 more rows
    # ℹ Use `print(n = ...)` to see more rows

    $dimension_axis
    [1] 96.400  0.968  0.218

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',
               na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',
               method = 'sum',collapse_by = 'row') |> # Get the kegg compound
  EMP_dimension_analysis(method = 'pls',estimate_group = 'Group')
```

    $dimension_coordinate
    # A tibble: 20 × 4
       primary        t1         t2        t3
       <chr>       <dbl>      <dbl>     <dbl>
     1 P11774  71825010.   1449190.  -896964.
     2 P31579  57854806.  -6840135.  -259301.
     3 P33531  93967497.   9353928. -2458657.
     4 P36987  58065643.  -7554557. -1001074.
     5 P40725  64953621.  -2583117. -4083072.
     6 P40923  49317437. -16100960. -1343397.
     7 P51467  94531587.  15784738.  1237971.
     8 P51956  73558938.   -960156. -1523990.
     9 P52630  53161481.  -5744750.   437193.
    10 P54631  63597342.  -3929692. -1222343.
    11 P60426  65907286.   2530232.  7718840.
    12 P66101  69301960.   2247136.    78185.
    13 P68071  68988945.   2876256.  3252336.
    14 P69489  46616005.  -7887130.  2712534.
    15 P70597  64393609.  13773373.  -311948.
    16 P75656  63002182.  -9669140.  2230182.
    17 P84567  68488383.   2547632.  -647486.
    18 P94346  43868117.  -7950264. -2226006.
    19 P95158  79129887.   6812869. -2104301.
    20 P96518  50692582. -16744960.  1435004.

    $dimension_VIP
    # A tibble: 192 × 2
       feature      VIP
       <chr>      <dbl>
     1 C00020  0.0119  
     2 C00022  0.000977
     3 C00025  0.00139 
     4 C00047  0.00611 
     5 C00051  0.00185 
     6 C00062  0.0102  
     7 C00064  0.00713 
     8 C00072  0.00442 
     9 C00073  0.00427 
    10 C00074  0.00592 
    # ℹ 182 more rows
    # ℹ Use `print(n = ...)` to see more rows

    $dimension_axis
    [1] 98.800  1.060  0.104


OPLS

```R
MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |>
  EMP_dimension_analysis(method = 'opls',estimate_group = 'Group') |>
  EMP_scatterplot(show='p12html',ellipse=0.3) ###### Visualization
```

<img src="tutorial_related/tutorial_figs/opls.jpg" alt="opls" style="zoom:100%;" />



#### EMP_enrich_analysis

Make the enrichment after EMP_diff_analysis

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) |>
  EMP_enrich_analysis(keyType ='ec',KEGG_Type = 'KEGG',
                      pvalue<0.05,pvalueCutoff=0.05,species = 'all') 
```

    Reading KEGG annotation online: "https://rest.kegg.jp/link/ec/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    KEGG database version: Release 110.0+/04-25, Apr 24
    Species: all
    # A tibble: 9 × 11
      Cluster sign_group ID       Description       GeneRatio BgRatio  pvalue p.adjust  qvalue geneID Count
      <fct>   <chr>      <chr>    <chr>             <chr>     <chr>     <dbl>    <dbl>   <dbl> <chr>  <int>
    1 Group_A Group_A    map00785 Lipoic acid meta… 6/92      14/3825 4.23e-7  2.83e-5 2.49e-5 1.2.4…     6
    2 Group_A Group_A    map00531 Glycosaminoglyca… 4/92      15/3825 3.49e-4  1.17e-2 1.03e-2 3.1.6…     4
    3 Group_A Group_A    map00520 Amino sugar and … 10/92     127/38… 8.15e-4  1.82e-2 1.60e-2 2.6.1…    10
    4 Group_A Group_A    map00280 Valine, leucine … 5/92      36/3825 1.51e-3  2.53e-2 2.22e-2 1.2.4…     5
    5 Group_A Group_A    map00030 Pentose phosphat… 6/92      59/3825 2.65e-3  3.56e-2 3.13e-2 1.1.1…     6
    6 Group_B Group_B    map00970 Aminoacyl-tRNA b… 9/78      31/3825 5.35e-9  2.27e-7 2.05e-7 6.1.1…     9
    7 Group_B Group_B    map00550 Peptidoglycan bi… 8/78      23/3825 7.96e-9  2.27e-7 2.05e-7 2.3.2…     8
    8 Group_B Group_B    map00730 Thiamine metabol… 5/78      28/3825 2.11e-4  4.01e-3 3.63e-3 2.2.1…     5
    9 Group_B Group_B    map00250 Alanine, asparta… 6/78      50/3825 4.61e-4  6.57e-3 5.94e-3 1.4.1…     6

Make the enrichment after EMP_diff_analysis and Visualization

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) |>
  EMP_enrich_analysis(keyType ='ec',KEGG_Type = 'KEGG',
                      pvalue<0.05,pvalueCutoff=0.05,species = 'all') |>
  EMP_dotplot()
```
<img src="tutorial_related/tutorial_figs/enrich1.jpg" alt="enrich1" style="zoom:100%;" />


```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) |>
  EMP_enrich_analysis(keyType ='ec',KEGG_Type = 'KEGG',
                      pvalue<0.05,pvalueCutoff=0.05,species = 'all') |>
  EMP_netplot()
```
<img src="tutorial_related/tutorial_figs/enrich2.jpg" alt="enrich2" style="zoom:100%;" />

Transcriptomic data

```R
MAE |>
  EMP_assay_extract(experiment = 'host_gene') |>
  EMP_feature_convert(from = 'symbol',to='entrezid',species='Human') |>
  EMP_diff_analysis(method = 'DESeq2',
                    .formula = ~Group,p.adjust = 'fdr') |> 
  EMP_enrich_analysis(keyType ='entrezid',
                      KEGG_Type ='KEGG',pvalue<0.05,
                      pvalueCutoff=0.05,species = 'hsa') |>
  EMP_dotplot()
```
<img src="tutorial_related/tutorial_figs/enrich3.jpg" alt="enrich3" style="zoom:100%;" />


#### EMP_marker_analysis

To estimate the improtance of feature by Boruta algorithm

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'boruta',
                      estimate_group = 'Group') |>
  EMP_filter(feature_condition = Boruta_decision!= 'Rejected') ###### select the Confirmed and Tentative feature
```

    If any features in the experiment have changed, the Boruta_model will become NULL and
    EMP_marker_analysis should be re-run if needed.
    If any features in the experiment have changed, the Boruta_feature_importance will become NULL
    and EMP_marker_analysis should be re-run if needed.
    0 of 20 samples were filterd out!
    2416 of 2424 features were filterd out!
    # A tibble: 20 × 9
       primary `2.1.1.181` `2.3.1.59` `3.1.1.31` `3.1.3.85` `3.1.6.1` `3.4.21.81` `4.1.3.45`
       <chr>         <int>      <int>      <int>      <int>     <int>       <int>      <int>
     1 P11774           13        126        633         64       160           0         12
     2 P31579           69        212        621         70       443           8         39
     3 P33531          101        349        634         19       634           0         59
     4 P36987            2         28        384        225       134          27          3
     5 P40725            6         55        392        124        74          21          8
     6 P40923            3         37        430        186       145           9          5
     7 P51467           30        155        583         49       333          92         18
     8 P51956           12        221        561         61       181           0         11
     9 P52630            4         55        472        377        96          61          1
    10 P54631            6        104        473         72       115          14          8
    11 P60426           28        109        527         81       175          66         20
    12 P66101            6         54        441         96       148           1          6
    13 P68071           93        373        663         17       658           0         74
    14 P69489           13        190        460         50       271           0         11
    15 P70597           12        134        511         40       162           2         12
    16 P75656            5         23        434        170       122          49          2
    17 P84567            2         79        381        203        41           1          7
    18 P94346            7         92        473        135       136          46          6
    19 P95158            8         68        496        194       150           0          5
    20 P96518            5          3        341        205        94          37          4
    # ℹ 1 more variable: `6.2.1.30` <int>

regression or classify by randomforest

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'randomforest',
                      estimate_group = 'Group') 
```
    $rf_model

    Call:
     randomForest(formula = Group ~ ., data = tran_data, importance = TRUE) 
                   Type of random forest: classification
                         Number of trees: 500
    No. of variables tried at each split: 49

            OOB estimate of  error rate: 40%
    Confusion matrix:
            Group_A Group_B class.error
    Group_A       6       4         0.4
    Group_B       4       6         0.4

    $rf_feature_importance
    # A tibble: 2,424 × 3
       feature    rf_MDA rf_MDG
       <chr>       <dbl>  <dbl>
     1 1.1.1.1    0      0     
     2 1.1.1.100  0      0     
     3 1.1.1.102  0      0     
     4 1.1.1.103  0      0     
     5 1.1.1.105  0      0     
     6 1.1.1.108  0      0     
     7 1.1.1.11   0      0     
     8 1.1.1.122  0      0.0036
     9 1.1.1.130  0      0     
    10 1.1.1.132 -0.0006 0.0134
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'randomforest',
                      estimate_group = 'Education_Years') 
```

    $rf_model

    Call:
     randomForest(formula = Group ~ ., data = tran_data, importance = TRUE) 
                   Type of random forest: regression
                         Number of trees: 500
    No. of variables tried at each split: 808

              Mean of squared residuals: 7.481647
                        % Var explained: 23.66

    $rf_feature_importance
    # A tibble: 2,424 × 3
       feature   rf_IncMSE rf_IncNodePurity
       <chr>         <dbl>            <dbl>
     1 1.1.1.1     0                 0.638 
     2 1.1.1.100   0                 0.003 
     3 1.1.1.102   0                 0     
     4 1.1.1.103   0.00514           0.0519
     5 1.1.1.105   0                 0     
     6 1.1.1.108   0                 0     
     7 1.1.1.11    0                 0.163 
     8 1.1.1.122   0                 0     
     9 1.1.1.130   0                 0.938 
    10 1.1.1.132   0                 0.225 
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

regression or classify by xgboost

Note: xgboost_run should be defined as regression or classify correctly, or the wrong parameter may lead to unexpected error results.

1. regression

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'xgboost',
                      xgboost_run = "regression",
                      estimate_group = 'Education_Years',objective = 'reg:linear')

```

    [00:28:18] WARNING: src/objective/regression_obj.cu:213: reg:linear is now deprecated in favor of reg:squarederror.
    $xgb_model
    ##### xgb.Booster
    raw: 46.8 Kb 
    call:
      xgb.train(params = params, data = dtrain, nrounds = nrounds, 
        watchlist = watchlist, verbose = verbose, print_every_n = print_every_n, 
        early_stopping_rounds = early_stopping_rounds, maximize = maximize, 
        save_period = save_period, save_name = save_name, xgb_model = xgb_model, 
        callbacks = callbacks, max.depth = ..1, eta = ..2, nthread = ..3, 
        objective = ..4)
    params (as set within xgb.train):
      max_depth = "6", eta = "0.3", nthread = "7", objective = "reg:linear", validate_parameters = "TRUE"
    xgb.attributes:
      niter
    callbacks:
      cb.evaluation.log()
    # of features: 2424 
    niter: 50
    nfeatures : 2424 
    evaluation_log:
         iter   train_rmse
        <num>        <num>
            1 11.452530720
            2  8.336914334
    ---                   
           49  0.001281180
           50  0.001100391

    $xgb_feature_importance
    # A tibble: 2,424 × 5
       feature       xgb_Gain xgb_Cover xgb_Frequency xgb_Importance
       <chr>            <dbl>     <dbl>         <dbl>          <dbl>
     1 1.1.1.1   0.000434       0.0227        0.0672    0.000434    
     2 1.1.1.100 0.000536       0.0148        0.0299    0.000536    
     3 1.1.1.102 0              0             0         0           
     4 1.1.1.103 0.0488         0.361         0.291     0.0488      
     5 1.1.1.105 0              0             0         0           
     6 1.1.1.108 0.0000000403   0.00278       0.00746   0.0000000403
     7 1.1.1.11  0.0943         0.0171        0.0224    0.0943      
     8 1.1.1.122 0              0             0         0           
     9 1.1.1.130 0.267          0.00925       0.00746   0.267       
    10 1.1.1.132 0              0             0         0           
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

2. For two categories classify

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'xgboost',
                      xgboost_run = "classify",
                      estimate_group = 'Group',objective = 'binary:logistic')
```

    $xgb_model
    ##### xgb.Booster
    raw: 39.2 Kb 
    call:
      xgb.train(params = params, data = dtrain, nrounds = nrounds, 
        watchlist = watchlist, verbose = verbose, print_every_n = print_every_n, 
        early_stopping_rounds = early_stopping_rounds, maximize = maximize, 
        save_period = save_period, save_name = save_name, xgb_model = xgb_model, 
        callbacks = callbacks, max.depth = ..1, eta = ..2, nthread = ..3, 
        objective = ..4)
    params (as set within xgb.train):
      max_depth = "6", eta = "0.3", nthread = "7", objective = "binary:logistic", validate_parameters = "TRUE"
    xgb.attributes:
      niter
    callbacks:
      cb.evaluation.log()
    # of features: 2424 
    niter: 50
    nfeatures : 2424 
    evaluation_log:
         iter train_logloss
        <num>         <num>
            1     0.5706268
            2     0.4927883
    ---                    
           49     0.1233436
           50     0.1233436

    $xgb_feature_importance
    # A tibble: 2,424 × 5
       feature   xgb_Gain xgb_Cover xgb_Frequency xgb_Importance
       <chr>        <dbl>     <dbl>         <dbl>          <dbl>
     1 1.1.1.1          0         0             0              0
     2 1.1.1.100        0         0             0              0
     3 1.1.1.102        0         0             0              0
     4 1.1.1.103        0         0             0              0
     5 1.1.1.105        0         0             0              0
     6 1.1.1.108        0         0             0              0
     7 1.1.1.11         0         0             0              0
     8 1.1.1.122        0         0             0              0
     9 1.1.1.130        0         0             0              0
    10 1.1.1.132        0         0             0              0
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

3. For multible categories classify

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ec',method = 'xgboost',
                      xgboost_run = "classify",
                      estimate_group = 'Status',objective = 'multi:softmax',
                      num_class=3) ###### num_class is necessary
```
    $xgb_model
    ##### xgb.Booster
    raw: 109.3 Kb 
    call:
      xgb.train(params = params, data = dtrain, nrounds = nrounds, 
        watchlist = watchlist, verbose = verbose, print_every_n = print_every_n, 
        early_stopping_rounds = early_stopping_rounds, maximize = maximize, 
        save_period = save_period, save_name = save_name, xgb_model = xgb_model, 
        callbacks = callbacks, max.depth = ..1, eta = ..2, nthread = ..3, 
        objective = ..4, num_class = 3)
    params (as set within xgb.train):
      max_depth = "6", eta = "0.3", nthread = "7", objective = "multi:softmax", num_class = "3", validate_parameters = "TRUE"
    xgb.attributes:
      niter
    callbacks:
      cb.evaluation.log()
    # of features: 2424 
    niter: 50
    nfeatures : 2424 
    evaluation_log:
         iter train_mlogloss
        <num>          <num>
            1     0.81524238
            2     0.62473397
    ---                     
           49     0.08043040
           50     0.08043039

    $xgb_feature_importance
    # A tibble: 2,424 × 5
       feature    xgb_Gain xgb_Cover xgb_Frequency xgb_Importance
       <chr>         <dbl>     <dbl>         <dbl>          <dbl>
     1 1.1.1.1   0            0             0           0        
     2 1.1.1.100 0            0             0           0        
     3 1.1.1.102 0            0             0           0        
     4 1.1.1.103 0            0             0           0        
     5 1.1.1.105 0            0             0           0        
     6 1.1.1.108 0.00177      0.0108        0.0169      0.00177  
     7 1.1.1.11  0            0             0           0        
     8 1.1.1.122 0            0             0           0        
     9 1.1.1.130 0            0             0           0        
    10 1.1.1.132 0.0000638    0.0143        0.0169      0.0000638
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows


Lasso regression

```R
MAE |>
  EMP_marker_analysis(experiment = 'geno_ko',
                      method = 'lasso',estimate_group = 'Education_Years') |>
  EMP_filter(feature_condition = lasso_coe >0) ### Select the imprortant feature
```

    If any features in the experiment have changed, the lasso_model will become NULL and
    EMP_marker_analysis should be re-run if needed.
    If any features in the experiment have changed, the lasso_feature_importance will become NULL
    and EMP_marker_analysis should be re-run if needed.
    0 of 20 samples were filterd out!
    7627 of 7629 features were filterd out!
    # A tibble: 20 × 3
       primary K03565 K06177
       <chr>    <int>  <int>
     1 P11774     242    493
     2 P31579     203    446
     3 P33531     185    455
     4 P36987     267    503
     5 P40725     226    434
     6 P40923     260    477
     7 P51467     180    361
     8 P51956     208    483
     9 P52630     290    522
    10 P54631     174    378
    11 P60426     173    394
    12 P66101     183    439
    13 P68071     184    386
    14 P69489     201    436
    15 P70597     167    382
    16 P75656     239    470
    17 P84567     194    400
    18 P94346     318    506
    19 P95158     290    520
    20 P96518     210    432


#### EMP_GSEA_analysis

Based on cor analysis

```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='cor',
                    estimate_group = 'BMI',cor_method = 'spearman',
                    threshold_r = 0.3,threshold_p = 0.05, ###### filter by coe and pvalue
                    pvalueCutoff = 0.05,keyType = 'ko')
```
    Reading KEGG annotation online: "https://rest.kegg.jp/link/ko/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    preparing geneSet collections...
    GSEA analysis...
    leading edge analysis...
    done...
    Correlation analysis based on BMI
    The correlation analysis values are arranged in descending order.
    # A tibble: 2 × 11
      ID       Description  setSize enrichmentScore   NES  pvalue p.adjust  qvalue  rank leading_edge
      <chr>    <chr>          <int>           <dbl> <dbl>   <dbl>    <dbl>   <dbl> <dbl> <chr>       
    1 map02010 ABC transpo…      25           0.417  2.26 9.30e-4  0.00465 0.00294    81 tags=68%, l…
    2 map01100 Metabolic p…      65          -0.242 -2.03 4.32e-3  0.0108  0.00683    76 tags=51%, l…
    # ℹ 1 more variable: core_enrichment <chr>


Based on diff analysis

```R
MAE |>
  EMP_diff_analysis(experiment = 'geno_ec',method='DESeq2',.formula = ~0+Group,
                    group_level=c('Group_A','Group_B')) |>
  EMP_GSEA_analysis(method='log2FC',pvalue<0.05,
                    keyType = 'ec',KEGG_Type = 'KEGG',pvalueCutoff = 0.05)
```
    Reading KEGG annotation online: "https://rest.kegg.jp/link/ec/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    preparing geneSet collections...
    GSEA analysis...
    leading edge analysis...
    done...
    VS info: Group_A vs Group_B
    The log2FC values are arranged in descending order.
    # A tibble: 1 × 11
      ID       Description   setSize enrichmentScore   NES  pvalue p.adjust qvalue  rank leading_edge
      <chr>    <chr>           <int>           <dbl> <dbl>   <dbl>    <dbl>  <dbl> <dbl> <chr>       
    1 map01110 Biosynthesis…      46          -0.393 -1.64 0.00901   0.0451 0.0379    71 tags=41%, l…
    # ℹ 1 more variable: core_enrichment <chr>


Based on signal2Noise

```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko')
```
    Reading KEGG annotation online: "https://rest.kegg.jp/link/ko/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    preparing geneSet collections...
    GSEA analysis...
    leading edge analysis...
    done...
    VS info: Group_A vs Group_B
    The Signal2Noise values are arranged in descending order.
    # A tibble: 23 × 11
       ID      Description setSize enrichmentScore   NES   pvalue p.adjust  qvalue  rank leading_edge
       <chr>   <chr>         <int>           <dbl> <dbl>    <dbl>    <dbl>   <dbl> <dbl> <chr>       
     1 map006… Methane me…     138           0.708  2.19 1   e-10  1.95e-8 1.74e-8   599 tags=31%, l…
     2 map030… ATP-depend…      11          -0.942 -1.88 2.17e- 7  2.12e-5 1.89e-5   419 tags=82%, l…
     3 map050… Alcoholism       12          -0.931 -1.88 6.40e- 7  4.16e-5 3.71e-5   419 tags=67%, l…
     4 map050… Huntington…      52          -0.754 -2.01 1.54e- 6  6.02e-5 5.37e-5   493 tags=46%, l…
     5 map012… Carbon met…     253           0.530  1.75 1.45e- 6  6.02e-5 5.37e-5   599 tags=17%, l…
     6 map046… Neutrophil…      10          -0.932 -1.80 5.15e- 6  1.67e-4 1.49e-4   419 tags=70%, l…
     7 map050… Amyotrophi…      49          -0.719 -1.89 1.48e- 4  4.12e-3 3.67e-3   493 tags=43%, l…
     8 map041… Endocytosis      16          -0.839 -1.79 5.69e- 4  1.23e-2 1.10e-2   458 tags=75%, l…
     9 map050… Alzheimer …      45          -0.687 -1.79 5.39e- 4  1.23e-2 1.10e-2   493 tags=42%, l…
    10 map030… Nucleocyto…      14          -0.839 -1.74 8.18e- 4  1.59e-2 1.42e-2   361 tags=64%, l…
    11 map050… Pathways o…      53          -0.665 -1.77 1.09e- 3  1.94e-2 1.72e-2   493 tags=40%, l…
    12 map050… Prion dise…      43          -0.670 -1.73 1.50e- 3  2.44e-2 2.17e-2   493 tags=40%, l…
    13 map041… Efferocyto…      12          -0.843 -1.70 1.67e- 3  2.50e-2 2.23e-2   290 tags=42%, l…
    14 map040… MAPK signa…      11          -0.834 -1.66 2.89e- 3  4.02e-2 3.58e-2   487 tags=91%, l…
    15 map047… Thermogene…      42          -0.675 -1.74 3.44e- 3  4.47e-2 3.98e-2   417 tags=36%, l…
    16 map009… Aminoacyl-…      36           0.669  1.67 3.75e- 3  4.57e-2 4.07e-2  1801 tags=58%, l…
    17 map050… Spinocereb…      17          -0.790 -1.71 5.29e- 3  4.81e-2 4.29e-2   425 tags=65%, l…
    18 map051… Pathogenic…      15          -0.794 -1.68 5.68e- 3  4.81e-2 4.29e-2   493 tags=47%, l…
    19 map047… Synaptic v…      12          -0.820 -1.66 5.09e- 3  4.81e-2 4.29e-2   440 tags=75%, l…
    20 map054… Diabetic c…      39          -0.650 -1.65 5.51e- 3  4.81e-2 4.29e-2   417 tags=28%, l…
    21 map041… Meiosis - …      11          -0.819 -1.63 5.16e- 3  4.81e-2 4.29e-2   417 tags=73%, l…
    22 map012… Degradatio…      69          -0.577 -1.59 5.53e- 3  4.81e-2 4.29e-2   471 tags=12%, l…
    23 map007… Carbon fix…      75           0.545  1.55 5.01e- 3  4.81e-2 4.29e-2   723 tags=20%, l…
    # ℹ 1 more variable: core_enrichment <chr>

Visualization

```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko') |>
  EMP_curveplot(geneSetID='map00680')
```
    Reading KEGG annotation online: "https://rest.kegg.jp/link/ko/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    preparing geneSet collections...
    GSEA analysis...
    leading edge analysis...
    done...
    VS info: Group_A vs Group_B
    The Signal2Noise values are arranged in descending order.

<img src="tutorial_related/tutorial_figs/gsea1.jpg" alt="gsea1" style="zoom:100%;" />

```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko') |>
  EMP_dotplot(color='p.adjust',showCategory=10) 
```
<img src="tutorial_related/tutorial_figs/gsea2.jpg" alt="gsea2" style="zoom:100%;" />


```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko') |>
  EMP_netplot(showCategory=5) 
```

<img src="tutorial_related/tutorial_figs/gsea3.jpg" alt="gsea3" style="zoom:100%;" />

#### EMP_WGCNA_cluster_analysis

```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85)
```

    Module
           black         blue        brown         cyan    darkgreen      darkred        green 
             116          291          248           61           31           40          140 
     greenyellow         grey       grey60    lightcyan   lightgreen  lightyellow      magenta 
              75           73           51           58           48           43          107 
    midnightblue         pink       purple          red    royalblue       salmon          tan 
              59          110           80          126           43           71           72 
       turquoise       yellow 
             304          177 
    # A tibble: 2,424 × 4
       feature   WGCNA_cluster WGCNA_color WGCNA_module_elements
       <chr>             <dbl> <chr>                       <int>
     1 1.1.1.1               1 turquoise                     304
     2 1.1.1.100             0 grey                           73
     3 1.1.1.102            13 salmon                         71
     4 1.1.1.103            15 cyan                           61
     5 1.1.1.105            16 lightcyan                      58
     6 1.1.1.108             0 grey                           73
     7 1.1.1.11              9 green                         140
     8 1.1.1.122            18 grey60                         51
     9 1.1.1.130             1 turquoise                     304
    10 1.1.1.132            20 lightyellow                    43
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

<img src="tutorial_related/tutorial_figs/wgcna_clust1.jpg" alt="wgcna_clust1" style="zoom:100%;" />


```R
MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4) # adjust parameter to modify the module result
```
    Module
           black         blue        brown         cyan        green  greenyellow         grey 
             126          304          291           59          147           74           73 
          grey60    lightcyan   lightgreen      magenta midnightblue         pink       purple 
              48           51           43          116           58          120          107 
             red       salmon          tan    turquoise       yellow 
             140           61           71          358          177 
    # A tibble: 2,424 × 4
       feature   WGCNA_cluster WGCNA_color  WGCNA_module_elements
       <chr>             <dbl> <chr>                        <int>
     1 1.1.1.1               1 blue                           304
     2 1.1.1.100             0 grey                            73
     3 1.1.1.102            13 tan                             71
     4 1.1.1.103            15 salmon                          61
     5 1.1.1.105            16 midnightblue                    58
     6 1.1.1.108             0 grey                            73
     7 1.1.1.11              9 red                            140
     8 1.1.1.122            18 lightcyan                       51
     9 1.1.1.130             1 blue                           304
    10 1.1.1.132            20 greenyellow                     74
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

<img src="tutorial_related/tutorial_figs/wgcna_clust2.jpg" alt="wgcna_clust2" style="zoom:100%;" />



#### EMP_WGCNA_cor_analysis

From one experiment

```R
WGCNA_COR_result <-MAE |>
  EMP_assay_extract('geno_ec')  |> 
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4)  |>
  EMP_WGCNA_cor_analysis(coldata_to_assay = c('BMI','PHQ9','GAD7','HAMD','SAS','SDS'),
                         method='spearman') 
WGCNA_COR_result
```

    EMP_WGCNA_cor_analysis: 
    Cor-relationship matrix: 6 x 10 
    geno_ec observation: 20 
    geno_ec_codata observation: 20 
    Intersect observation: 20

Visualization

```R
MAE |>
  EMP_assay_extract('geno_ec')  |> 
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4)  |>
  EMP_WGCNA_cor_analysis(coldata_to_assay = c('BMI','PHQ9','GAD7','HAMD','SAS','SDS'),
                         method='spearman') |>
  EMP_heatmap_plot(palette = 'Spectral')
```

<img src="tutorial_related/tutorial_figs/wgcna_cor1.jpg" alt="wgcna_cor1" style="zoom:100%;" />

Select the interesting module and make the enrichment analysis

```R
MAE |>
  EMP_assay_extract('geno_ec')  |> 
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4)  |>
  EMP_WGCNA_cor_analysis(coldata_to_assay = c('BMI','PHQ9','GAD7','HAMD','SAS','SDS'),
                         method='spearman') |>
  EMP_heatmap_plot(palette = 'Spectral') |>
  EMP_filter(feature_condition = WGCNA_color == 'brown' ) |> 
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |>
  EMP_enrich_analysis(keyType = 'ec',KEGG_Type = 'MKEGG') |>
  EMP_dotplot()
```

    If any features in the experiment have changed, the feature_WGCNA_cluster_result will become NULL
    and EMP_WGCNA_cluster_analysis should be re-run if needed.
    Because condtion has changed, all plot results will be removed!
    0 of 20 samples were filterd out!
    1170 of 1301 features were filterd out!
    Reading KEGG annotation online: "https://rest.kegg.jp/link/ec/module"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/module"...
    KEGG database version: Release 110.0+/04-25, Apr 24
    Species: all

<img src="tutorial_related/tutorial_figs/wgcna_cor2.jpg" alt="wgcna_cor2" style="zoom:100%;" />


From two different experiments

```R
k1 <- MAE |>
  EMP_assay_extract('geno_ec')  |> 
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4)

k2 <- MAE |>
  EMP_assay_extract('host_gene',pattern = c('A1BG','A1CF','A2MP1','AACS'),pattern_ref = 'feature')

(k1 + k2) |>
  EMP_WGCNA_cor_analysis(method='spearman') |>
  EMP_heatmap_plot(palette = 'Spectral') 
```
<img src="tutorial_related/tutorial_figs/wgcna_cor3.jpg" alt="wgcna_cor3" style="zoom:100%;" />


#### EMP_multi_analysis

Prepare the results

```R
k1 <- MAE |> 
  EMP_assay_extract('geno_ec') |> 
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |> 
  EMP_enrich_analysis(pvalue < 0.05,keyType = 'ec')

# Use different differential analysis methods to mimic the statistical results of the same feature across different cohorts.
k2 <- MAE |> 
  EMP_assay_extract('geno_ec') |> 
  EMP_diff_analysis(method = 'wilcox.test',estimate_group = 'Group') |>
  EMP_enrich_analysis(pvalue < 0.05,keyType = 'ec')

k3 <- MAE |> 
  EMP_assay_extract('geno_ko') |> 
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |> 
  EMP_enrich_analysis(pvalue < 0.05,keyType = 'ko')

k4 <- MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |>
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |> 
  EMP_enrich_analysis(pvalue < 0.05,keyType = 'cpd')
```

Melt diff analysis of the same feature from two result

```R
(k1+k2) |> 
  EMP_multi_analysis(method = 'feature',combineMethod='edgington',p.adjust = 'BH') 
```

    # A tibble: 2,424 × 3
       feature   pvalue    BH
       <chr>      <dbl> <dbl>
     1 1.1.1.1   0.838  1.00 
     2 1.1.1.100 0.0714 0.371
     3 1.1.1.102 0.734  1.00 
     4 1.1.1.103 0.431  0.957
     5 1.1.1.105 0.244  0.710
     6 1.1.1.108 0.0218 0.192
     7 1.1.1.11  0.770  1.00 
     8 1.1.1.122 0.491  1.00 
     9 1.1.1.130 0.121  0.496
    10 1.1.1.132 0.943  1.00 
    # ℹ 2,414 more rows
    # ℹ Use `print(n = ...)` to see more rows

Melt diff analysis of the same feature from two result and make a combine enrichment

```R
(k1+k2) |> 
  EMP_multi_analysis(method = 'same_feature_enrich',keyType = 'ec',combineFun='ActivePathways') 
```

    Reading KEGG annotation online: "https://rest.kegg.jp/link/ec/pathway"...
    Reading KEGG annotation online: "https://rest.kegg.jp/list/pathway"...
    685 rows were removed from scores because they are not found in the background
    743 rows were removed from scores because they are not found in the background
    # A tibble: 15 × 10
       Cluster ID       Description                      GeneRatio BgRatio  pvalue p.adjust  qvalue geneID Count
       <fct>   <chr>    <chr>                            <chr>     <chr>     <dbl>    <dbl>   <dbl> <chr>  <dbl>
     1 Group_A map00785 Lipoic acid metabolism           3/1546    14/3825 6.62e-5  4.95e-3 4.51e-3 1.2.4…     3
     2 Group_A map00500 Starch and sucrose metabolism    3/1546    76/3825 7.33e-5  4.95e-3 4.51e-3 2.4.1…     3
     3 Group_A map00020 Citrate cycle (TCA cycle)        3/1546    26/3825 2.13e-4  7.50e-3 6.84e-3 6.2.1…     3
     4 Group_A map00640 Propanoate metabolism            4/1546    52/3825 2.22e-4  7.50e-3 6.84e-3 6.2.1…     4
     5 Group_A map00531 Glycosaminoglycan degradation    3/1546    15/3825 5.48e-4  1.48e-2 1.35e-2 3.2.1…     3
     6 Group_A map00280 Valine, leucine and isoleucine … 3/1546    36/3825 1.19e-3  2.68e-2 2.45e-2 1.2.4…     3
     7 Group_A map00010 Glycolysis / Gluconeogenesis     4/1546    50/3825 1.83e-3  3.54e-2 3.23e-2 1.2.7…     4
     8 Group_B map00550 Peptidoglycan biosynthesis       7/1674    23/3825 5.63e-9  7.60e-7 6.81e-7 2.3.2…     7
     9 Group_B map00785 Lipoic acid metabolism           4/1674    14/3825 3.97e-5  2.68e-3 2.40e-3 1.2.4…     4
    10 Group_B map00010 Glycolysis / Gluconeogenesis     3/1674    50/3825 2.37e-4  1.06e-2 9.54e-3 2.7.9…     3
    11 Group_B map00640 Propanoate metabolism            5/1674    52/3825 5.09e-4  1.72e-2 1.54e-2 1.2.7…     5
    12 Group_B map00710 Carbon fixation in photosynthet… 2/1674    22/3825 6.52e-4  1.76e-2 1.58e-2 2.7.9…     2
    13 Group_B map00500 Starch and sucrose metabolism    5/1674    76/3825 1.04e-3  2.16e-2 1.93e-2 2.4.1…     5
    14 Group_B map00280 Valine, leucine and isoleucine … 4/1674    36/3825 1.12e-3  2.16e-2 1.93e-2 1.2.4…     4
    15 Group_B map00720 Carbon fixation pathways in pro… 3/1674    54/3825 2.20e-3  3.71e-2 3.32e-2 2.7.9…     3

 Melt diff analysis of the different feature from multi-results and make a combine enrichment

```R
(k1+k4) |> 
  EMP_multi_analysis(method = 'diff_feature_enrich')
```

    # A tibble: 29 × 11
       Cluster sign_group ID       Description           GeneRatio BgRatio  pvalue p.adjust  qvalue geneID Count
       <fct>   <chr>      <chr>    <chr>                 <chr>     <chr>     <dbl>    <dbl>   <dbl> <chr>  <int>
     1 Group_A Group_A    map00785 Lipoic acid metaboli… 6/131     14/3825 4.23e-7  6.34e-6 2.49e-5 1.2.4…     6
     2 Group_A Group_A    map00531 Glycosaminoglycan de… 4/131     15/3825 3.49e-4  2.62e-3 1.03e-2 3.1.6…     4
     3 Group_A Group_A    map00520 Amino sugar and nucl… 10/131    127/38… 8.15e-4  4.08e-3 1.60e-2 2.6.1…    10
     4 Group_A Group_A    map00280 Valine, leucine and … 5/131     36/3825 1.51e-3  4.53e-3 2.22e-2 1.2.4…     5
     5 Group_A Group_A    map00030 Pentose phosphate pa… 6/131     59/3825 2.65e-3  5.69e-3 3.13e-2 1.1.1…     6
     6 Group_A Group_A    map00564 Glycerophospholipid … 2/131     56/6430 1.09e-3  4.10e-3 6.90e-3 C0423…     2
     7 Group_A Group_A    map00627 Aminobenzoate degrad… 2/131     86/6430 2.56e-3  5.69e-3 8.09e-3 C0014…     2
     8 Group_A Group_A    map04979 Cholesterol metaboli… 1/131     10/6430 9.30e-3  1.62e-2 1.17e-2 C00422     1
     9 Group_A Group_A    map05231 Choline metabolism i… 1/131     11/6430 1.02e-2  1.62e-2 1.17e-2 C04230     1
    10 Group_A Group_A    map04975 Fat digestion and ab… 1/131     13/6430 1.21e-2  1.62e-2 1.17e-2 C00422     1
    11 Group_A Group_A    map04923 Regulation of lipoly… 1/131     14/6430 1.30e-2  1.62e-2 1.17e-2 C00422     1
    12 Group_A Group_A    map05417 Lipid and atheroscle… 1/131     14/6430 1.30e-2  1.62e-2 1.17e-2 C00422     1
    13 Group_A Group_A    map04931 Insulin resistance    1/131     19/6430 1.76e-2  2.03e-2 1.34e-2 C00422     1
    14 Group_A Group_A    map04148 Efferocytosis         1/131     21/6430 1.94e-2  2.08e-2 1.34e-2 C04230     1
    15 Group_A Group_A    map04714 Thermogenesis         1/131     23/6430 2.13e-2  2.13e-2 1.34e-2 C00422     1
    16 Group_B Group_B    map00970 Aminoacyl-tRNA biosy… 9/128     31/3825 5.35e-9  5.97e-8 2.05e-7 6.1.1…     9
    17 Group_B Group_B    map00550 Peptidoglycan biosyn… 8/128     23/3825 7.96e-9  5.97e-8 2.05e-7 2.3.2…     8
    18 Group_B Group_B    map00730 Thiamine metabolism   5/128     28/3825 2.11e-4  4.52e-4 3.63e-3 2.2.1…     5
    19 Group_B Group_B    map00250 Alanine, aspartate a… 6/128     50/3825 4.61e-4  8.64e-4 5.94e-3 1.4.1…     6
    20 Group_B Group_B    map04136 Autophagy - other     2/128     3/6430  5.22e-6  1.96e-5 6.05e-5 C0035…     2
    21 Group_B Group_B    map04138 Autophagy - yeast     2/128     3/6430  5.22e-6  1.96e-5 6.05e-5 C0035…     2
    22 Group_B Group_B    map00563 Glycosylphosphatidyl… 2/128     4/6430  1.04e-5  3.13e-5 8.06e-5 C0035…     2
    23 Group_B Group_B    map04140 Autophagy - animal    2/128     6/6430  2.60e-5  6.51e-5 1.51e-4 C0035…     2
    24 Group_B Group_B    map00571 Lipoarabinomannan (L… 1/128     2/6430  2.80e-3  4.20e-3 1.08e-2 C01194     1
    25 Group_B Group_B    map05130 Pathogenic Escherich… 1/128     3/6430  4.19e-3  5.72e-3 1.39e-2 C00350     1
    26 Group_B Group_B    map05132 Salmonella infection  1/128     5/6430  6.98e-3  8.06e-3 1.80e-2 C01194     1
    27 Group_B Group_B    map05167 Kaposi sarcoma-assoc… 1/128     5/6430  6.98e-3  8.06e-3 1.80e-2 C00350     1
    28 Group_B Group_B    map00402 Benzoxazinoid biosyn… 1/128     9/6430  1.25e-2  1.25e-2 2.64e-2 C12312     1
    29 Group_B Group_B    map05152 Tuberculosis          1/128     9/6430  1.25e-2  1.25e-2 2.64e-2 C01194     1

Visualization

```R
(k1+k2) |> 
  EMP_multi_analysis(method = 'same_feature_enrich',keyType = 'ec',combineFun='ActivePathways') |>
  EMP_dotplot()
```
<img src="tutorial_related/tutorial_figs/multi_analysis1.jpg" alt="multi_analysis1" style="zoom:100%;" />


### Data Virtualization :
#### EMP_boxplot

For assay

```R
MAE |> 
  EMP_assay_extract('host_gene',pattern = 'A1BG',pattern_ref = 'feature') |>
  EMP_boxplot(method='t.test',estimate_group='Group')
```
<img src="tutorial_related/tutorial_figs/boxplot1.jpg" alt="boxplot1" style="zoom:100%;" />


For alpha analysis

```R
MAE |> 
  EMP_assay_extract('taxonomy') |> 
  EMP_alpha_analysis() |>
  EMP_boxplot(method='t.test',estimate_group='Group') 
```

<img src="tutorial_related/tutorial_figs/boxplot2.jpg" alt="boxplot2" style="zoom:100%;" />


#### EMP_scatterplot

```R
MAE |> 
 EMP_assay_extract('taxonomy') |> 
 EMP_collapse(estimate_group = 'Species',collapse_by = 'row') |>
 EMP_dimension_analysis(method = 'pcoa',distance = 'bray',estimate_group = 'Group') |>
 EMP_scatterplot(show='p12html') ### eg. p12,p12html,p23,p23htm
```
<img src="tutorial_related/tutorial_figs/scatterplot1.jpg" alt="scatterplot1" style="zoom:100%;" />


#### EMP_dotplot

```R
MAE |> 
  EMP_assay_extract('geno_ko') |> 
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |> 
  EMP_enrich_analysis(pvalue < 0.05,keyType = 'ko') |>
  EMP_dotplot(showCategory=5,x = "GeneRatio",color = "p.adjust") # more paramter will pass into enrichplot::dotplot
```

<img src="tutorial_related/tutorial_figs/dotplot.jpg" alt="dotplot" style="zoom:100%;" />


#### EMP_netplot

```R
MAE |>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko') |>
  EMP_netplot(showCategory=5) 
```
<img src="tutorial_related/tutorial_figs/netplot.jpg" alt="netplot" style="zoom:100%;" />



#### EMP_curveplot

```R
MAE|>
  EMP_GSEA_analysis(experiment = 'geno_ko',method='signal2Noise',
                    estimate_group = 'Group',
                    pvalueCutoff = 0.05,keyType = 'ko') |>
  EMP_curveplot(geneSetID='map03082')
```
<img src="tutorial_related/tutorial_figs/curveplot1.jpg" alt="curveplot1" style="zoom:100%;" />


#### EMP_heatmap_plot

For assay
```R
MAE %>%
  EMP_assay_extract('geno_ec') %>%
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) %>%
  EMP_filter(feature_condition = pvalue<0.05 & abs(fold_change) >3.5) %>% # Select your interested feature
  EMP_decostand(method = 'clr') %>%
  EMP_heatmap_plot(rotate=FALSE,palette='Spectral')
```
<img src="tutorial_related/tutorial_figs/heatmap1.jpg" alt="heatmap1" style="zoom:100%;" />


```R
MAE %>%
  EMP_assay_extract('geno_ec') %>%
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) %>%
  EMP_filter(feature_condition = pvalue<0.05 & abs(fold_change) >3.5) %>%
  EMP_collapse(estimate_group = 'Group',collapse_by = 'col') %>% # collapse the data by group
  EMP_heatmap_plot(rotate=TRUE,palette='Spectral')
```
<img src="tutorial_related/tutorial_figs/heatmap2.jpg" alt="heatmap2" style="zoom:100%;" />


For cor analysis

```R
k1 <- MAE |>
  EMP_assay_extract('geno_ec') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.001 & abs(fold_change)>2)

k2 <- MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.05 & abs(fold_change) > 1.5)

(k1 + k2) |> EMP_cor_analysis(method = 'spearman') |>
  EMP_heatmap_plot(palette='RdBu') ###### Visualization
```
<img src="tutorial_related/tutorial_figs/heatmap3.jpg" alt="heatmap3" style="zoom:100%;" />


For WGCNA

````R
MAE |>
  EMP_assay_extract('geno_ec')  |> 
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_WGCNA_cluster_analysis(RsquaredCut = 0.85,mergeCutHeight=0.4)  |>
  EMP_WGCNA_cor_analysis(coldata_to_assay = c('BMI','PHQ9','GAD7','HAMD','SAS','SDS'),method='spearman') |>
  EMP_heatmap_plot(palette = 'Spectral') 
````

<img src="tutorial_related/tutorial_figs/wgcna_cor1.jpg" alt="wgcna_cor1" style="zoom:100%;" />


#### EMP_volcanol_plot

```R
MAE |>
  EMP_assay_extract(experiment = 'geno_ec') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group)  |>
  EMP_volcanol_plot(show='html')
```
<img src="tutorial_related/tutorial_figs/vocanol1.jpg" alt="vocanol1" style="zoom:100%;" />


Addtionl parameters will pass into ggrepel::geom_text_repel.

```R
MAE |>
  EMP_decostand(experiment = 'geno_ec',method = 'integer') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group)  |>
  EMP_volcanol_plot(key_feature = c('3.6.1.62','1.5.3.19'),color = "white",
                    bg.color = "grey30",bg.r = 0.15)
```
<img src="tutorial_related/tutorial_figs/vocanol2.jpg" alt="vocanol2" style="zoom:100%;" />

```R
MAE |>
  EMP_decostand(experiment = 'geno_ec',method = 'integer') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group)  |>
  EMP_volcanol_plot(key_feature = c('3.6.1.62','1.5.3.19'),
                    min.segment.length = 0, seed = 42, box.padding = 0.5) ## Add arrow
```
<img src="tutorial_related/tutorial_figs/vocanol3.jpg" alt="vocanol3" style="zoom:100%;" />


### Data support :


#### EMP_filter

For MultiAssayExperiment

```R
MAE |>
  EMP_filter(sample_condition = BMI>20 & Sex == 'M') |>
  EMP_summary()
```
<img src="tutorial_related/tutorial_figs/EMP_summary1.jpg" alt="EMP_summary1" style="zoom:100%;" />


For EMPT

```R
MAE |>
  EMP_assay_extract(experiment = 'host_gene') |>
  EMP_identify_assay(method = 'edgeR',estimate_group = 'Group') |>
  EMP_diff_analysis(method='DESeq2',.formula = ~Group) |>
  EMP_filter(feature_condition = fdr < 0.05)
```
    0 of 20 samples were filterd out!
    270 of 272 features were filterd out!
    # A tibble: 2 × 9
      feature Estimate_group    pvalue     fdr sign_group method vs                 fold_change log2FC
      <chr>   <chr>              <dbl>   <dbl> <fct>      <chr>  <chr>                    <dbl>  <dbl>
    1 ACVR1C  Group          0.0000127 0.00345 Group_A    DESeq2 Group_A vs Group_B       8.29    3.05
    2 ACY3    Group          0.000240  0.0326  Group_B    DESeq2 Group_A vs Group_B       0.105  -3.26

```R
MAE |>
  EMP_assay_extract(experiment = 'taxonomy') |>
  EMP_alpha_analysis() |>
  EMP_filter(sample_condition = shannon >3 | invsimpson >19)
```

    # A tibble: 16 × 7
       primary shannon simpson invsimpson observerd_index chao1   ACE
       <chr>     <dbl>   <dbl>      <dbl>           <dbl> <dbl> <dbl>
     1 P11774     3.63   0.948       19.0             172   172   172
     2 P31579     3.83   0.955       22.0             184   184   184
     3 P33531     3.54   0.938       16.1             168   168   168
     4 P40725     4.05   0.953       21.3             238   238   238
     5 P40923     3.33   0.918       12.2             180   180   180
     6 P51467     3.78   0.956       22.5             195   195   195
     7 P51956     4.40   0.975       40.0             278   278   278
     8 P52630     4.12   0.974       38.5             178   178   178
     9 P60426     3.81   0.951       20.6             191   191   191
    10 P66101     3.10   0.912       11.4              98    98    98
    11 P68071     3.57   0.939       16.3             170   170   170
    12 P69489     4.21   0.970       33.0             238   238   238
    13 P70597     3.56   0.935       15.4             146   146   146
    14 P94346     3.44   0.911       11.3             224   224   224
    15 P95158     3.98   0.961       25.8             231   231   231
    16 P96518     4.03   0.968       31.7             222   222   222

Precise selection

```R
MAE |>
  EMP_assay_extract(experiment = 'taxonomy') |>
  EMP_alpha_analysis() |>
  EMP_filter(sample_condition = shannon >3 | invsimpson >19,
             filterSample = c('P11774','P31579'),
             action = 'kick') # Accurately kick samples after sample_condition
```

    # A tibble: 14 × 7
       primary shannon simpson invsimpson observerd_index chao1   ACE
       <chr>     <dbl>   <dbl>      <dbl>           <dbl> <dbl> <dbl>
     1 P33531     3.54   0.938       16.1             168   168   168
     2 P40725     4.05   0.953       21.3             238   238   238
     3 P40923     3.33   0.918       12.2             180   180   180
     4 P51467     3.78   0.956       22.5             195   195   195
     5 P51956     4.40   0.975       40.0             278   278   278
     6 P52630     4.12   0.974       38.5             178   178   178
     7 P60426     3.81   0.951       20.6             191   191   191
     8 P66101     3.10   0.912       11.4              98    98    98
     9 P68071     3.57   0.939       16.3             170   170   170
    10 P69489     4.21   0.970       33.0             238   238   238
    11 P70597     3.56   0.935       15.4             146   146   146
    12 P94346     3.44   0.911       11.3             224   224   224
    13 P95158     3.98   0.961       25.8             231   231   231
    14 P96518     4.03   0.968       31.7             222   222   222

#### EMP_history

from EMPT

```R
MAE |>
  EMP_assay_extract(experiment = 'taxonomy') |>
  EMP_alpha_analysis() |>
  EMP_history()
```
    [1] ".EMP_assay_extract_EMPT(EMPT = ., pattern_ref = pattern_ref, pattern = pattern, exact = exact, action = add)"
    [2] "EMP_alpha_analysis(x = EMP_assay_extract(MAE, experiment = taxonomy))"   


from EMP

```R
k1 <- MAE |>
  EMP_assay_extract('taxonomy') |>
  EMP_collapse(estimate_group = 'Genus',collapse_by = 'row') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.05)

k2 <- MAE |>
  EMP_collapse(experiment = 'untarget_metabol',na_string=c('NA','null','','-'),
               estimate_group = 'MS2kegg',method = 'sum',collapse_by = 'row') |>
  EMP_diff_analysis(method='DESeq2', .formula = ~Group) |>
  EMP_filter(feature_condition = pvalue<0.05 & abs(fold_change) > 1.5)

(k1 + k2) |> EMP_cor_analysis(method = 'spearman') |>
  EMP_heatmap_plot()  |>
  EMP_history()  ###### get all the history about what happened to the object
```

    $total
    [1] "as.EMP(object = data_list)"                          
    [2] "EMP_cor_analysis(EMP = (k1 + k2), method = spearman)"
    [3] ".Primitive(call)"                                    

    $taxonomy
    [1] ".EMP_assay_extract_EMPT(EMPT = ., pattern_ref = pattern_ref, pattern = pattern, exact = exact, action = add)"                                                                                         
    [2] "EMP_collapse_byrow(x = x, experiment = experiment, estimate_group = estimate_group, method = method, na_string = na_string, collapse_sep = collapse_sep, action = action)"                            
    [3] "EMP_diff_analysis(x = EMP_collapse(EMP_assay_extract(MAE, taxonomy), estimate_group = Genus, collapse_by = row), .formula = ~Group, method = DESeq2)"                                                 
    [4] "EMP_filter(obj = EMP_diff_analysis(EMP_collapse(EMP_assay_extract(MAE, taxonomy), estimate_group = Genus, collapse_by = row), method = DESeq2, .formula = ~Group), feature_condition = pvalue < 0.05)"

    $untarget_metabol
    [1] "EMP_collapse_byrow(x = x, experiment = experiment, estimate_group = estimate_group, method = method, na_string = na_string, collapse_sep = collapse_sep, action = action)"                                                                                                     
    [2] "EMP_diff_analysis(x = EMP_collapse(MAE, experiment = untarget_metabol, na_string = c(NA, null, , -), estimate_group = MS2kegg, method = sum, collapse_by = row), .formula = ~Group, method = DESeq2)"                                                                          
    [3] "EMP_filter(obj = EMP_diff_analysis(EMP_collapse(MAE, experiment = untarget_metabol, na_string = c(NA, null, , -), estimate_group = MS2kegg, method = sum, collapse_by = row), method = DESeq2, .formula = ~Group), feature_condition = pvalue < 0.05 & abs(fold_change) > 1.5)"


#### EMP_result

Obtain the result from EMPT

```R
MAE |> 
  EMP_assay_extract('geno_ec') |>
  EMP_alpha_analysis()|> 
  EMP_diff_analysis(method = 'DESeq2',.formula = ~Group) |>
  EMP_enrich_analysis(pvalue<0.05,keyType='ec',pvalueCutoff=0.05) -> result

diff_re <- result |> EMP_result(info = 'EMP_diff_analysis')
alpha_re <- result |> EMP_result(info = 'EMP_alpha_analysis')
enrich_re <- result |> EMP_result(info = 'EMP_enrich_analysis')
```

Inject external result into EMPT

1. Get a EMPT object

```R
MAE |> 
  EMP_assay_extract('taxonomy') |>
  EMP_collapse(estimate_group = 'Genus',method = 'sum',
               collapse_by = 'row',action = 'add') -> obj  
```

2. Get the raw data from the EMPT

```R 
MAE |> 
  EMP_assay_extract('taxonomy') |>
  EMP_collapse(estimate_group = 'Genus',method = 'sum',
               collapse_by = 'row',action = 'get') -> assay_data  
```

3. Caculate the result from other packages

```R
assay_data <- assay_data |> tibble::column_to_rownames('primary')
shannon_index <- vegan::diversity(assay_data,index = 'shannon') 
new_result <- tibble::tibble(primary=names(shannon_index),new_shannon=shannon_index)
new_result
```
    # A tibble: 20 × 2
       primary new_shannon
       <chr>         <dbl>
     1 P11774         2.28
     2 P31579         2.55
     3 P33531         2.19
     4 P36987         2.02
     5 P40725         3.11
     6 P40923         2.15
     7 P51467         2.36
     8 P51956         3.01
     9 P52630         2.80
    10 P54631         1.74
    11 P60426         2.73
    12 P66101         1.47
    13 P68071         2.32
    14 P69489         2.71
    15 P70597         2.24
    16 P75656         1.50
    17 P84567         1.96
    18 P94346         2.93
    19 P95158         2.76
    20 P96518         2.59

4. Inject the new result into EMPT object

   NOTE: When injecting external result into EMPT, users need read the   parameter carefully to make sure EMP_filter work correctly

```R
EMP_result(obj,
           value_name = 'new_alpha',
           affect_when_sample_changed=0,
           affect_when_feature_changed=1,
           attribute='primary',
           attribute2='normal',source='user_import') <- new_result

obj |> 
  EMP_filter(sample_condition  = new_shannon >2)
```

    4 of 20 samples were filterd out!
    0 of 217 features were filterd out!
    # A tibble: 16 × 218
       primary Acetoanaerobium Acetobacterium Acidaminococcus Adlercreutzia Akkermansia Algoriphagus
       <chr>             <int>          <int>           <int>         <int>       <int>        <int>
     1 P11774                0              0               4             0           9            0
     2 P31579                0              3               0             0           4            0
     3 P33531                0              0               0             0           7            0
     4 P36987                0              0              14             0           8            0
     5 P40725                3              5              10             0           4            0
     6 P40923                0              0               0             0           4            0
     7 P51467                0              0               0             0           5            2
     8 P51956                2              4              19             8          72            2
     9 P52630                0              0               6             0         167            0
    10 P60426                0              3               2             0           4            0
    11 P68071                0              0               2             0           8            0
    12 P69489                0              3               6             6           6            0
    13 P70597                0              0               0             0           3            0
    14 P94346                2              7              10             0           3            0
    15 P95158                2              6               5             0           4            0
    16 P96518                0              0               7             0           5            3
    # ℹ 211 more variables: Alistipes <int>, Alkaliphilus <int>, Alkalitalea <int>,
    #   Alloprevotella <int>, Amedibacterium <int>, Aminipila <int>, Anaerobutyricum <int>,
    #   Anaerocolumna <int>, Anaerostipes <int>, Anaerotignum <int>, Anoxybacter <int>,
    #   Anseongella <int>, Apibacter <int>, Arachidicoccus <int>, Arenibacter <int>, Bacteroides <int>,
    #   Barnesiella <int>, Belliella <int>, Bifidobacterium <int>, Blautia <int>, Bulleidia <int>,
    #   Butyricimonas <int>, Butyrivibrio <int>, Caldithrix <int>, Caloramator <int>,
    #   Caloranaerobacter <int>, Calothrix <int>, Campylobacter <int>, …
    # ℹ Use `colnames()` to see all variable names

#### EMP_summary
```R
## from MultiAssayExperiment
MAE |>
 EMP_summary()
## from EMP object
k1 <- MAE |>
 EMP_assay_extract('geno_ec')
k2 <- MAE |>
 EMP_assay_extract('geno_ko')
(k1+k2) |> EMP_summary()
```
<img src="tutorial_related/tutorial_figs/EMP_summary2.jpg" alt="EMP_summary2" style="zoom:100%;" />


#### EMP_taxonomy_import

Detailed example could be found in the section **Import data** above. 

#### EMP_function_import

Detailed example could be found in the section **Import data** above. 

#### EMP_normal_import

Detailed example could be found in the section **Import data** above. 

#### EMP_easy_import

Detailed example could be found in the section **Import data** above. 