# Height Estimation from Skeletal Remains 
 
![Height Estimation from Skeletal
Remains](./2_img/banner.jpg) 

## Overview

## Dataset

### Cargando el dataset

El dataset se encuentra en un archivo `.accdb` (Microsoft Access) asi que lo primero que vamos a hacer es exportaro como `.csv`

![Acces DB](./2_img/accesdb.png)

Analizandolo rapidamente en un editor de texto podemos ver que contiene carateres que no van a poder ser decodeados con UTF-8 (`�`). 

![UTF-8 Error](./2_img/utf8Error.png)

Ahora ya podemos cargarlo con `pandas`


```python
import missingno as msno
import pandas as pd
import pandas_profiling as pf
```


```python
data = pd.read_csv('../dataindsamling.csv')
```


```python
msno.matrix(data)
```


![png](./2_img/output_2_1.png)



```python
msno.bar(data)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1afe692c048>




![png](./2_img/output_3_1.png)



```python
pf.ProfileReport(data)
```




<meta charset="UTF-8">

<style>
        .variablerow {
            border: 1px solid #e1e1e8;
            border-top: hidden;
            padding-top: 2em;
            padding-bottom: 2em;
            padding-left: 1em;
            padding-right: 1em;
        }

        .headerrow {
            border: 1px solid #e1e1e8;
            background-color: #f5f5f5;
            padding: 2em;
        }
        .namecol {
            margin-top: -1em;
            overflow-x: auto;
        }

        .dl-horizontal dt {
            text-align: left;
            padding-right: 1em;
            white-space: normal;
        }

        .dl-horizontal dd {
            margin-left: 0;
        }

        .ignore {
            opacity: 0.4;
        }

        .container.pandas-profiling {
            max-width:975px;
        }

        .col-md-12 {
            padding-left: 2em;
        }

        .indent {
            margin-left: 1em;
        }

        .center-img {
            margin-left: auto !important;
            margin-right: auto !important;
            display: block;
        }

        /* Table example_values */
            table.example_values {
                border: 0;
            }

            .example_values th {
                border: 0;
                padding: 0 ;
                color: #555;
                font-weight: 600;
            }

            .example_values tr, .example_values td{
                border: 0;
                padding: 0;
                color: #555;
            }

        /* STATS */
            table.stats {
                border: 0;
            }

            .stats th {
                border: 0;
                padding: 0 2em 0 0;
                color: #555;
                font-weight: 600;
            }

            .stats tr {
                border: 0;
            }

            .stats td{
                color: #555;
                padding: 1px;
                border: 0;
            }


        /* Sample table */
            table.sample {
                border: 0;
                margin-bottom: 2em;
                margin-left:1em;
            }
            .sample tr {
                border:0;
            }
            .sample td, .sample th{
                padding: 0.5em;
                white-space: nowrap;
                border: none;

            }

            .sample thead {
                border-top: 0;
                border-bottom: 2px solid #ddd;
            }

            .sample td {
                width:100%;
            }


        /* There is no good solution available to make the divs equal height and then center ... */
            .histogram {
                margin-top: 3em;
            }
        /* Freq table */

            table.freq {
                margin-bottom: 2em;
                border: 0;
            }
            table.freq th, table.freq tr, table.freq td {
                border: 0;
                padding: 0;
            }

            .freq thead {
                font-weight: 600;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;

            }

            td.fillremaining{
                width:auto;
                max-width: none;
            }

            td.number, th.number {
                text-align:right ;
            }

        /* Freq mini */
            .freq.mini td{
                width: 50%;
                padding: 1px;
                font-size: 12px;

            }
            table.freq.mini {
                 width:100%;
            }
            .freq.mini th {
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                max-width: 5em;
                font-weight: 400;
                text-align:right;
                padding-right: 0.5em;
            }

            .missing {
                color: #a94442;
            }
            .alert, .alert > th, .alert > td {
                color: #a94442;
            }


        /* Bars in tables */
            .freq .bar{
                float: left;
                width: 0;
                height: 100%;
                line-height: 20px;
                color: #fff;
                text-align: center;
                background-color: #337ab7;
                border-radius: 3px;
                margin-right: 4px;
            }
            .other .bar {
                background-color: #999;
            }
            .missing .bar{
                background-color: #a94442;
            }
            .tooltip-inner {
                width: 100%;
                white-space: nowrap;
                text-align:left;
            }

            .extrapadding{
                padding: 2em;
            }

            .pp-anchor{

            }

</style>

<div class="container pandas-profiling">
    <div class="row headerrow highlight">
        <h1>Overview</h1>
    </div>
    <div class="row variablerow">
    <div class="col-md-6 namecol">
        <p class="h4">Dataset info</p>
        <table class="stats" style="margin-left: 1em;">
            <tbody>
            <tr>
                <th>Number of variables</th>
                <td>23 </td>
            </tr>
            <tr>
                <th>Number of observations</th>
                <td>117 </td>
            </tr>
            <tr>
                <th>Total Missing (%)</th>
                <td>18.8% </td>
            </tr>
            <tr>
                <th>Total size in memory</th>
                <td>19.5 KiB </td>
            </tr>
            <tr>
                <th>Average record size in memory</th>
                <td>170.7 B </td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="col-md-6 namecol">
        <p class="h4">Variables types</p>
        <table class="stats" style="margin-left: 1em;">
            <tbody>
            <tr>
                <th>Numeric</th>
                <td>10 </td>
            </tr>
            <tr>
                <th>Categorical</th>
                <td>8 </td>
            </tr>
            <tr>
                <th>Boolean</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Date</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Text (Unique)</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Rejected</th>
                <td>5 </td>
            </tr>
            <tr>
                <th>Unsupported</th>
                <td>0 </td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="col-md-12" style="padding-left: 1em;">  
        <p class="h4">Warnings</p>
        <ul class="list-unstyled"><li><a href="#pp_var_Abnormalities Femur"><code>Abnormalities Femur</code></a> has 106 / 90.6% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Abnormalities Vertebras"><code>Abnormalities Vertebras</code></a> has 104 / 88.9% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Age_Maximum"><code>Age_Maximum</code></a> is highly correlated with <a href="#pp_var_Age_Minumum"><code>Age_Minumum</code></a> (ρ = 0.94228) <span class="label label-primary">Rejected</span></li><li><a href="#pp_var_Canine 2nd largest age"><code>Canine 2nd largest age</code></a> has 38 / 32.5% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Canine 2nd largest age"><code>Canine 2nd largest age</code></a> has 41 / 35.0% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> has 38 / 32.5% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> has 34 / 29.1% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Canine number"><code>Canine number</code></a> has 44 / 37.6% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Canine number"><code>Canine number</code></a> has 28 / 23.9% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Date"><code>Date</code></a> has 3 / 2.6% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Femur left"><code>Femur left</code></a> has 7 / 6.0% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Femur left"><code>Femur left</code></a> has 25 / 21.4% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Femur right"><code>Femur right</code></a> has 11 / 9.4% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Femur right"><code>Femur right</code></a> has 27 / 23.1% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Grave Number"><code>Grave Number</code></a> has a high cardinality: 116 distinct values  <span class="label label-warning">Warning</span></li><li><a href="#pp_var_Height in grave"><code>Height in grave</code></a> has 2 / 1.7% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Hyperplasia"><code>Hyperplasia</code></a> is highly correlated with <a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> (ρ = 0.90091) <span class="label label-primary">Rejected</span></li><li><a href="#pp_var_Incisor 2nd largest age"><code>Incisor 2nd largest age</code></a> is highly correlated with <a href="#pp_var_Incisor largest age"><code>Incisor largest age</code></a> (ρ = 0.90384) <span class="label label-primary">Rejected</span></li><li><a href="#pp_var_Incisor largest age"><code>Incisor largest age</code></a> has 42 / 35.9% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Incisor largest age"><code>Incisor largest age</code></a> has 42 / 35.9% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Incisor number"><code>Incisor number</code></a> has 44 / 37.6% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Incisor number"><code>Incisor number</code></a> has 40 / 34.2% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Notes"><code>Notes</code></a> has 52 / 44.4% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Notes"><code>Notes</code></a> has a high cardinality: 55 distinct values  <span class="label label-warning">Warning</span></li><li><a href="#pp_var_Sex"><code>Sex</code></a> has 2 / 1.7% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Signature"><code>Signature</code></a> has 3 / 2.6% missing values <span class="label label-default">Missing</span></li><li><a href="#pp_var_Site_Number"><code>Site_Number</code></a> has constant value ASR1015 <span class="label label-primary">Rejected</span></li><li><a href="#pp_var_Teeth Scorable"><code>Teeth Scorable</code></a> is highly correlated with <a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> (ρ = 0.90509) <span class="label label-primary">Rejected</span></li> </ul>
    </div>
</div>
    <div class="row headerrow highlight">
        <h1>Variables</h1>
    </div>
    <div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Abnormalities Femur">Abnormalities Femur<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>12</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>10.3%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>90.6%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>106</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable-1748492645196362724">
    <table class="mini freq">
        <tr class="">
    <th>Sk?r postmortal nedbrydning</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="">
    <th>L?bedannelse p? femur, men ingen betydning for h?jden.</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="">
    <th>h?jre femur ikke bevaret</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="other">
    <th>Other values (8)</th>
    <td>
        <div class="bar" style="width:8%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 6.8%">
            &nbsp;
        </div>
        8
    </td>
</tr><tr class="missing">
    <th>(Missing)</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 90.6%">
            106
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable-1748492645196362724, #minifreqtable-1748492645196362724"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable-1748492645196362724">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">Sk?r postmortal nedbrydning</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">L?bedannelse p? femur, men ingen betydning for h?jden.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">h?jre femur ikke bevaret</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Meget store og robuste. En rigtig mand.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Flade exostoser p? caput. Ingen betdning for h?jden</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Kn?kkede postmortalt s? kan ikke m?les</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">M?rkelig venstre femur</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Br?kket Postmortalt</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Begge l?rben for nedbrudte til at m?le</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">H?jre femur lidt deform... Syfilis?</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">106</td>
        <td class="number">90.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Abnormalities Vertebras">Abnormalities Vertebras<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>13</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>11.1%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>88.9%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>104</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable-2780961831983723855">
    <table class="mini freq">
        <tr class="">
    <th>L?bedannelse</th>
    <td>
        <div class="bar" style="width:2%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 1.7%">
            &nbsp;
        </div>
        2
    </td>
</tr><tr class="">
    <th>L?bedanelse og ser lidt sammentrykkede ud.</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="">
    <th>l?bedannelse</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="other">
    <th>Other values (9)</th>
    <td>
        <div class="bar" style="width:9%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 7.7%">
            &nbsp;
        </div>
        9
    </td>
</tr><tr class="missing">
    <th>(Missing)</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 88.9%">
            104
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable-2780961831983723855, #minifreqtable-2780961831983723855"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable-2780961831983723855">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">L?bedannelse</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">L?bedanelse og ser lidt sammentrykkede ud.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">l?bedannelse</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">udvoksninger, men intet der p?virker h?jden</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">To ryghvirvler fusioneret, men har ingen betydning for h?jden.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">To hvirvler fusioneret - har ingen betydning for h?jden</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">et par hvirvler sammenklemte. Nok pga af alder. Har indvirkning p? h?jden.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">en enkelt hvirvel lidt sammentrykket. Intet voldsomt.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Nogle hvirvler lettere trykkede</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Ryg lidt sammenklemt</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (2)</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">104</td>
        <td class="number">88.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Age_Maximum"><s>Age_Maximum</s><br/>
            <small>Highly correlated</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is highly correlated with <a href="#pp_var_Age_Minumum"><code>Age_Minumum</code></a> and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Correlation</th>
            <td>0.94228</td>
        </tr>
    </table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Age_Minumum">Age_Minumum<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>16</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>13.7%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>34.795</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>18</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>60</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram-5190896101747871022">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAc9JREFUeJzt2kEO0kAYhuG/xG1hT%2BgtPIR3cu3S%2B3gIbwHhAG1cuJC6UNyoX8SEKZTnWULItMm8mdKZbp7nuRo7nU41DEPrYXlyx%2BOxDodD0zHfNB3tp77vq%2BrHDW%2B32yUugScyjmMNw/Br3rS0SCBd11VV1Xa7XXUgb99/uvk3nz%2B8u8OVrMN13rS0aT4iPBGBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCwSKHFRMH/HgkVhAIBAKBQCAQCAQCgUAgEAgEgofbB2nllfdbXvneb2UFgUAgEAgEAoFAIBAIBAKBQCBYxT7I/7zXh39hBYFgFSvImtjlfiwCWQGPmPfjEQsCgUAgEAgEAoFAIBAIBAKBQCAQCAQCO%2BnczRqOzVhBIBAIBB6xbvDKhwJf9d4XCWSe56qqGsfxt%2B%2B%2Bff3S%2BnJ4IH%2BaE9fPrvOmpUUCmaapqqqGYVhieB7Y7uPfv5umqXa7XbuLqapuXiDLy%2BVS5/O5%2Br6vrutaD8%2BTmee5pmmq/X5fm03bv82LBALPwlssCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBILvxbFVG1T9m6MAAAAASUVORK5CYII%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives-5190896101747871022,#minihistogram-5190896101747871022"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives-5190896101747871022">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles-5190896101747871022"
                                                  aria-controls="quantiles-5190896101747871022" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram-5190896101747871022" aria-controls="histogram-5190896101747871022"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common-5190896101747871022" aria-controls="common-5190896101747871022"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme-5190896101747871022" aria-controls="extreme-5190896101747871022"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles-5190896101747871022">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>18</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>20</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>25</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>35</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>43</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>50</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>60</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>42</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>18</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>10.675</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.3068</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-0.6854</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>34.795</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>8.4607</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.32841</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>4071</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>113.96</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram-5190896101747871022">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3XtwVPX9//HXSpLlYrIQINlEIpMqWiBAkVC5CeFiIApW8QIiCIpWKKAIVA2MX8N0IAzelZoKlpuKoFNQGBAIKgEmRSGC3ByMghIgMYCQBArL7fP7w7p1GxDs75Oc3ezzMXNm3HPObt7MZ5SnZ89uXMYYIwAAAFhzhdMDAAAA1DQEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGUEFgAAgGURTg8QDs6fP6%2BDBw8qOjpaLpfL6XEAAAgLxhhVVFQoMTFRV1xRzdeUTJh77bXXTKtWrUx0dLSJjo42HTp0MCtWrPAfP3XqlBk9erRp2LChqVu3runXr58pKir6VT%2BjqKjISGJjY2NjY2NzYPu1f2/b4DLGGIWxZcuWqVatWrr22mslSfPmzdOzzz6rLVu2qGXLlho5cqSWLVumuXPnqmHDhho/frx%2B%2BOEHFRQUqFatWpf1M8rKylS/fn0VFRUpJiamKv84AADg38rLy5WUlKRjx47J4/FU688O%2B8C6kNjYWD377LO666671LhxY7355psaMGCAJOngwYNKSkrSihUr1Lt378t6vfLycnk8HpWVlRFYAABUEyf//uUm9585d%2B6cFi5cqBMnTqhjx44qKCjQmTNnlJ6e7j8nMTFRKSkpys/Pd3BSAAAQzLjJXdL27dvVsWNHnTp1SldeeaWWLFmiFi1aaOvWrYqKilKDBg0Czo%2BPj1dJSclFX8/n88nn8/kfl5eXV9nsAAAg%2BHAFS9L111%2BvrVu3auPGjRo5cqSGDh2qXbt2XfR8Y8wvfhowOztbHo/HvyUlJVXF2AAAIEgRWJKioqJ07bXXKjU1VdnZ2WrTpo1efvlleb1enT59WkePHg04v7S0VPHx8Rd9vczMTJWVlfm3oqKiqv4jAACAIEJgXYAxRj6fT%2B3atVNkZKRyc3P9x4qLi7Vjxw516tTpos93u92KiYkJ2AAAQPgI%2B3uwJk6cqIyMDCUlJamiokILFy7U2rVrtXLlSnk8Hg0fPlzjx49Xw4YNFRsbqwkTJqhVq1bq1auX06MDAIAgFfaB9f3332vIkCEqLi6Wx%2BNR69attXLlSt18882SpBdffFERERG65557dPLkSfXs2VNz58697O/AAgAA4YfvwaoGfA8WAADVj%2B/BAgAAqEEILAAAAMsILAAAAMsILAAAAMvC/lOEAKpf6qSVTo/wq2ye0sfpEQCEGK5gAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWBb2gZWdna327dsrOjpacXFxuv3227V79%2B6Ac9LS0uRyuQK2gQMHOjQxAAAIdmEfWHl5eRo1apQ2btyo3NxcnT17Vunp6Tpx4kTAeQ8//LCKi4v92%2Buvv%2B7QxAAAINhFOD2A01auXBnweM6cOYqLi1NBQYG6du3q31%2B3bl15vd7qHg8AAISgsL%2BC9d/KysokSbGxsQH73377bTVq1EgtW7bUhAkTVFFRcdHX8Pl8Ki8vD9gAAED4CPsrWD9njNG4cePUpUsXpaSk%2BPffd999Sk5Oltfr1Y4dO5SZmakvvvhCubm5F3yd7OxsTZ48ubrGBgAAQcZljDFODxEsRo0apeXLl2vDhg1q0qTJRc8rKChQamqqCgoKdMMNN1Q67vP55PP5/I/Ly8uVlJSksrIyxcTEVMnsQChJnbTy0icFkc1T%2Bjg9AoD/QXl5uTwejyN//3IF69/GjBmjpUuXat26db8YV5J0ww03KDIyUoWFhRcMLLfbLbfbXVWjAgCAIBf2gWWM0ZgxY7RkyRKtXbtWycnJl3zOzp07debMGSUkJFTDhAAAINSEfWCNGjVKCxYs0AcffKDo6GiVlJRIkjwej%2BrUqaNvvvlGb7/9tm655RY1atRIu3bt0vjx49W2bVt17tzZ4ekBAEAwCvtPEebk5KisrExpaWlKSEjwb4sWLZIkRUVF6aOPPlLv3r11/fXX69FHH1V6errWrFmjWrVqOTw9AAAIRmF/BetS9/gnJSUpLy%2BvmqYBAAA1QdhfwQIAALCNwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALAswukB8P8nddJKp0e4bJun9HF6BAAAqgVXsAAAACwjsAAAACwjsAAAACwjsAAAACwjsAAAACwL%2B8DKzs5W%2B/btFR0drbi4ON1%2B%2B%2B3avXt3wDk%2Bn09jxoxRo0aNVK9ePd12223av3%2B/QxMDAIBgF/aBlZeXp1GjRmnjxo3Kzc3V2bNnlZ6erhMnTvjPGTt2rJYsWaKFCxdqw4YNOn78uPr27atz5845ODkAAAhWYf89WCtXBn6P1Jw5cxQXF6eCggJ17dpVZWVl%2Bvvf/64333xTvXr1kiS99dZbSkpK0po1a9S7d28nxgYAAEEs7K9g/beysjJJUmxsrCSpoKBAZ86cUXp6uv%2BcxMREpaSkKD8/35EZAQBAcAv7K1g/Z4zRuHHj1KVLF6WkpEiSSkpKFBUVpQYNGgScGx8fr5KSkgu%2Bjs/nk8/n8z8uLy%2BvuqEBAEDQ4QrWz4wePVrbtm3TO%2B%2B8c8lzjTFyuVwXPJadnS2Px%2BPfkpKSbI8KAACCGIH1b2PGjNHSpUv1ySefqEmTJv79Xq9Xp0%2Bf1tGjRwPOLy0tVXx8/AVfKzMzU2VlZf6tqKioSmcHAADBJewDyxij0aNHa/Hixfr444%2BVnJwccLxdu3aKjIxUbm6uf19xcbF27NihTp06XfA13W63YmJiAjYAABA%2Bwv4erFGjRmnBggX64IMPFB0d7b%2BvyuPxqE6dOvJ4PBo%2BfLjGjx%2Bvhg0bKjY2VhMmTFCrVq38nyoEAAD4ubAPrJycHElSWlpawP45c%2BZo2LBhkqQXX3xRERERuueee3Ty5En17NlTc%2BfOVa1atap5WgAAEArCPrCMMZc8p3bt2nr11Vf16quvVsNEAAAg1IX9PVgAAAC2EVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWhWxgvfXWWzp16pTTYwAAAFQSsoE1btw4eb1ePfLII/rss8%2BcHgcAAMAvZAPr4MGDmj17toqLi9WlSxe1bNlSzz//vA4dOuT0aAAAIMyFbGBFRESof//%2BWrp0qfbt26ehQ4dq9uzZatKkifr376/ly5fLGOP0mAAAIAyFbGD9nNfrVc%2BePZWWliaXy6XNmzdr0KBBatasmdavX%2B/0eAAAIMyEdGAdPnxYL730ktq0aaPOnTurtLRU77//vr777jsdOHBAffv21f333%2B/0mAAAIMxEOD3A/%2BqOO%2B7QihUrlJycrIceekhDhw5V48aN/cevvPJKPfHEE3rllVccnBIAAISjkA2smJgYrVmzRjfddNNFz0lISFBhYWE1TgUAABDCgTVv3rxLnuNyuXTNNddUwzQAAAD/EbL3YD3%2B%2BOOaMWNGpf1//etfNX78eAcmAgAA%2BFHIBtZ7772nDh06VNrfsWNHLVq0yIGJAAAAfhSygXX48GE1aNCg0v6YmBgdPnzYgYkAAAB%2BFLKBdc0112jVqlWV9q9atUrJyckOTAQAAPCjkL3JfezYsRo7dqyOHDmiHj16SJI%2B%2BugjTZ8%2BXc8995zD0wEAgHAWsoH18MMP69SpU5o6daqeeeYZSVKTJk30yiuv6MEHH3R4OgAAEM5CNrAkacyYMRozZoyKi4tVp04d1a9f3%2BmRAAAAQjuwfpKQkOD0CAAAAH4he5P7oUOH9MADD%2Bjqq69W7dq1FRUVFbABAAA4JWSvYA0bNkzffPON/vznPyshIUEul8vpkQAAACSFcGCtW7dO69atU9u2bZ0eBQAAIEDIvkXYpEkTrloBAICgFLKB9eKLLyozM1P79%2B93ehQAAIAAIfsW4ZAhQ1RRUaGmTZsqJiZGkZGRAcdLS0sdmgwAAIS7kA2sadOmOT0CAADABYVsYA0fPtzpEQAAAC4oZO/BkqRvv/1WWVlZGjJkiP8twdWrV%2BvLL790eDIAABDOQvYK1vr169WnTx/9/ve/V35%2BviZPnqy4uDh9/vnnmjVrlt577z2nR0SIS5200ukRfpXNU/o4PQLwq/HvGWqqkL2C9eSTTyorK0uffPJJwDe39%2BjRQxs3bnRwMgAAEO5CNrC2bdumu%2B66q9L%2BuLg4HTp0yIGJAAAAfhSygVW/fn2VlJRU2r9161ZdddVVDkwEAADwo5ANrIEDB%2Bqpp57SoUOH/N/o/umnn2rChAkaPHiww9MBAIBwFrKBNXXqVHm9XiUkJOj48eNq0aKFOnXqpPbt2%2Bvpp592ejwAABDGQjawoqKitGjRIu3atUsLFizQ7NmztXPnTr3zzjuKiLj8D0euW7dO/fr1U2Jiolwul95///2A48OGDZPL5QrYOnToYPuPAwAAapCQ/ZqGn1x33XW67rrr/ufnnzhxQm3atNEDDzygO%2B%2B884Ln9OnTR3PmzPE//vmnFgEAAP5byAbWH//4x188PnPmzMt6nYyMDGVkZPziOW63W16v97JnAwAA4S1kA6u4uDjg8ZkzZ7Rz505VVFSoa9euVn/W2rVrFRcXp/r166tbt26aMmWK4uLiLnq%2Bz%2BeTz%2BfzPy4vL7c6DwAACG4hG1jLli2rtO/s2bMaOXKkmjdvbu3nZGRk6O6771bTpk21d%2B9ePf300%2BrRo4cKCgrkdrsv%2BJzs7GxNnjzZ2gwAACC0hOxN7hcSERGhCRMm6Nlnn7X2mgMGDNCtt96qlJQU9evXTx9%2B%2BKG%2B%2BuorLV%2B%2B/KLPyczMVFlZmX8rKiqyNg8AAAh%2BIXsF62L27NmjM2fOVNnrJyQkqGnTpiosLLzoOW63%2B6JXtwAAQM0XsoH1xBNPBDw2xqi4uFhLly7VfffdV2U/98iRIyoqKlJCQkKV/QwAABDaQjaw/vnPfwY8vuKKK9S4cWNNmzZNDz/88GW/zvHjx/X111/7H%2B/du1dbt25VbGysYmNjlZWVpTvvvFMJCQn69ttvNXHiRDVq1Eh33HGHtT8LAACoWUI2sNavX2/ldTZv3qzu3bv7H48bN06SNHToUOXk5Gj79u2aP3%2B%2Bjh07poSEBHXv3l2LFi1SdHS0lZ8PAABqnpANLFvS0tJkjLno8VWrVlXjNAAAoCYI2cBq3769/5c8X8pnn31WxdMAAAD8R8gGVvfu3fX666/ruuuuU8eOHSVJGzdu1O7du/XII4/wKT4AAOCYkA2sY8eOadSoUZo6dWrA/kmTJun777/XG2%2B84dBkAAAg3IXsF42%2B%2B%2B67euCBByrtHzZsmN577z0HJgIAAPhRyAaW2%2B1Wfn5%2Bpf35%2Bfm8PQgAABwVsm8RPvrooxoxYoS2bNmiDh06SPrxHqxZs2Zp4sSJDk8HAADCWcgG1qRJk5ScnKyXX35Zs2fPliQ1b95cs2bN0qBBgxyeDgAAhLOQDSxJGjRoEDEFAACCTsjegyVJ5eXlmjt3rv7v//5PR48elSR98cUXKi4udngyAAAQzkL2CtaOHTvUq1cv1a1bV0VFRRo2bJgaNGigd999V/v379e8efOcHhEAAISpkL2C9fjjj2vQoEH65ptvVLt2bf/%2BW2%2B9VevWrXNwMgAAEO5C9grWpk2blJOTU%2BnX5Vx11VW8RQgAABwVslewoqKidPz48Ur7CwsL1ahRIwcmAgAA%2BFHIBtZtt92mv/zlLzp79qwkyeVy6cCBA3rqqafUv39/h6cDAADhLGQD6/nnn9fBgwfl9Xp18uRJ9ejRQ7/5zW9Uu3btSr%2BfEAAAoDqF7D1YHo9H%2Bfn5ys3N1eeff67z58/rhhtuUO/evSvdlwUAAFCdQjKwzpw5o1tuuUWvvfaa0tPTlZ6e7vRIAAAAfiH5FmFkZKS2bNnClSoAABCUQjKwJGnw4MGaM2eO02MAAABUEpJvEf5kxowZWrNmjVJTU1WvXr2AY9OnT3doKgAAEO5CNrAKCgrUunVrSdK2bdsCjvHWIQAAcFLIBdaePXuUnJys9evXOz0KAADABYXcPVjNmjXToUOH/I8HDBig77//3sGJAAAAAoVcYBljAh6vWLFCJ06ccGgaAACAykIusAAAAIJdyAWWy%2BWqdBM7N7UDAIBgEnI3uRtjNGzYMLndbknSqVOnNGLEiEpf07B48WInxgMAAAi9wBo6dGjA48GDBzs0CQAAwIWFXGDx7e0AACDYhdw9WAAAAMGOwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALAs5L6mAaErddJKp0cAAKBacAULAADAMgILAADAMgILAADAMgILAADAMgILAADAsrAPrHXr1qlfv35KTEyUy%2BXS%2B%2B%2B/H3DcGKOsrCwlJiaqTp06SktL086dOx2aFgAAhIKwD6wTJ06oTZs2mjFjxgWPT58%2BXS%2B88IJmzJihTZs2yev16uabb1ZFRUU1TwoAAEJF2H8PVkZGhjIyMi54zBijl156SZMmTVL//v0lSfPmzVN8fLwWLFigRx55pDpHBQAAISLsr2D9kr1796qkpETp6en%2BfW63W926dVN%2Bfr6DkwEAgGAW9lewfklJSYkkKT4%2BPmB/fHy8vvvuu4s%2Bz%2Bfzyefz%2BR%2BXl5dXzYAAACAoEViXweVyBTw2xlTa93PZ2dmaPHlyVY8FoJqE2q952jylj9MjAGGPtwh/gdfrlfSfK1k/KS0trXRV6%2BcyMzNVVlbm34qKiqp0TgAAEFwIrF%2BQnJwsr9er3Nxc/77Tp08rLy9PnTp1uujz3G63YmJiAjYAABA%2Bwv4twuPHj%2Bvrr7/2P967d6%2B2bt2q2NhYXX311Ro7dqymTp2qZs2aqVmzZpo6darq1q2rQYMGOTg1AAAIZmEfWJs3b1b37t39j8eNGydJGjp0qObOnasnnnhCJ0%2Be1J/%2B9CcdPXpUN954o1avXq3o6GinRgYAAEEu7AMrLS1NxpiLHne5XMrKylJWVlb1DQUAAEIa92ABAABYRmABAABYRmABAABYRmABAABYFvY3uQNATRNq3zwP1ERcwQIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwLqErKwsuVyugM3r9To9FgAACGIRTg8QClq2bKk1a9b4H9eqVcvBaQAAQLAjsC5DREQEV60AAMBl4y3Cy1BYWKjExEQlJydr4MCB2rNnzy%2Be7/P5VF5eHrABAIDwQWBdwo033qj58%2Bdr1apVmjVrlkpKStSpUycdOXLkos/Jzs6Wx%2BPxb0lJSdU4MQAAcBqBdQkZGRm688471apVK/Xq1UvLly%2BXJM2bN%2B%2Biz8nMzFRZWZl/Kyoqqq5xAQBAEOAerF%2BpXr16atWqlQoLCy96jtvtltvtrsapAABAMOEK1q/k8/n05ZdfKiEhwelRAABAkCKwLmHChAnKy8vT3r179emnn%2Bquu%2B5SeXm5hg4d6vRoAAAgSPEW4SXs379f9957rw4fPqzGjRurQ4cO2rhxo5o2ber0aAAAIEgRWJewcOFCp0cAAAAhhrcIAQAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALItwegAAAFA1UietdHqEy7Z5Sh%2BnR7CKK1gAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWEVgAAACWRTg9AAA7UietdHoEoMbj3zNcLq5gAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgAQAAWEZgXabXXntNycnJql27ttq1a6f169c7PRIAAAhSBNZlWLRokcaOHatJkyZpy5Ytuummm5SRkaF9%2B/Y5PRoAAAhCBNZleOGFFzR8%2BHA99NBDat68uV566SUlJSUpJyfH6dEAAEAQ4pvcL%2BH06dMqKCjQU089FbA/PT1d%2Bfn5F3yOz%2BeTz%2BfzPy4rK5MklZeXW5/vnO%2BE9dcEAKC6VcXfkT%2B9pjHG%2BmtfCoF1CYcPH9a5c%2BcUHx8fsD8%2BPl4lJSUXfE52drYmT55caX9SUlKVzAgAQKjzPF91r11RUSGPx1N1P%2BACCKzL5HK5Ah4bYyrt%2B0lmZqbGjRvnf3z%2B/Hn98MMPatiw4UWfEyzKy8uVlJSkoqIixcTEOD0Ofoa1CW6sT3BjfYJbVa2PMUYVFRVKTEy09pqXi8C6hEaNGqlWrVqVrlaVlpZWuqr1E7fbLbfbHbCvfv36VTZjVYiJieE/QkGKtQlurE9wY32CW1WsT3VfufoJN7lfQlRUlNq1a6fc3NyA/bm5uerUqZNDUwEAgGDGFazLMG7cOA0ZMkSpqanq2LGjZs6cqX379mnEiBFOjwYAAIJQraysrCynhwh2KSkpatiwoaZOnarnnntOJ0%2Be1Jtvvqk2bdo4PVqVqFWrltLS0hQRQX8HG9YmuLE%2BwY31CW41bX1cxonPLgIAANRg3IMFAABgGYEFAABgGYEFAABgGYEFAABgGYEVhrKzs9W%2BfXtFR0crLi5Ot99%2Bu3bv3h1wjs/n05gxY9SoUSPVq1dPt912m/bv3%2B/QxOElJydHrVu39n/hXseOHfXhhx/6j7M2wSM7O1sul0tjx47172N9nJWVlSWXyxWweb1e/3FjjLKyspSYmKg6deooLS1NO3fudHDi8HLgwAENHjxYDRs2VN26dfW73/1OBQUF/uM1aX0IrDCUl5enUaNGaePGjcrNzdXZs2eVnp6uEyf%2B84ujx44dqyVLlmjhwoXasGGDjh8/rr59%2B%2BrcuXMOTh4emjRpomnTpmnz5s3avHmzevTooT/84Q/%2B/8iwNsFh06ZNmjlzplq3bh2wn/VxXsuWLVVcXOzftm/f7j82ffp0vfDCC5oxY4Y2bdokr9erm2%2B%2BWRUVFQ5OHB6OHj2qzp07KzIyUh9%2B%2BKF27dql559/PuA3ndSo9TEIe6WlpUaSycvLM8YYc%2BzYMRMZGWkWLlzoP%2BfAgQPmiiuuMCtXrnRqzLDWoEED88Ybb7A2QaKiosI0a9bM5Obmmm7dupnHHnvMGMO/O8HgmWeeMW3atLngsfPnzxuv12umTZvm33fq1Cnj8XjM3/72t%2BoaMWw9%2BeSTpkuXLhc9XtPWhytYUFlZmSQpNjZWklRQUKAzZ84oPT3df05iYqJSUlKUn5/vyIzh6ty5c1q4cKFOnDihjh07sjZBYtSoUbr11lvVq1evgP2sT3AoLCxUYmKikpOTNXDgQO3Zs0eStHfvXpWUlASsj9vtVrdu3VifarB06VKlpqbq7rvvVlxcnNq2batZs2b5j9e09SGwwpwxRuPGjVOXLl2UkpIiSSopKVFUVJQaNGgQcG58fHylX3qNqrF9%2B3ZdeeWVcrvdGjFihJYsWaIWLVqwNkFg4cKFKigoUHZ2dqVjrI/zbrzxRs2fP1%2BrVq3SrFmzVFJSok6dOunIkSP%2BNYiPjw94DutTPfbs2aOcnBw1a9ZMq1at0ogRI/Too49q/vz5klTj1qdmfB89/mejR4/Wtm3btGHDhkuea4yRy%2BWqhqlw/fXXa%2BvWrTp27Jj%2B8Y9/aOjQocrLy7vo%2BaxN9SgqKtJjjz2m1atXq3bt2pf9PNan%2BmRkZPj/uVWrVurYsaOuueYazZs3Tx06dJCkSmvB%2BlSP8%2BfPKzU1VVOnTpUktW3bVjt37lROTo7uv/9%2B/3k1ZX24ghXGxowZo6VLl%2BqTTz5RkyZN/Pu9Xq9Onz6to0ePBpxfWlpa6f8sUDWioqJ07bXXKjU1VdnZ2WrTpo1efvll1sZhBQUFKi0tVbt27RQREaGIiAjl5eXplVdeUUREhOLj41mfIFOvXj21atVKhYWF/k8T/vfVENaneiQkJKhFixYB%2B5o3b659%2B/ZJUo1bHwIrDBljNHr0aC1evFisesg8AAACSElEQVQff/yxkpOTA463a9dOkZGRys3N9e8rLi7Wjh071KlTp%2BoeF/pxzXw%2BH2vjsJ49e2r79u3aunWrf0tNTdV9993n/2fWJ7j4fD59%2BeWXSkhIUHJysrxeb8D6nD59Wnl5eaxPNejcuXOlrwT66quv1LRpU0mqeevj3P31cMrIkSONx%2BMxa9euNcXFxf7tX//6l/%2BcESNGmCZNmpg1a9aYzz//3PTo0cO0adPGnD171sHJw0NmZqZZt26d2bt3r9m2bZuZOHGiueKKK8zq1auNMaxNsPn5pwiNYX2cNn78eLN27VqzZ88es3HjRtO3b18THR1tvv32W2OMMdOmTTMej8csXrzYbN%2B%2B3dx7770mISHBlJeXOzx5zffZZ5%2BZiIgIM2XKFFNYWGjefvttU7duXfPWW2/5z6lJ60NghSFJF9zmzJnjP%2BfkyZNm9OjRJjY21tSpU8f07dvX7Nu3z7mhw8iDDz5omjZtaqKiokzjxo1Nz549/XFlDGsTbP47sFgfZw0YMMAkJCSYyMhIk5iYaPr372927tzpP37%2B/HnzzDPPGK/Xa9xut%2BnatavZvn27gxOHl2XLlpmUlBTjdrvNb3/7WzNz5syA4zVpfVzGGOPkFTQAAICahnuwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALCOwAAAALPt/Wekotz35tZwAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common-5190896101747871022">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">35</td>
        <td class="number">26</td>
        <td class="number">22.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">50</td>
        <td class="number">16</td>
        <td class="number">13.7%</td>
        <td>
            <div class="bar" style="width:61%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">30</td>
        <td class="number">15</td>
        <td class="number">12.8%</td>
        <td>
            <div class="bar" style="width:58%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">20</td>
        <td class="number">15</td>
        <td class="number">12.8%</td>
        <td>
            <div class="bar" style="width:58%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">25</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:42%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">45</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:35%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:35%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">21</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">60</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">36</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (6)</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:23%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme-5190896101747871022">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">18</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">20</td>
        <td class="number">15</td>
        <td class="number">12.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">21</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:27%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">22</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">25</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:73%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">43</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">45</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:56%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">50</td>
        <td class="number">16</td>
        <td class="number">13.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">55</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">60</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Canine 2nd largest age">Canine 2nd largest age<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>12</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>10.3%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>35.0%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>41</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>1.8908</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>5</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>32.5%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram3052661021054984646">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAc5JREFUeJzt3E2q02AYhuE3xWm6gJLswrkjwT0JDgT3JDhy7i4auoAUBw5sHGidnMMjPciX/lzXsCV5G/rdJCGQblmWpRqbpqnGcWw9lhu33%2B9rGIamM181nfZH3/dV9fuAt9vtGj%2BBGzLPc43j%2BHfdtLRKIF3XVVXVdrt9Esjr958v3t%2B3T%2B/%2By%2B/iup3XTUub5hPhhggEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBALBKm935zHcw5v6nUEgEAgEAoFAIBAIBAKBQCAQCDwHeUD38HyiFYHcgZcs%2BGuccY1cYkEgEAhcYl2ZR72UuVbOIBAIBAKBQLDKPciyLFVVNc/zk%2B9%2B/vh%2B8f6e28%2B/vPn45eJtvn54e/E2l3rJ8d%2BT5/7L82fnddNSt6wwdZqmGsex9Vhu3H6/r2EYms5cJZDT6VSHw6H6vq%2Bu61qP58Ysy1LH47F2u11tNm3vClYJBG6Fm3QIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEgl/7GVaWSpxAtQAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives3052661021054984646,#minihistogram3052661021054984646"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives3052661021054984646">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles3052661021054984646"
                                                  aria-controls="quantiles3052661021054984646" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram3052661021054984646" aria-controls="histogram3052661021054984646"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common3052661021054984646" aria-controls="common3052661021054984646"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme3052661021054984646" aria-controls="extreme3052661021054984646"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles3052661021054984646">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>0.5</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>4</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>4</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>2.0108</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>1.0635</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.724</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>1.8908</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.9142</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.26792</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>143.7</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>4.0435</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram3052661021054984646">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3X9QVWXix/HPXZCrGdxCVCBuDlvamr82pDFMXfwRRWWZtWqWi6252RKtoduGzha2Jo79tFwZrU3N1cGawtzRKGxX0HEpIckfNS6VJSaKtskFRq%2BK9/uH3%2B52Fw3dHjj3cN%2BvmTPTec65hw%2Bnks885/Hg8Pl8PgEAAMCYn1gdAAAAoL2hYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYeFWBwgFp0%2Bf1oEDBxQZGSmHw2F1HAAAQoLP51N9fb3i4%2BP1k5%2B07ZwSBasNHDhwQG632%2BoYAACEpOrqaiUkJLTp16RgtYHIyEhJZ/4FR0VFWZwGAIDQ4PF45Ha7/T%2BH2xIFqw1891gwKiqKggUAQBuzYnkOi9wBAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBi/7NnmkmcXWR3hvJU/dZPVEQAAaBPMYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYFjIF6z8/Hz1799fUVFRioqKUkpKit555x3/8dTUVDkcjoBtwoQJFiYGAADBLtzqAFZLSEjQ/PnzdeWVV0qSVqxYodtvv13bt29Xnz59JElTp07Vk08%2B6f9Mp06dLMkKAADsIeQL1ujRowP2n3rqKeXn56usrMxfsC666CLFxsZaEQ8AANhQyD8i/L6mpiYVFBSosbFRKSkp/vFVq1YpJiZGffr00cyZM1VfX/%2BD1/F6vfJ4PAEbAAAIHSE/gyVJO3fuVEpKio4fP66LL75YhYWFuvrqqyVJ99xzjxITExUbG6tdu3YpJydHH3/8sYqLi895vby8PM2ZM6et4gMAgCDj8Pl8PqtDWO3EiRPat2%2Bfjh49qjfffFOvvPKKSkpK/CXr%2ByoqKpScnKyKigolJSWd9Xper1der9e/7/F45Ha7VVdXp6ioKKPZk2cXGb1eayp/6iarIwAAQojH45HL5WqVn78tYQZLUkREhH%2BRe3JysrZt26aFCxdqyZIlzc5NSkpShw4dVFVVdc6C5XQ65XQ6WzUzAAAIXqzBOgufzxcwA/V9u3fv1smTJxUXF9fGqQAAgF2E/AzWrFmzlJ6eLrfbrfr6ehUUFGjTpk0qKirS559/rlWrVunmm29WTEyMPvnkE82YMUPXXHONrr/%2BequjAwCAIBXyBevQoUOaNGmSampq5HK51L9/fxUVFemGG25QdXW13n//fS1cuFANDQ1yu9265ZZb9MQTTygsLMzq6AAAIEiFfMH6y1/%2Bcs5jbrdbJSUlbZgGAAC0B6zBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMNCvmDl5%2Berf//%2BioqKUlRUlFJSUvTOO%2B/4j3u9XmVlZSkmJkadO3fWbbfdpv3791uYGAAABLuQL1gJCQmaP3%2B%2BysvLVV5erhEjRuj222/X7t27JUnTp09XYWGhCgoKtGXLFjU0NOjWW29VU1OTxckBAECwcvh8Pp/VIYJNdHS0nn76ad11113q2rWrVq5cqfHjx0uSDhw4ILfbrQ0bNujGG288r%2Bt5PB65XC7V1dUpKirKaNbk2UVGr9eayp%2B6yeoIAIAQ0po/f1sS8jNY39fU1KSCggI1NjYqJSVFFRUVOnnypNLS0vznxMfHq2/fvtq6des5r%2BP1euXxeAI2AAAQOihYknbu3KmLL75YTqdT06ZNU2Fhoa6%2B%2BmodPHhQERERuvTSSwPO7969uw4ePHjO6%2BXl5cnlcvk3t9vd2t8CAAAIIhQsSVdddZUqKytVVlamBx98UBkZGfrkk0/Oeb7P55PD4Tjn8ZycHNXV1fm36urq1ogNAACCVLjVAYJBRESErrzySklScnKytm3bpoULF2r8%2BPE6ceKEvv3224BZrNraWg0ePPic13M6nXI6na2eGwAABCdmsM7C5/PJ6/Vq4MCB6tChg4qLi/3HampqtGvXrh8sWAAAILSF/AzWrFmzlJ6eLrfbrfr6ehUUFGjTpk0qKiqSy%2BXSlClTNGPGDHXp0kXR0dGaOXOm%2BvXrp1GjRlkdHQAABKmQL1iHDh3SpEmTVFNTI5fLpf79%2B6uoqEg33HCDJOn5559XeHi4xo0bp2PHjmnkyJFavny5wsLCLE4OAACCFe/BagO8B%2BsM3oMFAGhLvAcLAACgHaFgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwLCQL1h5eXm69tprFRkZqW7dumnMmDHas2dPwDmpqalyOBwB24QJEyxKDAAAgl3IF6ySkhJlZmaqrKxMxcXFOnXqlNLS0tTY2Bhw3tSpU1VTU%2BPflixZYlFiAAAQ7MKtDmC1oqKigP1ly5apW7duqqio0LBhw/zjF110kWJjY9s6HgAAsKGQn8H6b3V1dZKk6OjogPFVq1YpJiZGffr00cyZM1VfX3/Oa3i9Xnk8noANAACEjpCfwfo%2Bn8%2Bn7OxsDRkyRH379vWP33PPPUpMTFRsbKx27dqlnJwcffzxxyouLj7rdfLy8jRnzpy2ig0AAIKMw%2Bfz%2BawOESwyMzO1fv16bdmyRQkJCec8r6KiQsnJyaqoqFBSUlKz416vV16v17/v8XjkdrtVV1enqKgoo5mTZxe1fFKQKH/qJqsjAABCiMfjkcvlapWfvy1hBuv/ZWVlad26dSotLf3BciVJSUlJ6tChg6qqqs5asJxOp5xOZ2tFBQAAQS7kC5bP51NWVpYKCwu1adMmJSYmtviZ3bt36%2BTJk4qLi2uDhAAAwG5CvmBlZmZq9erVevvttxUZGamDBw9Kklwulzp16qTPP/9cq1at0s0336yYmBh98sknmjFjhq655hpdf/31FqcHAADBKOT/FmF%2Bfr7q6uqUmpqquLg4/7ZmzRpJUkREhN5//33deOONuuqqq/Twww8rLS1NGzduVFhYmMXpAQBAMAr5GayW1vi73W6VlJS0URoAANAe2HYG669//auOHz9udQwAAIBmbFuwsrOzFRsbqwceeEAffvih1XEAAAD8bFuwDhw4oFdffVU1NTUaMmSI%2BvTpo2effVaHDx%2B2OhoAAAhxti1Y4eHhGjt2rNatW6d9%2B/YpIyNDr776qhISEjR27FitX7%2B%2BxfVVAAAArcG2Bev7YmNjNXLkSKWmpsrhcKi8vFwTJ05Uz549tXnzZqvjAQCAEGPrgnXkyBG98MILGjBggK6//nrV1tZq7dq1%2Buqrr/T111/r1ltv1a9%2B9SurYwIAgBBj29c03HHHHdqwYYMSExN1//33KyMjQ127dvUfv/jii/Xoo4/qxRdftDAlAAAIRbYtWFFRUdq4caOGDh16znPi4uJUVVXVhqkAAABsXLBWrFjR4jkOh0NXXHFFG6QBAAD4D9uuwXrkkUe0aNGiZuN//vOfNWPGDAsSAQAAnGHbgvXGG2/ouuuuazaekpLi/z2CAAAAVrBtwTpy5IguvfTSZuNRUVE6cuSIBYkAAADOsG3BuuKKK/Tuu%2B82G3/33XeVmJhoQSIAAIAzbLvIffr06Zo%2Bfbq%2B%2BeYbjRgxQpL0/vvva8GCBXrmmWcsTgcAAEKZbQvW1KlTdfz4cc2bN09PPPGEJCkhIUEvvviifv3rX1ucDgAAhDLbFixJysrKUlZWlmpqatSpUyddcsklVkcCAACwd8H6TlxcnNURAAAA/Gy7yP3w4cO67777dPnll6tjx46KiIgI2AAAAKxi2xmsyZMn6/PPP9fvf/97xcXFyeFwWB0JAABAko0LVmlpqUpLS3XNNddYHQUAACCAbR8RJiQkMGsFAACCkm0L1vPPP6%2BcnBzt37/f6igAAAABbPuIcNKkSaqvr1ePHj0UFRWlDh06BByvra21KBkAAAh1ti1Y8%2BfPtzoCAADAWdm2YE2ZMsXqCAAAAGdl2zVYkvTll18qNzdXkyZN8j8SfO%2B99/Tpp59anAwAAIQy2xaszZs3q0%2BfPiopKdHrr7%2BuhoYGSdJHH32kxx9/3OJ0AAAglNm2YP3hD39Qbm6u/vGPfwS8uX3EiBEqKyuzMBkAAAh1ti1YO3bs0F133dVsvFu3bjp8%2BLAFiQAAAM6wbcG65JJLdPDgwWbjlZWVuuyyy877Onl5ebr22msVGRmpbt26acyYMdqzZ0/AOV6vV1lZWYqJiVHnzp1122238f4tAABwTrYtWBMmTNBjjz2mw4cP%2B9/o/sEHH2jmzJm69957z/s6JSUlyszMVFlZmYqLi3Xq1CmlpaWpsbHRf8706dNVWFiogoICbdmyRQ0NDbr11lvV1NRk/PsCAAD25/D5fD6rQ/wvTpw4oUmTJunNN9/U6dOnFRERoZMnT2rcuHFauXKlwsP/tzdQHD58WN26dVNJSYmGDRumuro6de3aVStXrtT48eMlSQcOHJDb7daGDRt04403tnhNj8cjl8uluro6RUVF/U%2B5ziV5dpHR67Wm8qdusjoCACCEtObP35bY9j1YERERWrNmjf71r3/po48%2B0unTp5WUlKSf/exnP%2Bq6dXV1kqTo6GhJUkVFhU6ePKm0tDT/OfHx8erbt6%2B2bt16XgULAACEFtsWrO/06tVLvXr1MnItn8%2Bn7OxsDRkyRH379pUkHTx4UBEREbr00ksDzu3evftZ14BJZ9Zseb1e/77H4zGSDwAA2INtC9ZvfvObHzy%2BdOnSC77mQw89pB07dmjLli0tnuvz%2Bfxrv/5bXl6e5syZc8FfHwAAtA%2B2LVg1NTUB%2BydPntTu3btVX1%2BvYcOGXfD1srKytG7dOpWWliohIcE/HhsbqxMnTujbb78NmMWqra3V4MGDz3qtnJwcZWdn%2B/c9Ho/cbvcFZwIAAPZk24L1t7/9rdnYqVOn9OCDD6p3797nfR2fz6esrCwVFhZq06ZNSkxMDDg%2BcOBAdejQQcXFxRo3bpykM%2BVu165dWrBgwVmv6XQ65XQ6L%2BC7AQAA7YltC9bZhIeHa%2BbMmUpNTQ2YQfohmZmZWr16td5%2B%2B21FRkb611W5XC516tRJLpdLU6ZM0YwZM9SlSxdFR0dr5syZ6tevn0aNGtWa3w4AALCpdlWwJOmLL77QyZMnz/v8/Px8SVJqamrA%2BLJlyzR58mRJ0vPPP6/w8HCNGzdOx44d08iRI7V8%2BXKFhYWZig0AANoR2xasRx99NGDf5/OppqZG69at0z333HPe1zmf14B17NhRL730kl566aULzgkAAEKPbQvWP//5z4D9n/zkJ%2Bratavmz5%2BvqVOnWpQKAADAxgVr8%2BbNVkcAAAA4K9v%2BLkIAAIBgZdsZrGuvvfacL/r8bx9%2B%2BGErpwEAAPgP2xas4cOHa8mSJerVq5dSUlIkSWVlZdqzZ48eeOAB3kMFAAAsY9uCdfToUWVmZmrevHkB47Nnz9ahQ4f0yiuvWJQMAACEOtuuwXr99dd13333NRufPHmy3njjDQsSAQAAnGHbguV0OrV169Zm41u3buXxIAAAsJRtHxE%2B/PDDmjZtmrZv367rrrtO0pk1WC%2B//LJmzZplcToAABDKbFuwZs%2BercTERC1cuFCvvvqqJKl37956%2BeWXNXHiRIvTAQCAUGbbgiVJEydOpEwBAICgY9s1WJLk8Xi0fPlyPf744/r2228lSR9//LFqamosTgYAAEKZbWewdu3apVGjRumiiy5SdXW1Jk%2BerEsvvVSvv/669u/frxUrVlgdEQAAhCjbzmA98sgjmjhxoj7//HN17NjRP37LLbeotLTUwmQAACDU2XYGa9u2bcrPz2/263Iuu%2BwyHhECAABL2XYGKyIiQg0NDc3Gq6qqFBMTY0EiAACAM2xbsG677Tb96U9/0qlTpyRJDodDX3/9tR577DGNHTvW4nQAACCU2bZgPfvsszpw4IBiY2N17NgxjRgxQj/96U/VsWPHZr%2BfEAAAoC3Zdg2Wy%2BXS1q1bVVxcrI8%2B%2BkinT59WUlKSbrzxxmbrsgAAANqSLQvWyZMndfPNN2vx4sVKS0tTWlqa1ZEAAAD8bPmIsEOHDtq%2BfTszVQAAICjZsmBJ0r333qtly5ZZHQMAAKAZWz4i/M6iRYu0ceNGJScnq3PnzgHHFixYYFEqAAAQ6mxbsCoqKtS/f39J0o4dOwKO8egQAABYyXYF64svvlBiYqI2b95sdRQAAICzst0arJ49e%2Brw4cP%2B/fHjx%2BvQoUMWJgIAAAhku4Ll8/kC9jds2KDGxkaL0gAAADRnu4IFAAAQ7GxXsBwOR7NF7CxqBwAAwcR2i9x9Pp8mT54sp9MpSTp%2B/LimTZvW7DUNb7311nldr7S0VE8//bQqKipUU1OjwsJCjRkzxn988uTJWrFiRcBnBg0apLKysh/5nQAAgPbKdgUrIyMjYP/ee%2B/9UddrbGzUgAEDdN999%2BnOO%2B886zk33XRTwEtNIyIiftTXBAAA7ZvtCpbpt7enp6crPT39B89xOp2KjY01%2BnUBAED7Zbs1WFbYtGmTunXrpl69emnq1Kmqra21OhIAAAhitpvBamvp6en65S9/qR49emjv3r364x//qBEjRqiiosK/Duy/eb1eeb1e/77H42mruAAAIAhQsFowfvx4/z/37dtXycnJ6tGjh9avX6%2BxY8ee9TN5eXmaM2dOW0UEAABBhkeEFyguLk49evRQVVXVOc/JyclRXV2df6uurm7DhAAAwGrMYF2gb775RtXV1YqLizvnOU6n85yPDwEAQPsX8gWroaFBn332mX9/7969qqysVHR0tKKjo5Wbm6s777xTcXFx%2BvLLLzVr1izFxMTojjvusDA1AAAIZiFfsMrLyzV8%2BHD/fnZ2tqQz79vKz8/Xzp079dprr%2Bno0aOKi4vT8OHDtWbNGkVGRloVGQAABLmQL1ipqanNfoH097377rttmAYAALQHLHIHAAAwjIIFAABgWMg/IgQAoL1Knl1kdYTzVv7UTVZHMIoZLAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIaFfMEqLS3V6NGjFR8fL4fDobVr1wYc9/l8ys3NVXx8vDp16qTU1FTt3r3borQAAMAOQr5gNTY2asCAAVq0aNFZjy9YsEDPPfecFi1apG3btik2NlY33HCD6uvr2zgpAACwi3CrA1gtPT1d6enpZz3m8/n0wgsvaPbs2Ro7dqwkacWKFerevbtWr16tBx54oC2jAgAAmwj5GawfsnfvXh08eFBpaWn%2BMafTqV/84hfaunWrhckAAEAwC/kZrB9y8OBBSVL37t0Dxrt3766vvvrqnJ/zer3yer3%2BfY/H0zoBAQBAUGIG6zw4HI6AfZ/P12zs%2B/Ly8uRyufyb2%2B1u7YgAACCIULB%2BQGxsrKT/zGR9p7a2ttms1vfl5OSorq7Ov1VXV7dqTgAAEFwoWD8gMTFRsbGxKi4u9o%2BdOHFCJSUlGjx48Dk/53Q6FRUVFbABAIDQEfJrsBoaGvTZZ5/59/fu3avKykpFR0fr8ssv1/Tp0zVv3jz17NlTPXv21Lx583TRRRdp4sSJFqYGAADBLOQLVnl5uYYPH%2B7fz87OliRlZGRo%2BfLlevTRR3Xs2DH99re/1bfffqtBgwbpvffeU2RkpFWRAQBAkAv5gpWamiqfz3fO4w6HQ7m5ucrNzW27UAAAwNZYgwUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwLBwqwMAAEJX8uwiqyNckPKnbrI6AmyCGSwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMN6DBQDtjN3eLWUn3FucL2awAAAADKNgAQAAGEbBakFubq4cDkfAFhsba3UsAAAQxFiDdR769OmjjRs3%2BvfDwsIsTAMAAIIdBes8hIeHM2sFAADOG48Iz0NVVZXi4%2BOVmJioCRMm6IsvvrA6EgAACGLMYLVg0KBBeu2119SrVy8dOnRIc%2BfO1eDBg7V792516dLlrJ/xer3yer3%2BfY/H01ZxAQBAEGAGqwXp6em688471a9fP40aNUrr16%2BXJK1YseKcn8nLy5PL5fJvbre7reICAIAgQMG6QJ07d1a/fv1UVVV1znNycnJUV1fn36qrq9swIQAAsBqPCC%2BQ1%2BvVp59%2BqqFDh57zHKfTKafT2YapAABAMGEGqwUzZ85USUmJ9u7dqw8%2B%2BEB33XWXPB6PMjIyrI4GAACCFDNYLdi/f7/uvvtuHTlyRF27dtV1112nsrIy9ejRw%2BpoAAAgSFGwWlBQUGB1BAAAYDM8IgQAADCMggUAAGAYjwgBoAXJs4usjgDAZpjBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADAu3OgAQrJJnF1kd4YKUP3WT1REAAP%2BPGSwAAADDKFgAAACGUbAAAAAMo2Cdp8WLFysxMVEdO3bUwIEDtXnzZqsjAQCAIEXBOg9r1qzR9OnTNXv2bG3fvl1Dhw5Venq69u3bZ3U0AAAQhChY5%2BG5557TlClTdP/996t379564YUX5Ha7lZ%2Bfb3U0AAAQhHhNQwtOnDihiooKPfbYYwHjaWlp2rp161k/4/V65fV6/ft1dXWSJI/HYzxfk7fR%2BDVbS2t8/63JTvdWst/9tRO7/bcA2FFr/Bn23TV9Pp/xa7eEgtWCI0eOqKmpSd27dw8Y7969uw4ePHjWz%2BTl5WnOnDnNxt1ud6tktAvXs1YnaN%2B4vwDsrDX/DKuvr5fL5Wq9L3AWFKzz5HA4AvZ9Pl%2Bzse/k5OQoOzvbv3/69Gn9%2B9//VpcuXc75mf%2BFx%2BOR2%2B1WdXW1oqKijF0X3NvWxL1tXdzf1sO9bT2tdW99Pp/q6%2BsVHx9v7Jrni4LVgpiYGIWFhTWbraqtrW02q/Udp9Mpp9MZMHbJJZe0WsaoqCj%2BZ28l3NvWw71tXdzf1sO9bT2tcW/beubqOyxyb0FERIQGDhyo4uLigPHi4mINHjzYolQAACCYMYN1HrKzszVp0iQlJycrJSVFS5cu1b59%2BzRt2jSrowEAgCAUlpubm2t1iGDXt29fdenSRfPmzdMzzzyjY8eOaeXKlRowYIDV0RQWFqbU1FSFh9OVTePeth7ubevi/rYe7m3raW/31uGz4u8uAgAAtGOswQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCybWrx4sRITE9WxY0cNHDhQmzdvtjpSu1BaWqrRo0crPj5eDodDa9eutTpSu5GXl6drr71WkZGR6tatm8aMGaM9e/ZYHatdyM/PV//%2B/f0vaUxJSdE777xjdax2KS8vTw6HQ9OnT7c6SruQm5srh8MRsMXGxlodywgKlg2tWbNG06dP1%2BzZs7V9%2B3YNHTpU6enp2rdvn9XRbK%2BxsVEDBgzQokWLrI7S7pSUlCgzM1NlZWUqLi7WqVOnlJaWpsZGfpHyj5WQkKD58%2BervLxc5eXlGjFihG6//Xbt3r3b6mjtyrZt27R06VL179/f6ijtSp8%2BfVRTU%2BPfdu7caXUkI3hNgw0NGjRISUlJys/P94/17t1bY8aMUV5enoXJ2heHw6HCwkKNGTPG6ijt0uHDh9WtWzeVlJRo2LBhVsdpd6Kjo/X0009rypQpVkdpFxoaGpSUlKTFixdr7ty5%2BvnPf64XXnjB6li2l5ubq7Vr16qystLqKMYxg2UzJ06cUEVFhdLS0gLG09LStHXrVotSAReurq5O0pkiAHOamppUUFCgxsZGpaSkWB2n3cjMzNQtt9yiUaNGWR2l3amqqlJ8fLwSExM1YcIEffHFF1ZHMqJ9vC41hBw5ckRNTU3NftHKh%2BjJAAADQklEQVR09%2B7dm/1CaiBY%2BXw%2BZWdna8iQIerbt6/VcdqFnTt3KiUlRcePH9fFF1%2BswsJCXX311VbHahcKCgpUUVGh8vJyq6O0O4MGDdJrr72mXr166dChQ5o7d64GDx6s3bt3q0uXLlbH%2B1EoWDblcDgC9n0%2BX7MxIFg99NBD2rFjh7Zs2WJ1lHbjqquuUmVlpY4ePao333xTGRkZKikpoWT9SNXV1frd736n9957Tx07drQ6TruTnp7u/%2Bd%2B/fopJSVFV1xxhVasWKHs7GwLk/14FCybiYmJUVhYWLPZqtra2mazWkAwysrK0rp161RaWqqEhASr47QbERERuvLKKyVJycnJ2rZtmxYuXKglS5ZYnMzeKioqVFtbq4EDB/rHmpqaVFpaqkWLFsnr9SosLMzChO1L586d1a9fP1VVVVkd5UdjDZbNREREaODAgSouLg4YLy4u1uDBgy1KBbTM5/PpoYce0ltvvaW///3vSkxMtDpSu%2Bbz%2BeT1eq2OYXsjR47Uzp07VVlZ6d%2BSk5N1zz33qLKyknJlmNfr1aeffqq4uDiro/xozGDZUHZ2tiZNmqTk5GSlpKRo6dKl2rdvn6ZNm2Z1NNtraGjQZ5995t/fu3evKisrFR0drcsvv9zCZPaXmZmp1atX6%2B2331ZkZKR/FtblcqlTp04Wp7O3WbNmKT09XW63W/X19SooKNCmTZtUVFRkdTTbi4yMbLZOsHPnzurSpQvrBw2YOXOmRo8ercsvv1y1tbWaO3euPB6PMjIyrI72o1GwbGj8%2BPH65ptv9OSTT6qmpkZ9%2B/bVhg0b1KNHD6uj2V55ebmGDx/u3/9uDUBGRoaWL19uUar24bvXiqSmpgaML1u2TJMnT277QO3IoUOHNGnSJNXU1Mjlcql///4qKirSDTfcYHU04Aft379fd999t44cOaKuXbvquuuuU1lZWbv4ecZ7sAAAAAxjDRYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMP%2BD7jyAD5ahE4/AAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common3052661021054984646">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">38</td>
        <td class="number">32.5%</td>
        <td>
            <div class="bar" style="width:92%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.5</td>
        <td class="number">8</td>
        <td class="number">6.8%</td>
        <td>
            <div class="bar" style="width:20%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:17%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:15%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:13%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.2</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.3</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">41</td>
        <td class="number">35.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme3052661021054984646">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">38</td>
        <td class="number">32.5%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:11%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">4.2</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:13%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.3</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:13%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.5</td>
        <td class="number">8</td>
        <td class="number">6.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:13%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:62%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Canine largest age">Canine largest age<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>9</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>7.7%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>29.1%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>34</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Mean</th>
                    <td>2.0241</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>5</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>32.5%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram7523693738087770258">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAcRJREFUeJzt3TGO01AUhtHriDZeQGTvgp6WPdEhsScktsAubGUBjigoiCkgNDP6RUboOR7OKRPpXUf2J8dJ8bp1XddqbJ7nGsex9Vh2bpqmGoah6cw3Taf9djweq%2BrXB%2B77fotDYEeWZalxHP9cNy1tEkjXdVVV1ff9k0Defvh893pfP73/J8fFY7tdNy0dmk%2BEHREIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUCwyfYH7M9LtqV4iUfbysIdBAKBQCAQCAQCgUAgEAgEfuZ9MHb5fSwCeQXujUpQf89XLAjcQf5Drf4Vfw3cQSAQCAQCgUAgEGzykL6ua1VVLcvy5L0f37/dvd5z6zyCdx%2B/bH0Iu/Pcuby9drtuWurWDabO81zjOLYey85N01TDMDSduUkg1%2Bu1zudzHY/H6rqu9Xh2Zl3XulwudTqd6nBo%2B1SwSSCwFx7SIRAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAh%2BArrxUlmkNue/AAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives7523693738087770258,#minihistogram7523693738087770258"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives7523693738087770258">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles7523693738087770258"
                                                  aria-controls="quantiles7523693738087770258" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram7523693738087770258" aria-controls="histogram7523693738087770258"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common7523693738087770258" aria-controls="common7523693738087770258"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme7523693738087770258" aria-controls="extreme7523693738087770258"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles7523693738087770258">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>2.5</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>4</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>4.95</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>4</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>1.9691</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.97284</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.753</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>2.0241</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.854</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.099451</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>168</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>3.8775</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram7523693738087770258">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3X9QVXXi//HXDeRqBrcQFYibw5q2JuqmOIqli5oYlWXWqlkutuZmS7SGbBs6W9iaOP3UcmP6sam5NVhTmDsZhe0KOi4lJKXUtKSWmCjaFhcYvSLe7x9%2Bu5/uoqnbG8493Odj5sx43ufcw4tbzn3N%2B7w91%2BHz%2BXwCAACAMedZHQAAAKCzoWABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGHhVgcIBSdOnND%2B/fsVGRkph8NhdRwAAEKCz%2BdTY2Oj4uPjdd55HTunRMHqAPv375fb7bY6BgAAIam2tlYJCQkd%2BjMpWB0gMjJS0sn/wFFRURanAQAgNHg8Hrndbv/ncEeiYHWA728LRkVFUbAAAOhgVizPYZE7AACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAzjy55tLnlhsdURzlrFI9dYHQEAgA7BDBYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIaFfMEqKCjQ4MGDFRUVpaioKKWkpOidd97xH09NTZXD4QjYpk%2BfbmFiAAAQ7MKtDmC1hIQELV26VJdeeqkkafXq1brxxhu1fft2DRw4UJI0Z84cPfzww/7XdOvWzZKsAADAHkK%2BYE2aNClg/5FHHlFBQYHKy8v9Bev8889XbGysFfEAAIANhfwtwh9qbW1VYWGhmpublZKS4h9/5ZVXFBMTo4EDByonJ0eNjY0/eh2v1yuPxxOwAQCA0BHyM1iStGPHDqWkpOjo0aO64IILVFRUpMsvv1ySdNtttykxMVGxsbHauXOncnNz9fHHH6ukpOS018vPz9eiRYs6Kj4AAAgyDp/P57M6hNWOHTumvXv36rvvvtMbb7yhF198UaWlpf6S9UOVlZVKTk5WZWWlhg4desrreb1eeb1e/77H45Hb7VZDQ4OioqKMZk9eWGz0eu2p4pFrrI4AAAghHo9HLperXT5/z4QZLEkRERH%2BRe7Jycnatm2bli9frueee67NuUOHDlWXLl1UU1Nz2oLldDrldDrbNTMAAAherME6BZ/PFzAD9UPV1dVqaWlRXFxcB6cCAAB2EfIzWAsWLFB6errcbrcaGxtVWFioTZs2qbi4WLt27dIrr7yia6%2B9VjExMfr00081f/58XXHFFbryyiutjg4AAIJUyBesgwcPaubMmaqrq5PL5dLgwYNVXFysCRMmqLa2Vu%2B//76WL1%2BupqYmud1uXXfddXrooYcUFhZmdXQAABCkQr5g/fWvfz3tMbfbrdLS0g5MAwAAOgPWYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhIV%2BwCgoKNHjwYEVFRSkqKkopKSl65513/Me9Xq%2BysrIUExOj7t2764YbbtC%2BffssTAwAAIJdyBeshIQELV26VBUVFaqoqNC4ceN04403qrq6WpI0b948FRUVqbCwUFu2bFFTU5Ouv/56tba2WpwcAAAEK4fP5/NZHSLYREdH67HHHtMtt9yinj17as2aNZo2bZokaf/%2B/XK73dqwYYMmTpx4VtfzeDxyuVxqaGhQVFSU0azJC4uNXq89VTxyjdURAAAhpD0/f88k5Gewfqi1tVWFhYVqbm5WSkqKKisr1dLSorS0NP858fHxSkpK0tatW097Ha/XK4/HE7ABAIDQQcGStGPHDl1wwQVyOp2aO3euioqKdPnll%2BvAgQOKiIjQRRddFHB%2B7969deDAgdNeLz8/Xy6Xy7%2B53e72/hUAAEAQoWBJuuyyy1RVVaXy8nLdfffdysjI0Keffnra830%2BnxwOx2mP5%2BbmqqGhwb/V1ta2R2wAABCkwq0OEAwiIiJ06aWXSpKSk5O1bds2LV%2B%2BXNOmTdOxY8f07bffBsxi1dfXa9SoUae9ntPplNPpbPfcAAAgODGDdQo%2Bn09er1fDhg1Tly5dVFJS4j9WV1ennTt3/mjBAgAAoS3kZ7AWLFig9PR0ud1uNTY2qrCwUJs2bVJxcbFcLpdmz56t%2BfPnq0ePHoqOjlZOTo4GDRqkq6%2B%2B2uroAAAgSIV8wTp48KBmzpypuro6uVwuDR48WMXFxZowYYIk6amnnlJ4eLimTp2qI0eOaPz48Vq1apXCwsIsTg4AAIIVz8HqADwH6ySegwUA6Eg8BwsAAKAToWABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAsJAvWPn5%2BRo%2BfLgiIyPVq1cvTZ48WZ9//nnAOampqXI4HAHb9OnTLUoMAACCXcgXrNLSUmVmZqq8vFwlJSU6fvy40tLS1NzcHHDenDlzVFdX59%2Bee%2B45ixIDAIBgF251AKsVFxcH7K9cuVK9evVSZWWlxowZ4x8///zzFRsb29HxAACADYX8DNZ/a2hokCRFR0cHjL/yyiuKiYnRwIEDlZOTo8bGxtNew%2Bv1yuPxBGwAACB0hPwM1g/5fD5lZ2frqquuUlJSkn/8tttuU2JiomJjY7Vz507l5ubq448/VklJySmvk5%2Bfr0WLFnVUbAAAEGQcPp/PZ3WIYJGZmam3335bW7ZsUUJCwmnPq6ysVHJysiorKzV06NA2x71er7xer3/f4/HI7XaroaFBUVFRRjMnLyw%2B80lBouKRa6yOAAAIIR6PRy6Xq10%2Bf8%2BEGaz/LysrS%2BvXr1dZWdmPlitJGjp0qLp06aKamppTFiyn0ymn09leUQEAQJAL%2BYLl8/mUlZWloqIibdq0SYmJiWd8TXV1tVpaWhQXF9cBCQEAgN2EfMHKzMzUq6%2B%2BqrfeekuRkZE6cOCAJMnlcqlbt27atWuXXnnlFV177bWKiYnRp59%2Bqvnz5%2BuKK67QlVdeaXF6AAAQjEL%2BXxEWFBSooaFBqampiouL829r166VJEVEROj999/XxIkTddlll%2Bnee%2B9VWlqaNm7cqLCwMIvTAwCAYBTyM1hnWuPvdrtVWlraQWkAAEBnYNsZrL/97W86evSo1TEAAADasG3Bys7OVmxsrO666y59%2BOGHVscBAADws23B2r9/v1566SXV1dXpqquu0sCBA/XEE0/o0KFDVkcDAAAhzrYFKzw8XFOmTNH69eu1d%2B9eZWRk6KWXXlJCQoKmTJmit99%2B%2B4zrqwAAANqDbQvWD8XGxmr8%2BPFKTU2Vw%2BFQRUWFZsyYoX79%2Bmnz5s1WxwMAACHG1gXr8OHDWrZsmYYMGaIrr7xS9fX1Wrdunb766it9/fXXuv766/XrX//a6pgAACDE2PYxDTfddJM2bNigxMRE3XnnncrIyFDPnj39xy%2B44ALdf//9evrppy1MCQAAQpFtC1ZUVJQ2btyo0aNHn/acuLg41dTUdGAqAAAAGxes1atXn/Ech8Ohvn37dkAaAACA/2PbNVj33XefVqxY0Wb8L3/5i%2BbPn29BIgAAgJNsW7Bef/11jRw5ss14SkqK/3sEAQAArGDbgnX48GFddNFFbcajoqJ0%2BPBhCxIBAACcZNuC1bdvX7377rttxt99910lJiZakAgAAOAk2y5ynzdvnubNm6dvvvlG48aNkyS9//77evTRR/X4449bnA4AAIQy2xasOXPm6OjRo1qyZIkeeughSVJCQoKefvpp/eY3v7E4HQAACGW2LViSlJWVpaysLNXV1albt2668MILrY4EAABg74L1vbi4OKsjAAAA%2BNl2kfuhQ4d0xx136JJLLlHXrl0VERERsAEAAFjFtjNYs2bN0q5du/SHP/xBcXFxcjgcVkcCAACQZOOCVVZWprKyMl1xxRVWRwEAAAhg21uECQkJzFoBAICgZNuC9dRTTyk3N1f79u2zOgoAAEAA294inDlzphobG9WnTx9FRUWpS5cuAcfr6%2BstSgYAAEKdbQvW0qVLrY4AAABwSrYtWLNnz7Y6AgAAwCnZdg2WJH355ZfKy8vTzJkz/bcE33vvPX322WcWJwMAAKHMtgVr8%2BbNGjhwoEpLS/Xaa6%2BpqalJkvTRRx/pwQcftDgdAAAIZbYtWH/84x%2BVl5enf/7znwFPbh83bpzKy8stTAYAAEKdbQvWJ598oltuuaXNeK9evXTo0CELEgEAAJxk24J14YUX6sCBA23Gq6qqdPHFF5/1dfLz8zV8%2BHBFRkaqV69emjx5sj7//POAc7xer7KyshQTE6Pu3bvrhhtu4PlbAADgtGxbsKZPn64HHnhAhw4d8j/R/YMPPlBOTo5uv/32s75OaWmpMjMzVV5erpKSEh0/flxpaWlqbm72nzNv3jwVFRWpsLBQW7ZsUVNTk66//nq1trYa/70AAID9OXw%2Bn8/qEP%2BLY8eOaebMmXrjjTd04sQJRUREqKWlRVOnTtWaNWsUHv6/PYHi0KFD6tWrl0pLSzVmzBg1NDSoZ8%2BeWrNmjaZNmyZJ2r9/v9xutzZs2KCJEyee8Zoej0cul0sNDQ2Kior6n3KdTvLCYqPXa08Vj1xjdQQAQAhpz8/fM7Htc7AiIiK0du1a/fvf/9ZHH32kEydOaOjQofr5z3/%2Bk67b0NAgSYqOjpYkVVZWqqWlRWlpaf5z4uPjlZSUpK1bt55VwQIAAKHFtgXre/3791f//v2NXMvn8yk7O1tXXXWVkpKSJEkHDhxQRESELrroooBze/fufco1YNLJNVter9e/7/F4jOQDAAD2YNuC9dvf/vZHjz///PPnfM177rlHn3zyibZs2XLGc30%2Bn3/t13/Lz8/XokWLzvnnAwCAzsG2Bauuri5gv6WlRdXV1WpsbNSYMWPO%2BXpZWVlav369ysrKlJCQ4B%2BPjY3VsWPH9O233wbMYtXX12vUqFGnvFZubq6ys7P9%2Bx6PR263%2B5wzAQAAe7Jtwfr73//eZuz48eO6%2B%2B67NWDAgLO%2Bjs/nU1ZWloqKirRp0yYlJiYGHB82bJi6dOmikpISTZ06VdLJcrdz5049%2Buijp7ym0%2BmU0%2Bk8h98GAAB0JrYtWKcSHh6unJwcpaamBswg/ZjMzEy9%2BuqreuuttxQZGelfV%2BVyudStWze5XC7Nnj1b8%2BfPV48ePRQdHa2cnBwNGjRIV199dXv%2BOgAAwKY6VcGSpN27d6ulpeWszy8oKJAkpaamBoyvXLlSs2bNkiQ99dRTCg8P19SpU3XkyBGNHz9eq1atUlhYmKnYAACgE7Ftwbr//vsD9n0%2Bn%2Brq6rR%2B/XrddtttZ32ds3kMWNeuXfXMM8/omWeeOeecAAAg9Ni2YP3rX/8K2D/vvPPUs2dPLV26VHPmzLEoFQAAgI0L1ubNm62OAAAAcEq2/S5CAACAYGXbGazhw4ef9kGf/%2B3DDz9s5zQAAAD/x7YFa%2BzYsXruuefUv39/paSkSJLKy8v1%2Beef66677uI5VAAAwDK2LVjfffedMjMztWTJkoDxhQsX6uDBg3rxxRctSgYAAEKdbddgvfbaa7rjjjvajM%2BaNUuvv/66BYkAAABOsm3Bcjqd2rp1a5vxrVu3cnsQAABYyra3CO%2B9917NnTtX27dv18iRIyWdXIP1wgsvaMGCBRanAwAAocy2BWvhwoVKTEzU8uXL9dJLL0mSBgwYoBdeeEEzZsywOB0AAAhlti1YkjRjxgzKFAAACDq2XYMlSR6PR6tWrdKDDz6ob7/9VpL08ccfq66uzuJkAAAglNl2Bmvnzp26%2Buqrdf7556u2tlazZs3SRRddpNdee0379u3T6tWrrY4IAABClG1nsO677z7NmDFDu3btUteuXf3j1113ncrKyixMBgAAQp1tZ7C2bdumgoKCNl%2BXc/HFF3OLEAAAWMq2M1gRERFqampqM15TU6OYmBgLEgEAAJxk24J1ww036M9//rOOHz8uSXI4HPr666/1wAMPaMqUKRanAwAAocy2BeuJJ57Q/v37FRsbqyNHjmjcuHH62c9%2Bpq5du7b5fkIAAICOZNs1WC6XS1u3blVJSYk%2B%2BugjnThxQkOHDtXEiRPbrMsCAADoSLYsWC0tLbr22mv17LPPKi0tTWlpaVZHAgAA8LPlLcIuXbpo%2B/btzFQBAICgZMuCJUm33367Vq5caXUMAACANmx5i/B7K1as0MaNG5WcnKzu3bsHHHv00UctSgUAAEKdbQtWZWWlBg8eLEn65JNPAo5x6xAAAFjJdgVr9%2B7dSkxM1ObNm62OAgAAcEq2W4PVr18/HTp0yL8/bdo0HTx40MJEAAAAgWxXsHw%2BX8D%2Bhg0b1NzcbFEaAACAtmxXsAAAAIKd7QqWw%2BFos4idRe0AACCY2G6Ru8/n06xZs%2BR0OiVJR48e1dy5c9s8puHNN988q%2BuVlZXpscceU2Vlperq6lRUVKTJkyf7j8%2BaNUurV68OeM2IESNUXl7%2BE38TAADQWdmuYGVkZATs33777T/pes3NzRoyZIjuuOMO3Xzzzac855prrgl4qGlERMRP%2BpkAAKBzs13BMv309vT0dKWnp//oOU6nU7GxsUZ/LgAA6LxstwbLCps2bVKvXr3Uv39/zZkzR/X19VZHAgAAQcx2M1gdLT09Xb/61a/Up08f7dmzR3/60580btw4VVZW%2BteB/Tev1yuv1%2Bvf93g8HRUXAAAEAQrWGUybNs3/56SkJCUnJ6tPnz56%2B%2B23NWXKlFO%2BJj8/X4sWLeqoiAAAIMhwi/AcxcXFqU%2BfPqqpqTntObm5uWpoaPBvtbW1HZgQAABYjRmsc/TNN9%2BotrZWcXFxpz3H6XSe9vYhAADo/EK%2BYDU1NemLL77w7%2B/Zs0dVVVWKjo5WdHS08vLydPPNNysuLk5ffvmlFixYoJiYGN10000WpgYAAMEs5AtWRUWFxo4d69/Pzs6WdPJ5WwUFBdqxY4defvllfffdd4qLi9PYsWO1du1aRUZGWhUZAAAEuZAvWKmpqW2%2BQPqH3n333Q5MAwAAOgMWuQMAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwLOS/KgcAgLOVvLDY6gidVsUj11gdwShmsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGBbyBausrEyTJk1SfHy8HA6H1q1bF3Dc5/MpLy9P8fHx6tatm1JTU1VdXW1RWgAAYAchX7Cam5s1ZMgQrVix4pTHH330UT355JNasWKFtm3bptjYWE2YMEGNjY0dnBQAANhFuNUBrJaenq709PRTHvP5fFq2bJkWLlyoKVOmSJJWr16t3r1769VXX9Vdd93VkVEBAIBNhPwM1o/Zs2ePDhw4oLS0NP%2BY0%2BnUL3/5S23dutXCZAAAIJiF/AzWjzlw4IAkqXfv3gHjvXv31ldffXXa13m9Xnm9Xv%2B%2Bx%2BNpn4AAACAoMYN1FhwOR8C%2Bz%2BdrM/ZD%2Bfn5crlc/s3tdrd3RAAAEEQoWD8iNjZW0v/NZH2vvr6%2BzazWD%2BXm5qqhocG/1dbWtmtOAAAQXChYPyIxMVGxsbEqKSnxjx07dkylpaUaNWrUaV/ndDoVFRUVsAEAgNAR8muwmpqa9MUXX/j39%2BzZo6qqKkVHR%2BuSSy7RvHnztGTJEvXr10/9%2BvXTkiVLdP7552vGjBkWpgYAAMEs5AtWRUWFxo4d69/Pzs6WJGVkZGjVqlW6//77deTIEf3ud7/Tt99%2BqxEjRui9995TZGSkVZEBAECQC/mClZqaKp/Pd9rjDodDeXl5ysvL67hQAADA1liDBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAsHCrAwBAsEteWGx1hHNS8cg1VkcAQh4zWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhPAcLADoZOz23i2d2obNiBgsAAMAwChYAAIBhFKwzyMvLk8PhCNhiY2OtjgUAAIIYa7DOwsCBA7Vx40b/flhYmIVpAABAsKNgnYXw8HBmrQAAwFnjFuFZqKmpUXx8vBITEzV9%2BnTt3r3b6kgAACCIMYN1BiNGjNDLL7%2Bs/v376%2BDBg1q8eLFGjRql6upq9ejR45Sv8Xq98nq9/n2Px9NRcQEAQBCgYJ1Benq6/8%2BDBg1SSkqK%2Bvbtq9WrVys7O/uUr8nPz9eiRYs6KiIA2JadntkFnAtuEZ6j7t27a9CgQaqpqTntObm5uWpoaPBvtbW1HZgQAABYjRmsc%2BT1evXZZ59p9OjRpz3H6XTK6XR2YCoAABBMmME6g5ycHJWWlmrPnj364IMPdMstt8jj8SgjI8PqaAAAIEgxg3UG%2B/bt06233qrDhw%2BrZ8%2BeGjlypMrLy9WnTx%2BrowEAgCBFwTqDwsJCqyMAAACb4RYhAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAw8KtDgAg9CQvLLY6AgC0K2awAAAADKNgAQAAGEbBAgAAMIyCdZaeffZZJSYmqmvXrho2bJg2b95sdSQAABCkKFhnYe3atZo3b54WLlyo7du3a/To0UpPT9fevXutjgYAAIIQBessPPnkk5o9e7buvPNODRgwQMuWLZPb7VZBQYHV0QAAQBDiMQ1ncOzYMVVWVuqBBx4IGE9LS9PWrVtP%2BRqv1yuv1%2Bvfb2hokCR5PB7j%2BVq9zcav2V7a4/eHPdnp/1sAHaM9PiO%2Bv6bP5zN%2B7TOhYJ3B4cOH1draqt69eweM9%2B7dWwcOHDjla/Lz87Vo0aI24263u10y2oXrCasTAACCVXt%2BRjQ2NsrlcrXfDzgFCtZZcjgcAfs%2Bn6/N2Pdyc3OVnZ3t3z9x4oT%2B85//qEePHqd9zf/C4/HI7XartrZWUVFRxq4L3tv2xHvbvnh/2w/vbftpr/fW5/OpsbFR8fHxxq55tihYZxATE6OwsLA2s1X19fVtZrW%2B53Q65XQ6A8YuvPDCdssYFRXFX/Z2wnvbfnhv2xfvb/vhvW0/7fHedvTM1fdY5H4GERERGjZsmEpKSgLGS0pKNGrUKItSAQCAYMYM1lnIzs7WzJkzlZycrJSUFD3//PPau3ev5s6da3U0AAAQhMLy8vLyrA4R7JKSktSjRw8tWbJEjz/%2BuI4cOaI1a9ZoyJAhVkdTWFiYUlNTFR5OVzaN97b98N62L97f9sN7234623vr8FnxbxcBAAA6MdZgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKlk09%2B%2ByzSkxMVNeuXTVs2DBt3rzZ6kidQllZmSZNmqT4%2BHg5HA6tW7fO6kidRn5%2BvoYPH67IyEj16tVLkydP1ueff251rE6hoKBAgwcP9j%2BkMSUlRe%2B8847VsTql/Px8ORwOzZs3z%2BoonUJeXp4cDkfAFhsba3UsIyhYNrR27VrNmzdPCxcu1Pbt2zV69Gilp6dr7969VkezvebmZg0ZMkQrVqywOkqnU1paqszMTJWXl6ukpETHjx9XWlqampv54uefKiEhQUuXLlVFRYUqKio0btw43XjjjaqurrY6Wqeybds2Pf/88xo8eLDVUTqVgQMHqq6uzr/t2LHD6khG8JgGGxoxYoSGDh2qgoIC/9iAAQM0efJk5efnW5isc3E4HCoqKtLkyZOtjtIpHTp0SL169VJpaanGjBljdZxOJzo6Wo899phmz55tdZROoampSUOHDtWzzz6rxYsX6xe/%2BIWWLVtmdSzby8vL07p161RVVWV1FOOYwbKZY8eOqbKyUmlpaQHjaWlp2rp1q0WpgHPX0NAg6WQRgDmtra0qLCxUc3OzUlJSrI7TaWRmZuq6667T1VdfbXWUTqempkbx8fFKTEzU9OnTtXv3bqsjGdE5HpcaQg4fPqzW1tY2XzTdu3fvNl9IDQQrn8%2Bn7OxsXXXVVUpKSrJndDcNAAADKUlEQVQ6TqewY8cOpaSk6OjRo7rgggtUVFSkyy%2B/3OpYnUJhYaEqKytVUVFhdZROZ8SIEXr55ZfVv39/HTx4UIsXL9aoUaNUXV2tHj16WB3vJ6Fg2ZTD4QjY9/l8bcaAYHXPPffok08%2B0ZYtW6yO0mlcdtllqqqq0nfffac33nhDGRkZKi0tpWT9RLW1tfr973%2Bv9957T127drU6TqeTnp7u//OgQYOUkpKivn37avXq1crOzrYw2U9HwbKZmJgYhYWFtZmtqq%2BvbzOrBQSjrKwsrV%2B/XmVlZUpISLA6TqcRERGhSy%2B9VJKUnJysbdu2afny5XruuecsTmZvlZWVqq%2Bv17Bhw/xjra2tKisr04oVK%2BT1ehUWFmZhws6le/fuGjRokGpqaqyO8pOxBstmIiIiNGzYMJWUlASMl5SUaNSoURalAs7M5/Ppnnvu0Ztvvql//OMfSkxMtDpSp%2Bbz%2BeT1eq2OYXvjx4/Xjh07VFVV5d%2BSk5N12223qaqqinJlmNfr1Weffaa4uDiro/xkzGDZUHZ2tmbOnKnk5GSlpKTo%2Beef1969ezV37lyro9leU1OTvvjiC//%2Bnj17VFVVpejoaF1yySUWJrO/zMxMvfrqq3rrrbcUGRnpn4V1uVzq1q2bxensbcGCBUpPT5fb7VZjY6MKCwu1adMmFRcXWx3N9iIjI9usE%2Bzevbt69OjB%2BkEDcnJyNGnSJF1yySWqr6/X4sWL5fF4lJGRYXW0n4yCZUPTpk3TN998o4cfflh1dXVKSkrShg0b1KdPH6uj2V5FRYXGjh3r3/9%2BDUBGRoZWrVplUarO4fvHiqSmpgaMr1y5UrNmzer4QJ3IwYMHNXPmTNXV1cnlcmnw4MEqLi7WhAkTrI4G/Kh9%2B/bp1ltv1eHDh9WzZ0%2BNHDlS5eXlneLzjOdgAQAAGMYaLAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhv0/eRz3lmNBs6cAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common7523693738087770258">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">38</td>
        <td class="number">32.5%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">13</td>
        <td class="number">11.1%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.5</td>
        <td class="number">8</td>
        <td class="number">6.8%</td>
        <td>
            <div class="bar" style="width:21%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">34</td>
        <td class="number">29.1%</td>
        <td>
            <div class="bar" style="width:89%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme7523693738087770258">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">38</td>
        <td class="number">32.5%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:46%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:39%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">13</td>
        <td class="number">11.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.5</td>
        <td class="number">8</td>
        <td class="number">6.8%</td>
        <td>
            <div class="bar" style="width:61%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:39%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Canine number">Canine number<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>9</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>7.7%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>23.9%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>28</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>1.5506</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>8</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>37.6%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram6867184384696783866">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAdFJREFUeJzt3TGO01AUhtHriNZZwCjeBT0VEntCokBiT0hU9OzCVhaQiIKCmAJCE/hRRuR5PDmnTKTcZ%2Bl9sZ9SpJvnea7GpmmqYRhaj2XlxnGs3W7XdOaLptN%2B6fu%2Bqn5e8Ha7XWIJrMjhcKhhGH7vm5YWCaTruqqq2m63F4G8fPvx6s/78uHNf1kXT9t537S0aT4RVkQgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgWCRf5ji7/zD1tMikCtcu3lt3PW720Ae803N/XEGgUAgEDyLRyyPS9yKOwgEAoFAIBAIBIJncUi/d37AvB13EAgWuYPM81xVVYfD4eK979%2B%2Btl7Ozfzp%2Bv6lxfU/Zl2v3n%2B6wUoufX73%2BuK183rP%2B6albl5g6jRNNQxD67Gs3DiOtdvtms5cJJDT6VT7/b76vq%2Bu61qPZ2Xmea7j8VgPDw%2B12bQ9FSwSCKyFQzoEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCwQ9mOVgxSnS1QgAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives6867184384696783866,#minihistogram6867184384696783866"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives6867184384696783866">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles6867184384696783866"
                                                  aria-controls="quantiles6867184384696783866" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram6867184384696783866" aria-controls="histogram6867184384696783866"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common6867184384696783866" aria-controls="common6867184384696783866"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme6867184384696783866" aria-controls="extreme6867184384696783866"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles6867184384696783866">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>1</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>3</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>4.6</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>8</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>8</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>3</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>1.883</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>1.2144</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>0.54564</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>1.5506</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.6197</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>1.0426</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>138</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>3.5457</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram6867184384696783866">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3X9U1fXhx/HXDeRqBCSoIIGNTNNEnYErzMrUKPJHzVPa/BGmtSwiCS1/sBW1BGeZ1SwXVtpyDuuUZmdlYit/HOYUkvxRx2w2xQTRzfjh9KLw%2Bf7h17vu0HTxxg8f7vNxzj1n93Ph9ronOz73uR8uLsuyLAEAAMCYC%2BweAAAA0NIQWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYF2j3AH9TX12v//v0KCQmRy%2BWyew4AAH7BsixVV1crOjpaF1xwfs8pEVjnwf79%2BxUbG2v3DAAA/FJpaaliYmLO6z%2BTwDoPQkJCJJ38FxwaGmrzGgAA/ENVVZViY2O9fw%2BfTwTWeXDqbcHQ0FACCwCA88yOy3O4yB0AAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwftmzwyVmrbJ7wjkrmnWL3RMAADgvOIMFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIH1Pbm5uXK5XMrIyPAe83g8Sk9PV7t27RQcHKzhw4dr3759Nq4EAADNHYH1/zZv3qy8vDz16tXL53hGRoaWL1%2Bu/Px8bdiwQTU1NRo6dKjq6upsWgoAAJo7AktSTU2NxowZo4ULF6pt27be45WVlXrttdc0d%2B5cDR48WH369NGSJUu0bds2rVmzxsbFAACgOSOwJKWlpWnIkCEaPHiwz/Hi4mIdP35cycnJ3mPR0dGKj49XYWHh%2BZ4JAAAcItDuAXbLz89XcXGxioqKGjxWXl6uoKAgn7NakhQZGany8vIzPqfH45HH4/Her6qqMjcYAAA0e359Bqu0tFSTJ0/WH//4R7Vu3fqcv8%2ByLLlcrjM%2Bnpubq7CwMO8tNjbWxFwAAOAQfh1YxcXFqqioUEJCggIDAxUYGKi1a9fqxRdfVGBgoCIjI1VbW6vDhw/7fF9FRYUiIyPP%2BLwzZsxQZWWl91ZaWtrULwUAADQjfv0W4aBBg7Rt2zafY/fcc4%2B6deumadOmKTY2Vq1atVJBQYFGjhwpSSorK9P27ds1Z86cMz6v2%2B2W2%2B1u0u0AAKD58uvACgkJUXx8vM%2Bx4OBgRUREeI9PnDhRU6ZMUUREhMLDwzV16lT17NmzwQXxAAAAp/h1YJ2LefPmKTAwUCNHjtTRo0c1aNAgLV68WAEBAXZPAwAAzZTLsizL7hEtXVVVlcLCwlRZWanQ0FCjz52Ytcro8zWlolm32D0BAOBHmvLv37Px64vcAQAAmgKBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYJjfB9aCBQvUq1cvhYaGKjQ0VElJSfrwww%2B9j3s8HqWnp6tdu3YKDg7W8OHDtW/fPhsXAwCA5s7vAysmJkazZ89WUVGRioqKNHDgQN12223asWOHJCkjI0PLly9Xfn6%2BNmzYoJqaGg0dOlR1dXU2LwcAAM2Vy7Isy%2B4RzU14eLieeeYZ3XHHHWrfvr3efPNNjRo1SpK0f/9%2BxcbG6oMPPtDNN998Ts9XVVWlsLAwVVZWKjQ01OjWxKxVRp%2BvKRXNusXuCQAAP9KUf/%2Bejd%2Bfwfq%2Buro65efn68iRI0pKSlJxcbGOHz%2Bu5ORk79dER0crPj5ehYWFZ3wej8ejqqoqnxsAAPAfBJakbdu26aKLLpLb7dakSZO0fPlyXXnllSovL1dQUJDatm3r8/WRkZEqLy8/4/Pl5uYqLCzMe4uNjW3qlwAAAJoRAkvSFVdcoZKSEm3cuFEPPPCAUlNT9cUXX5zx6y3LksvlOuPjM2bMUGVlpfdWWlraFLMBAEAzFWj3gOYgKChIl19%2BuSQpMTFRmzdv1gsvvKBRo0aptrZWhw8f9jmLVVFRoX79%2Bp3x%2Bdxut9xud5PvBgAAzRNnsE7Dsix5PB4lJCSoVatWKigo8D5WVlam7du3/2BgAQAA/%2Bb3Z7BmzpyplJQUxcbGqrq6Wvn5%2Bfr000%2B1atUqhYWFaeLEiZoyZYoiIiIUHh6uqVOnqmfPnho8eLDd0wEAQDPl94F14MABjRs3TmVlZQoLC1OvXr20atUq3XTTTZKkefPmKTAwUCNHjtTRo0c1aNAgLV68WAEBATYvBwAAzRWfg3Ue8DlYJ/E5WACA84nPwQIAAGhBCCwAAADDCCwAAADDCCwAAADDCCwAAADDCCwAAADDCCwAAADDHBtYS5Ys0bFjx%2ByeAQAA0IBjAyszM1NRUVG6//77tWnTJrvnAAAAeDk2sPbv36/XX39dZWVl6t%2B/v3r06KG5c%2Bfq4MGDdk8DAAB%2BzrGBFRgYqBEjRmjlypXau3evUlNT9frrrysmJkYjRozQn//8Z/FbgAAAgB0cG1jfFxUVpUGDBmnAgAFyuVwqKirS6NGj1aVLF61fv97ueQAAwM84OrAOHTqk559/Xr1799a1116riooKrVixQnv27NG3336roUOH6u6777Z7JgAA8DOBdg/4sX7%2B85/rgw8%2BUFxcnO69916lpqaqffv23scvuugiPfbYY3rxxRdtXAkAAPyRYwMrNDRUa9as0XXXXXfGr%2BnYsaN27dp1HlcBAAA4OLDeeOONs36Ny%2BVS586dz8MaAACA/3DsNViPPPKI5s%2Bf3%2BD4Sy%2B9pClTptiwCAAA4CTHBtbbb7%2Bta665psHxpKQkLVu2zIZFAAAAJzk2sA4dOqS2bds2OB4aGqpDhw7ZsAgAAOAkxwZW586d9dFHHzU4/tFHHykuLs6GRQAAACc59iL3jIwMZWRk6J///KcGDhwoSfr44481Z84cPfvsszavAwAA/syxgXXffffp2LFjysnJ0RNPPCFJiomJ0YsvvqgJEybYvA4AAPgzxwaWJKWnpys9PV1lZWVq06aNLr74YrsnAQAAODuwTunYsaPdEwAAALwce5H7wYMHdc8996hTp05q3bq1goKCfG4AAAB2cewZrPHjx%2Bvvf/%2B7Hn30UXXs2FEul8vuSQAAAJIcHFjr1q3TunXr1KdPH7unAAAA%2BHDsW4QxMTGctQIAAM2SYwNr3rx5mjFjhvbt22f3FAAAAB%2BOfYtw3Lhxqq6u1qWXXqrQ0FC1atXK5/GKigqblgEAAH/n2MCaPXu23RMAAABOy7GBNXHiRLsnAAAAnJZjr8GSpH/84x/Kzs7WuHHjvG8Jrl69Wl9%2B%2BaXNywAAgD9zbGCtX79ePXr00Nq1a/XWW2%2BppqZGkvTZZ5/p8ccft3kdAADwZ44NrGnTpik7O1uffPKJzye3Dxw4UBs3brRxGQAA8HeODaytW7fqjjvuaHC8Q4cOOnjwoA2LAAAATnJsYF188cUqLy9vcLykpESXXHKJDYsAAABOcmxg3XXXXZo%2BfboOHjzo/UT3v/3tb5o6darGjh1r8zoAAODPHBtYOTk5ioqKUseOHVVTU6Mrr7xS/fr1U9%2B%2BffXrX//a7nkAAMCPOfZzsIKCgrRs2TJ99dVX%2Buyzz1RfX6%2BrrrpK3bp1s3saAADwc44NrFO6du2qrl272j0DAADAy7GB9ctf/vIHH8/LyztPSwAAAHw5NrDKysp87h8/flw7duxQdXW1rr/%2BeptWAQAAODiw3n///QbHTpw4oQceeEDdu3e3YREAAMBJjv0pwtMJDAzU1KlT9cwzz9g9BQAA%2BLEWFViStHv3bh0/ftzuGQAAwI859i3Cxx57zOe%2BZVkqKyvTypUrNWbMGJtWAQAAODiw/vrXv/rcv%2BCCC9S%2BfXvNnj1b9913n02rAAAAHBxY69evt3sCAADAabW4a7AAAADs5tgzWH379vX%2Bkuez2bRpUxOvAQAA%2BA/HBtaNN96oV155RV27dlVSUpIkaePGjdq5c6fuv/9%2Bud1umxcCAAB/5djA%2Bu6775SWlqacnByf41lZWTpw4IBeffVVm5YBAAB/59hrsN566y3dc889DY6PHz9eb7/9tg2LAAAATnJsYLndbhUWFjY4XlhYyNuDAADAVo59i/Dhhx/WpEmTtGXLFl1zzTWSTl6DtXDhQs2cOdPmdQAAwJ85NrCysrIUFxenF154Qa%2B//rokqXv37lq4cKFGjx5t8zoAAODPHBtYkjR69GhiCgAANDuOvQZLkqqqqrR48WI9/vjjOnz4sCTp888/V1lZ2Tk/R25urvr27auQkBB16NBBt99%2Bu3bu3OnzNR6PR%2Bnp6WrXrp2Cg4M1fPhw7du3z%2BhrAQAALYdjA2v79u3q2rWrnnrqKeXm5noD66233tL06dPP%2BXnWrl2rtLQ0bdy4UQUFBTpx4oSSk5N15MgR79dkZGRo%2BfLlys/P14YNG1RTU6OhQ4eqrq7O%2BOsCAADO59i3CB955BGNHj1ac%2BfOVWhoqPf4kCFDNGbMmHN%2BnlWrVvncX7RokTp06KDi4mJdf/31qqys1GuvvaY333xTgwcPliQtWbJEsbGxWrNmjW6%2B%2BWYzLwgAALQYjj2DtXnzZj344IMNfl3OJZdc8j%2B9RfjfKisrJUnh4eGSpOLiYh0/flzJycner4mOjlZ8fPxpPyZCOvmWYlVVlc8NAAD4D8cGVlBQkGpqahoc37Vrl9q1a/ejntOyLGVmZqp///6Kj4%2BXJJWXlysoKEht27b1%2BdrIyEiVl5ef9nlyc3MVFhbmvcXGxv6oPQAAwJkcG1jDhw/Xb37zG504cUKS5HK59O2332r69OkaMWLEj3rOhx56SFu3btWf/vSns36tZVln/GXTM2bMUGVlpfdWWlr6o/YAAABncmxgzZ07V/v371dUVJSOHj2qgQMH6rLLLlPr1q0b/H7Cc5Genq6VK1fqk08%2BUUxMjPd4VFSUamtrvRfRn1JRUaHIyMjTPpfb7VZoaKjPDQAA%2BA/HXuQeFhamwsJCFRQU6LPPPlN9fb2uuuoq3XzzzWc8s3Q6lmUpPT1dy5cv16effqq4uDifxxMSEtSqVSsVFBRo5MiRkqSysjJt375dc%2BbMMfqaAABAy%2BDIwDp%2B/LhuvfVWvfzyy0pOTva5AP1/lZaWpqVLl%2Bq9995TSEiI97qqsLAwtWnTRmFhYZo4caKmTJmiiIgIhYeHa%2BrUqerZs6f3pwoBAAC%2Bz5GB1apVK23ZsuV/OlN1JgsWLJAkDRgwwOf4okWLNH78eEnSvHnzFBgYqJEjR%2Bro0aMaNGiQFi9erICAgEb/8wEAQMvjsizLsnvEj5GRkaHg4GDNmjXL7ilnVVVVpbCwMFVWVhq/Hisxa9XZv6iZKJp1i90TAAB%2BpCn//j0bR57BOmX%2B/Plas2aNEhMTFRwc7PMY10cBAAC7ODawiouL1atXL0nS1q1bfR4z8dYhAADAj%2BW4wNq9e7fi4uK0fv16u6cAAACcluM%2BB6tLly46ePCg9/6oUaN04MABGxcBAAD4clxg/fc1%2BR988IGOHDli0xoAAICGHBdYAAAAzZ3jAsvlcjW4iJ2L2gEAQHPiuIvcLcvS%2BPHj5Xa7JUnHjh3TpEmTGnxMw7vvvmvHPAAAAOcFVmpqqs/9sWPH2rQEAADg9BwXWIsWLbJ7AgAAwA9y3DVYAAAAzR2BBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYBiBBQAAYFig3QMAoLlLzFpl94T/SdGsW%2ByeAPg9zmABAAAYRmABAAAYRmABAAAYRmABAAAYRmABAAAYRmABAAAYxsc0AC2Ekz5KgI8RANDScQYLAADAML8PrHXr1mnYsGGKjo6Wy%2BXSihUrfB63LEvZ2dmKjo5WmzZtNGDAAO3YscOmtQAAwAn8PrCOHDmi3r17a/78%2Bad9fM6cOXruuec0f/58bd68WVFRUbrppptUXV19npcCAACn8PtrsFJSUpSSknLaxyzL0vPPP6%2BsrCyNGDFCkvTGG28oMjJSS5cu1f33338%2BpwIAAIfw%2BzNYP%2BSbb75ReXm5kpOTvcfcbrduuOEGFRYW2rgMAAA0Z35/BuuHlJeXS5IiIyN9jkdGRmrPnj1n/D6PxyOPx%2BO9X1VV1TQDAQBAs0RgnQOXy%2BVz37KsBse%2BLzc3V08%2B%2BWRTz0ITc9LHHgAAmhfeIvwBUVFRkv5zJuuUioqKBme1vm/GjBmqrKz03kpLS5t0JwAAaF4IrB8QFxenqKgoFRQUeI/V1tZq7dq16tev3xm/z%2B12KzQ01OcGAAD8h9%2B/RVhTU6Ovv/7ae/%2Bbb75RSUmJwsPD1alTJ2VkZCgnJ0ddunRRly5dlJOTowsvvFCjR4%2B2cTUAAGjO/D6wioqKdOONN3rvZ2ZmSpJSU1O1ePFiPfbYYzp69KgefPBBHT58WFdffbVWr16tkJAQuyYDAIBmzu8Da8CAAbIs64yPu1wuZWdnKzs7%2B/yNAgAAjsY1WAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYF2j0A/iMxa5XdEwAAOC84gwUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGAYgQUAAGBYoN0DAABmJWatsnvCOSuadYvdE4AmwRksAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwgsAAAAwwLtHgAAgFMkZq2ye0KLVTTrFrsnGMUZLAAAAMMILAAAAMMIrHP08ssvKy4uTq1bt1ZCQoLWr19v9yQAANBMEVjnYNmyZcrIyFBWVpa2bNmi6667TikpKdq7d6/d0wAAQDNEYJ2D5557ThMnTtS9996r7t276/nnn1dsbKwWLFhg9zQAANAM8VOEZ1FbW6vi4mJNnz7d53hycrIKCwtP%2Bz0ej0cej8d7v7KyUpJUVVVlfF%2Bd54jx5wSaWlP8t9CU%2BO%2Bs6fBnAac0xZ%2BFU89pWZbx5z4bAussDh06pLq6OkVGRvocj4yMVHl5%2BWm/Jzc3V08%2B%2BWSD47GxsU2yEXCasLl2L0BzwZ8FnNKUfxaqq6sVFhbWdP%2BA0yCwzpHL5fK5b1lWg2OnzJgxQ5mZmd779fX1%2Bte//qWIiIgzfs%2BPUVVVpdjYWJWWlio0NNTY89qtpb4uqeW%2Btpb6uiRemxO11NcltdzX1lSvy7IsVVdXKzo62thznisC6yzatWungICABmerKioqGpzVOsXtdsvtdvscu/jii5tsY2hoaIv6D%2B2Ulvq6pJb72lrq65J4bU7UUl%2BX1HJfW1O8rvN95uoULnI/i6CgICUkJKigoMDneEFBgfr162fTKgAA0JxxBuscZGZmaty4cUpMTFRSUpLy8vK0d%2B9eTZo0ye5pAACgGQrIzs7OtntEcxcfH6%2BIiAjl5OTo2Wef1dGjR/Xmm2%2Bqd%2B/edk9TQECABgwYoMDAltXKLfV1SS33tbXU1yXx2pyopb4uqeW%2Btpb2ulyWHT%2B7CAAA0IJxDRYAAIBhBBYAAIBhBBYAAIBhBBYAAIBhBJZDvfzyy4qLi1Pr1q2VkJCg9evX2z2p0datW6dhw4YpOjpaLpdLK1assHuSMbm5uerbt69CQkLUoUMH3X777dq5c6fdsxptwYIF6tWrl/fDAZOSkvThhx/aPcu43NxcuVwuZWRk2D2l0bKzs%2BVyuXxuUVFRds8y5ttvv9XYsWMVERGhCy%2B8UD/96U9VXFxs96xG%2BclPftLg35nL5VJaWprd0xrtxIkT%2BtWvfqW4uDi1adNGl112mZ566inV19fbPa3RCCwHWrZsmTIyMpSVlaUtW7bouuuuU0pKivbu3Wv3tEY5cuSIevfurfnz59s9xbi1a9cqLS1NGzduVEFBgU6cOKHk5GQdOeLsXxwbExOj2bNnq6ioSEVFRRo4cKBuu%2B027dixw%2B5pxmzevFl5eXnq1auX3VOM6dGjh8rKyry3bdu22T3JiMOHD%2Bvaa69Vq1at9OGHH%2BqLL77Q3Llzm/Q3aZwPmzdv9vn3deqDr%2B%2B8806blzXeb3/7W/3%2B97/X/Pnz9eWXX2rOnDl65pln9Lvf/c7uaY1nwXF%2B9rOfWZMmTfI51q1bN2v69Ok2LTJPkrV8%2BXK7ZzSZiooKS5K1du1au6cY17ZtW%2BvVV1%2B1e4YR1dXVVpcuXayCggLrhhtusCZPnmz3pEZ74oknrN69e9s9o0lMmzbN6t%2B/v90zmtzkyZOtzp07W/X19XZPabQhQ4ZYEyZM8Dk2YsQIa%2BzYsTYtMoczWA5TW1ur4uJiJScn%2BxxPTk5WYWGhTavwv6qsrJQkhYeH27zEnLq6OuXn5%2BvIkSNKSkqye44RaWlpGjJkiAYPHmz3FKN27dql6OhoxcXF6a677tLu3bvtnmTEypUrlZiYqDvvvFMdOnRQnz59tHDhQrtnGVVbW6slS5ZowoQJcrlcds9ptP79%2B%2Bvjjz/WV199JUn6/PPPtWHDBt166602L2u8lvFxqX7k0KFDqqura/CLpiMjIxv8Qmo0T5ZlKTMzU/3791d8fLzdcxpt27ZtSkpK0rFjx3TRRRdp%2BfLluvLKK%2B2e1Wj5%2BfkqLi5WUVGR3VOMuvrqq/WHP/xBXbt21YEDB/T000%2BrX79%2B2rFjhyIiIuye1yi7d%2B/WggULlJmZqZkzZ2rTpk16%2BOGH5Xa7dffdd9s9z4gVK1bou%2B%2B%2B0/jx4%2B2eYsS0adNUWVmpbt26KSAgQHV1dZo1a5Z%2B8Ytf2D2t0Qgsh/rv/%2BdiWVaL%2BH8z/uChhx7S1q1btWHDBrunGHHFFVeopKRE3333nd555x2lpqZq7dq1jo6s0tJSTZ48WatXr1br1q3tnmNUSkqK93/37NlTSUlJ6ty5s9544w1lZmbauKzx6uvrlZiYqJycHElSnz59tGPHDi1YsKDFBNZrr72mlJQURUdH2z3FiGXLlmnJkiVaunSpevTooZKSEmVkZCg6Olqpqal2z2sUAsth2rVrp4CAgAZnqyoqKhqc1ULzk56erpUrV2rdunWKiYmxe44RQUFBuvzyyyVJiYmJ2rx5s1544QW98sorNi/78YqLi1VRUaGEhATvsbq6Oq1bt07z58%2BXx%2BNRQECAjQvNCQ4OVs%2BePbVr1y67pzRax44dG4R99%2B7d9c4779i0yKw9e/ZozZo1evfdd%2B2eYsyjjz6q6dOn66677pJ0Mvr37Nmj3NxcxwcW12A5TFBQkBISErw/RXJKQUGB%2BvXrZ9MqnI1lWXrooYf07rvv6i9/%2BYvi4uLsntRkLMuSx%2BOxe0ajDBo0SNu2bVNJSYn3lpiYqDFjxqikpKTFxJUkeTweffnll%2BrYsaPdUxrt2muvbfDxJ1999ZUuvfRSmxaZtWjRInXo0EFDhgyxe4ox//73v3XBBb4pEhAQ0CI%2BpoEzWA6UmZmpcePGKTExUUlJScrLy9PevXs1adIku6c1Sk1Njb7%2B%2Bmvv/W%2B%2B%2BUYlJSUKDw9Xp06dbFzWeGlpaVq6dKnee%2B89hYSEeM9AhoWFqU2bNjav%2B/FmzpyplJQUxcbGqrq6Wvn5%2Bfr000%2B1atUqu6c1SkhISIPr44KDgxUREeH46%2BamTp2qYcOGqVOnTqqoqNDTTz%2Btqqoqx58tkKRHHnlE/fr1U05OjkaOHKlNmzYpLy9PeXl5dk9rtPr6ei1atEipqakKDGw5f3UPGzZMs2bNUqdOndSjRw9t2bJFzz33nCZMmGD3tMaz94cY8WO99NJL1qWXXmoFBQVZV111VYv4cf8U2jByAAABEUlEQVRPPvnEktTglpqaave0Rjvd65JkLVq0yO5pjTJhwgTvn8P27dtbgwYNslavXm33rCbRUj6mYdSoUVbHjh2tVq1aWdHR0daIESOsHTt22D3LmPfff9%2BKj4%2B33G631a1bNysvL8/uSUZ89NFHliRr586ddk8xqqqqypo8ebLVqVMnq3Xr1tZll11mZWVlWR6Px%2B5pjeayLMuyJ%2B0AAABaJq7BAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMIzAAgAAMOz/APqO9oyNCexUAAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common6867184384696783866">    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">44</td>
        <td class="number">37.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">12</td>
        <td class="number">10.3%</td>
        <td>
            <div class="bar" style="width:27%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:25%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">10</td>
        <td class="number">8.5%</td>
        <td>
            <div class="bar" style="width:23%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1.0</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">8.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">28</td>
        <td class="number">23.9%</td>
        <td>
            <div class="bar" style="width:64%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme6867184384696783866">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">44</td>
        <td class="number">37.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1.0</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:16%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">10</td>
        <td class="number">8.5%</td>
        <td>
            <div class="bar" style="width:23%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:25%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">12</td>
        <td class="number">10.3%</td>
        <td>
            <div class="bar" style="width:27%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:91%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">12</td>
        <td class="number">10.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:25%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:9%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">8.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:9%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Date">Date<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>6</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>5.1%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>2.6%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>3</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable5227636804076289045">
    <table class="mini freq">
        <tr class="">
    <th>5/28/2008</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 39.3%">
            46
        </div>
        
    </td>
</tr><tr class="">
    <th>5/27/2008</th>
    <td>
        <div class="bar" style="width:76%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 29.9%">
            35
        </div>
        
    </td>
</tr><tr class="">
    <th>5/8/2008</th>
    <td>
        <div class="bar" style="width:31%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 12.0%">
            14
        </div>
        
    </td>
</tr><tr class="other">
    <th>Other values (2)</th>
    <td>
        <div class="bar" style="width:41%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 16.2%">
            19
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable5227636804076289045, #minifreqtable5227636804076289045"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable5227636804076289045">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">5/28/2008</td>
        <td class="number">46</td>
        <td class="number">39.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5/27/2008</td>
        <td class="number">35</td>
        <td class="number">29.9%</td>
        <td>
            <div class="bar" style="width:76%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5/8/2008</td>
        <td class="number">14</td>
        <td class="number">12.0%</td>
        <td>
            <div class="bar" style="width:31%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5/19/2008</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:24%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5/5/2008</td>
        <td class="number">8</td>
        <td class="number">6.8%</td>
        <td>
            <div class="bar" style="width:18%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Femur left">Femur left<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>58</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>49.6%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>21.4%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>25</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>42.504</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>54.4</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>6.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram507595831772605039">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAbxJREFUeJzt3DGO01AUhtHriNZewMjeBT0te6JDYk9IbIFd2MoCHFFQEFNApmH0SwPD83jmnDJRfF8Uf7JeZLnbtm2rxpZlqWmaWo/l4OZ5rnEcm85803Tab33fV9WvLzwMwx5L4EDWda1pmu7Pm5Z2CaTruqqqGoZBIAfx9sPnR3/m66f3T7qG23nT0qn5RDgQgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAsEuD21gX3/zAIbXyhUEAoFAIBAIBAKBTTr/zXN4GuO/cgWBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUDw7G5WfAk3uPFyuIJAIBAIBAKBQCAQCAQCgWCXv3m3bauqqnVd/3jvx/dvjz7eQ8d5Td59/LL3Ep7MQ7/l7bXbedNSt%2B0wdVmWmqap9VgObp7nGsex6cxdArler3U%2Bn6vv%2B%2Bq6rvV4DmbbtrpcLnV3d1enU9tdwS6BwFHYpEMgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQ/ASMHE9XW/gmuwAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives507595831772605039,#minihistogram507595831772605039"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives507595831772605039">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles507595831772605039"
                                                  aria-controls="quantiles507595831772605039" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram507595831772605039" aria-controls="histogram507595831772605039"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common507595831772605039" aria-controls="common507595831772605039"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme507595831772605039" aria-controls="extreme507595831772605039"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles507595831772605039">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>43</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>45.1</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>48.325</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>51.67</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>54.4</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>54.4</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>5.325</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>12.719</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.29923</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>7.2054</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>42.504</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>6.8649</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-2.8512</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>3910.4</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>161.76</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram507595831772605039">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3X90VOWB//HPmB8DxMxICGSSZsrJKviDAEcSVoK/ImA0imipRyjIYkvZYiE1DdQaOFvjbiUcWlQUzdbfaPGE7qlQPWI0aEngsLRJAAnBw1JBCZIQ6EImycIQ4H7/8Muss0kgwJPcGeb9Oueew33uncsnj5p8fO7NjMOyLEsAAAAw5gq7AwAAAFxuKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYFi03QEiwZkzZ3Tw4EHFx8fL4XDYHQcAgIhgWZZaWlqUkpKiK67o3TUlClYvOHjwoLxer90xAACISPX19UpNTe3Vv5OC1Qvi4%2BMlffMP2OVy2ZwGAIDI4PP55PV6Az%2BHexMFqxecvS3ocrkoWAAA9DI7Hs/hIXcAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGMaHPQMA0E2Zi8rsjnBBqp%2B%2B2%2B4IEYsVLAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFjfUlxcLIfDofz8/MCY3%2B9XXl6eEhMTFRcXp0mTJunAgQM2pgQAAKGOgvX/VVVV6eWXX9aIESOCxvPz87VmzRqVlpZq06ZNam1t1cSJE3X69GmbkgIAgFBHwZLU2tqq6dOn65VXXlH//v0D483NzXrttde0bNkyTZgwQTfeeKN%2B//vfq7a2VuvXr7cxMQAACGUULElz587VvffeqwkTJgSN19TUqL29XTk5OYGxlJQUpaena/Pmzb0dEwAAhIlouwPYrbS0VDU1Naquru5wrLGxUbGxsUGrWpKUlJSkxsbGLq/p9/vl9/sD%2Bz6fz1xgAAAQ8iJ6Bau%2Bvl6PPfaYVq1apT59%2BnT7dZZlyeFwdHm8uLhYbrc7sHm9XhNxAQBAmIjoglVTU6OmpiZlZGQoOjpa0dHRqqio0PPPP6/o6GglJSXp5MmTOnr0aNDrmpqalJSU1OV1CwsL1dzcHNjq6%2Bt7%2BksBAAAhJKJvEY4fP161tbVBYz/84Q913XXX6Ze//KW8Xq9iYmJUXl6uhx56SJLU0NCgnTt3aunSpV1e1%2Bl0yul09mh2AAAQuiK6YMXHxys9PT1oLC4uTgMGDAiMz5o1S/Pnz9eAAQOUkJCgBQsWaPjw4R0eiAcAADgrogtWdzz77LOKjo7WQw89pOPHj2v8%2BPF68803FRUVZXc0AAAQohyWZVl2h7jc%2BXw%2Bud1uNTc3y%2BVy2R0HAHCRMheV2R3hglQ/fbfdEWxl58/fiH7IHQAAoCdQsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyL%2BIJVUlKiESNGyOVyyeVyKSsrSx9%2B%2BGHgeHZ2thwOR9A2depUGxMDAIBQF213ALulpqZqyZIluuaaayRJK1eu1P33369t27Zp2LBhkqTZs2frX//1XwOv6du3ry1ZAQBAeIj4gnXfffcF7T/99NMqKSnRli1bAgWrX79%2B8ng8dsQDAABhKOJvEX7b6dOnVVpaqra2NmVlZQXGV61apcTERA0bNkwLFixQS0vLOa/j9/vl8/mCNgAAEDkifgVLkmpra5WVlaUTJ07oyiuv1Jo1a3TDDTdIkqZPn660tDR5PB7t3LlThYWF%2Buyzz1ReXt7l9YqLi/XUU0/1VnwAABBiHJZlWXaHsNvJkye1f/9%2BHTt2TH/84x/16quvqqKiIlCyvq2mpkaZmZmqqanRqFGjOr2e3%2B%2BX3%2B8P7Pt8Pnm9XjU3N8vlcvXY1wEA6FmZi8rsjnBBqp%2B%2B2%2B4ItvL5fHK73bb8/GUFS1JsbGzgIffMzExVVVVp%2BfLl%2Bt3vftfh3FGjRikmJkZ79uzpsmA5nU45nc4ezQwAAEIXz2B1wrKsoBWob6urq1N7e7uSk5N7ORUAAAgXEb%2BCtXDhQuXm5srr9aqlpUWlpaXasGGDysrK9MUXX2jVqlW65557lJiYqF27dmn%2B/Pm68cYbdfPNN9sdHQAAhKiIL1iHDh3SjBkz1NDQILfbrREjRqisrEx33nmn6uvr9cknn2j58uVqbW2V1%2BvVvffeqyeffFJRUVF2RwcAACEq4gvWa6%2B91uUxr9erioqKXkwDAAAuBzyDBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYFjEF6ySkhKNGDFCLpdLLpdLWVlZ%2BvDDDwPH/X6/8vLylJiYqLi4OE2aNEkHDhywMTEAAAh1EV%2BwUlNTtWTJElVXV6u6ulrjxo3T/fffr7q6OklSfn6%2B1qxZo9LSUm3atEmtra2aOHGiTp8%2BbXNyAAAQqhyWZVl2hwg1CQkJ%2Bs1vfqMHH3xQAwcO1Ntvv60pU6ZIkg4ePCiv16t169bprrvu6tb1fD6f3G63mpub5XK5ejI6AKAHZS4qszvCBal%2B%2Bm67I9jKzp%2B/Eb%2BC9W2nT59WaWmp2tralJWVpZqaGrW3tysnJydwTkpKitLT07V58%2BYur%2BP3%2B%2BXz%2BYI2AAAQOShYkmpra3XllVfK6XRqzpw5WrNmjW644QY1NjYqNjZW/fv3Dzo/KSlJjY2NXV6vuLhYbrc7sHm93p7%2BEgAAQAihYEm69tprtX37dm3ZskWPPvqoZs6cqV27dnV5vmVZcjgcXR4vLCxUc3NzYKuvr%2B%2BJ2AAAIERF2x0gFMTGxuqaa66RJGVmZqqqqkrLly/XlClTdPLkSR09ejRoFaupqUljx47t8npOp1NOp7PHcwMAgNDEClYnLMuS3%2B9XRkaGYmJiVF5eHjjW0NCgnTt3nrNgAQCAyBbxK1gLFy5Ubm6uvF6vWlpaVFpaqg0bNqisrExut1uzZs3S/PnzNWDAACUkJGjBggUaPny4JkyYYHd0AAAQoiK%2BYB06dEgzZsxQQ0OD3G63RowYobKyMt15552SpGeffVbR0dF66KGHdPz4cY0fP15vvvmmoqKibE4OAABCFe%2BD1Qt4HywAuDzwPljhhffBAgAAuIxQsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhoVtwfr973%2BvEydO2B0DAACgg7AtWAUFBfJ4PPrJT36iv/71r3bHAQAACAjbgnXw4EG9/vrramho0C233KJhw4Zp2bJlOnz4sN3RAABAhAvbghUdHa3Jkyfrvffe0/79%2BzVz5ky9/vrrSk1N1eTJk/XBBx/Isiy7YwIAgAgUtgXr2zwej8aPH6/s7Gw5HA5VV1dr2rRpGjJkiDZu3Gh3PAAAEGHCumAdOXJEzz33nEaOHKmbb75ZTU1NWrt2rb766it9/fXXmjhxov7pn/7J7pgAACDCRNsd4GJ973vf07p165SWlqYf//jHmjlzpgYOHBg4fuWVV%2Brxxx/X888/b2NKAAAQicK2YLlcLq1fv1633nprl%2BckJydrz549vZgKAAAgjG8Rrly58pzlSpIcDoeuvvrqc55TXFys0aNHKz4%2BXoMGDdIDDzyg3bt3B51z9tmub29Tp0695K8BAABcnsK2YP385z/XihUrOoy/%2BOKLmj9/frevU1FRoblz52rLli0qLy/XqVOnlJOTo7a2tqDzZs%2BerYaGhsD2u9/97pK/BgAAcHkK21uE//Ef/6G1a9d2GM/KylJxcbGWLVvWreuUlZUF7b/xxhsaNGiQampqdNtttwXG%2B/XrJ4/Hc2mhAQBARAjbFawjR46of//%2BHcZdLpeOHDly0ddtbm6WJCUkJASNr1q1SomJiRo2bJgWLFiglpaWi/47AADA5S1sV7CuvvpqffTRR/rpT38aNP7RRx8pLS3toq5pWZYKCgp0yy23KD09PTA%2Bffp0paWlyePxaOfOnSosLNRnn32m8vLyTq/j9/vl9/sD%2Bz6f76LyAACA8BS2BSs/P1/5%2Bfn6%2B9//rnHjxkmSPvnkEy1dulS//e1vL%2Bqa8%2BbN044dO7Rp06ag8dmzZwf%2BnJ6eriFDhigzM1Nbt27VqFGjOlynuLhYTz311EVlAAAA4c9hhfHnybzwwgtavHixDh06JElKTU1VUVGRfvSjH13wtfLy8rR27VpVVlaedwXMsiw5nU69/fbbmjJlSofjna1geb1eNTc3y%2BVyXXA2AEBoyFxUdv6TQkj103fbHcFWPp9Pbrfblp%2B/YbuCJX1TivLy8tTQ0KC%2BffvqqquuuuBrWJalvLw8rVmzRhs2bOjW7cW6ujq1t7crOTm50%2BNOp1NOp/OCswAAgMtDWBess7oqOt0xd%2B5cvfPOO/rTn/6k%2BPh4NTY2SpLcbrf69u2rL774QqtWrdI999yjxMRE7dq1S/Pnz9eNN96om2%2B%2B2dSXAAAALiNh%2B1uEhw8f1g9/%2BEN997vfVZ8%2BfRQbGxu0dVdJSYmam5uVnZ2t5OTkwLZ69WpJUmxsrD755BPddddduvbaa/Wzn/1MOTk5Wr9%2BvaKionrqywMAAGEsbFewHnnkEX3xxRf6xS9%2BoeTkZDkcjou6zvkeQfN6vaqoqLioawMAgMgUtgWrsrJSlZWVuvHGG%2B2OAgAAECRsbxGmpqZe9KoVAABATwrbgvXss8%2BqsLBQBw4csDsKAABAkLC9RThjxgy1tLRo8ODBcrlciomJCTre1NRkUzIAABDpwrZgLVmyxO4IAAAAnQrbgjVr1iy7IwAAAHQqbJ/BkqQvv/xSRUVFmjFjRuCW4Mcff6zPP//c5mQAACCShW3B2rhxo4YNG6aKigr94Q9/UGtrqyRp69at%2BtWvfmVzOgAAEMnC9hbhL3/5SxUVFekXv/iF4uPjA%2BPjxo3Tiy%2B%2BaGMyAEB3hduHJwPdFbYrWDt27NCDDz7YYXzQoEE6fPiwDYkAAAC%2BEbYF66qrrgp8MPO3bd%2B%2BXd/5zndsSAQAAPCNsC1YU6dO1RNPPKHDhw8H3tH9L3/5ixYsWKCHH37Y5nQAACCShW3BWrx4sTwej5KTk9Xa2qobbrhBY8eO1ejRo/Uv//IvdscDAAARLGwfco%2BNjdXq1av1X//1X9q6davOnDmjUaNG6brrrrM7GgAAiHBhW7DOGjp0qIYOHWp3DAAAgICwLVj//M//fM7jL7/8ci8lAQAACBa2BauhoSFov729XXV1dWppadFtt91mUyoAAIAwLljvv/9%2Bh7FTp07p0Ucf1fXXX29DIgAAgG%2BE7W8RdiY6OloLFizQb37zG7ujAACACHZZFSxJ2rt3r9rb2%2B2OAQAAIljY3iJ8/PHHg/Yty1JDQ4Pee%2B89TZ8%2B3aZUAAAAYVyw/vM//zNo/4orrtDAgQO1ZMkSzZ4926ZUAAAAYVywNm7caHcEAACATl12z2ABAADYLWxXsEaPHh34kOfz%2Betf/9rDaQAAAP5X2K5g3XHHHdq9e7csy9KYMWM0ZswYSdLu3buVnZ2tu%2B66K7CdS3FxsUaPHq34%2BHgNGjRIDzzwgHbv3h10jt/vV15enhITExUXF6dJkybpwIEDPfa1AQCA8Ba2K1jHjh3T3LlztXjx4qDxRYsW6dChQ3r11Ve7dZ2KigrNnTtXo0eP1qlTp7Ro0SLl5ORo165diouLkyTl5%2Bfr/fffV2lpqQYMGKD58%2Bdr4sSJqqmpUVRUlPGvDQAAhDeHZVmW3SEuxlVXXaWqqioNGTIkaHzPnj3KzMxUc3PzRV338OHDGjRokCoqKnTbbbepublZAwcO1Ntvv60pU6ZIkg4ePCiv16t169add4VMknw%2Bn9xut5qbm%2BVyuS4qFwBcjjIXldkd4bJW/fTddkewlZ0/f8P2FqHT6dTmzZs7jG/evFlOp/Oir3u2mCUkJEiSampq1N7erpycnMA5KSkpSk9P7/TvBwAACNtbhD/72c80Z84cbdu2LfD81ZYtW/TKK69o4cKFF3VNy7JUUFCgW265Renp6ZKkxsZGxcbGqn///kHnJiUlqbGxsdPr%2BP1%2B%2Bf3%2BwL7P57uoPAAAIDyFbcFatGiR0tLStHz5cr3%2B%2BuuSpOuvv16vvPKKpk2bdlHXnDdvnnbs2KFNmzad91zLsrr8Lcbi4mI99dRTF5UBAACEv7AtWJI0bdq0iy5T/1deXp7ee%2B89VVZWKjU1NTDu8Xh08uRJHT16NGgVq6mpSWPHju30WoWFhSooKAjs%2B3w%2Beb1eIzkBAEDoC9tnsKRvisubb76pX/3qVzp69Kgk6bPPPlNDQ0O3r2FZlubNm6d3331Xn376qdLS0oKOZ2RkKCYmRuXl5YGxhoYG7dy5s8uC5XQ65XK5gjYAABA5wnYFa%2BfOnZowYYL69eun%2Bvp6PfLII%2Brfv7/%2B8Ic/6MCBA1q5cmW3rjN37ly98847%2BtOf/qT4%2BPjAc1Vut1t9%2B/aV2%2B3WrFmzNH/%2BfA0YMEAJCQlasGCBhg8frgkTJvTklwgAAMJU2K5g/fznP9e0adP0xRdfqE%2BfPoHxe%2B%2B9V5WVld2%2BTklJiZqbm5Wdna3k5OTAtnr16sA5zz77rB544AE99NBDuvnmm9WvXz%2B9//77vAcWAADoVNiuYFVVVamkpKTDg%2Bbf%2Bc53LvgW4fn06dNHL7zwgl544YULzgkAACJP2K5gxcbGqrW1tcP4nj17lJiYaEMiAACAb4TtCtakSZP0b//2b4FbeQ6HQ19//bWeeOIJTZ482eZ0AADYL5zeKf9ye9f5sF3BWrZsmQ4ePCiPx6Pjx49r3Lhx%2Bod/%2BAf16dOnw%2BcTAgAA9KawXcFyu93avHmzysvLtXXrVp05c0ajRo3SXXfd1eUbgAIAAPSGsCxY7e3tuueee/TSSy8pJycn6HMCAQAA7BaWtwhjYmK0bds2VqoAAEBICsuCJUkPP/yw3njjDbtjAAAAdBCWtwjPWrFihdavX6/MzEzFxcUFHVu6dKlNqQAAQKQL24JVU1OjESNGSJJ27NgRdIxbhwAAwE5hV7D27t2rtLQ0bdy40e4oAAAAnQq7Z7CGDBmiw4cPB/anTJmiQ4cO2ZgIAAAgWNgVrP/72YHr1q1TW1ubTWkAAAA6CruCBQAAEOrCrmA5HI4OD7HzUDsAAAglYfeQu2VZeuSRR%2BR0OiVJJ06c0Jw5czq8TcO7775rRzwAAIDwK1gzZ84M2n/44YdtSgIAANC5sCtYvHs7AAAIdWH3DBYAAECoo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADIv4glVZWan77rtPKSkpcjgcWrt2bdDxRx55JPAB02e3MWPG2JQWAACEg4gvWG1tbRo5cqRWrFjR5Tl33323GhoaAtu6det6MSEAAAg3YfdZhKbl5uYqNzf3nOc4nU55PJ5eSgQAAMJdxK9gdceGDRs0aNAgDR06VLNnz1ZTU9M5z/f7/fL5fEEbAACIHBSs88jNzdWqVav06aefatmyZaqqqtK4cePk9/u7fE1xcbHcbndg83q9vZgYAADYLeJvEZ7PlClTAn9OT09XZmamBg8erA8%2B%2BECTJ0/u9DWFhYUqKCgI7Pt8PkoWAAARhIJ1gZKTkzV48GDt2bOny3OcTqecTmcvpgIAAKGEW4QX6O9//7vq6%2BuVnJxsdxQAABCiIn4Fq7W1VX/7298C%2B/v27dP27duVkJCghIQEFRUV6fvf/76Sk5P15ZdfauHChUpMTNT3vvc9G1MDAIBQFvEFq7q6WnfccUdg/%2ByzUzNnzlRJSYlqa2v11ltv6dixY0pOTtYdd9yh1atXKz4%2B3q7IAAAgxEV8wcrOzpZlWV0e/%2Bijj3oxDQAAuBzwDBYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhEV%2BwKisrdd999yklJUUOh0Nr164NOm5ZloqKipSSkqK%2BffsqOztbdXV1NqUFAADhIOILVltbm0aOHKkVK1Z0enzp0qV65plntGLFClVVVcnj8ejOO%2B9US0tLLycFAADhItruAHbLzc1Vbm5up8csy9Jzzz2nRYsWafLkyZKklStXKikpSe%2B8845%2B8pOf9GZUAAAQJiJ%2BBetc9u3bp8bGRuXk5ATGnE6nbr/9dm3evLnL1/n9fvl8vqANAABEDgrWOTQ2NkqSkpKSgsaTkpICxzpTXFwst9sd2Lxeb4/mBAAAoYWC1Q0OhyNo37KsDmPfVlhYqObm5sBWX1/f0xEBAEAIifhnsM7F4/FI%2BmYlKzk5OTDe1NTUYVXr25xOp5xOZ4/nAwAAoYkVrHNIS0uTx%2BNReXl5YOzkyZOqqKjQ2LFjbUwGAABCWcSvYLW2tupvf/tbYH/fvn3avn27EhIS9N3vflf5%2BflavHixhgwZoiFDhmjx4sXq16%2Bfpk2bZmNqAAAQyiK%2BYFVXV%2BuOO%2B4I7BcUFEiSZs6cqTfffFOPP/64jh8/rp/%2B9Kc6evSobrrpJn388ceKj4%2B3KzIAAAhxDsuyLLtDXO58Pp/cbream5vlcrnsjgMAISNzUZndERAiqp%2B%2B2/g17fz5yzNYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBh0XYHwKXJXFRmd4Ruq376brsjAADQK1jBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMgnUeRUVFcjgcQZvH47E7FgAACGF8VE43DBs2TOvXrw/sR0VF2ZgGAACEOgpWN0RHR7NqBQAAuo1bhN2wZ88epaSkKC0tTVOnTtXevXvtjgQAAEIYK1jncdNNN%2Bmtt97S0KFDdejQIf3617/W2LFjVVdXpwEDBnT6Gr/fL7/fH9j3%2BXy9FRcAAIQAVrDOIzc3V9///vc1fPhwTZgwQR988IEkaeXKlV2%2Bpri4WG63O7B5vd7eigsAAEIABesCxcXFafjw4dqzZ0%2BX5xQWFqq5uTmw1dfX92JCAABgN24RXiC/36/PP/9ct956a5fnOJ1OOZ3OXkwFAABCCStY57FgwQJVVFRo3759%2Bstf/qIHH3xQPp9PM2fOtDsaAAAIUaxgnceBAwf0gx/8QEeOHNHAgQM1ZswYbdmyRYMHD7Y7GgAACFEUrPMoLS21OwIAAAgz3CIEAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMi7Y7AADArMxFZXZHACIeK1gAAACGUbAAAAAMo2B100svvaS0tDT16dNHGRkZ2rhxo92RAABAiKJgdcPq1auVn5%2BvRYsWadu2bbr11luVm5ur/fv32x0NAACEIApWNzzzzDOaNWuWfvzjH%2Bv666/Xc889J6/Xq5KSErujAQCAEMRvEZ7HyZMnVVNToyeeeCJoPCcnR5s3b%2B70NX6/X36/P7Df3NwsSfL5fMbznfa3Gb9mT%2BmJrx9AR%2BH0fQE4qyd%2BRpy9pmVZxq99PhSs8zhy5IhOnz6tpKSkoPGkpCQ1NjZ2%2Bpri4mI99dRTHca9Xm%2BPZAwX7mV2JwAAhKqe/BnR0tIit9vdc39BJyhY3eRwOIL2LcvqMHZWYWGhCgoKAvtnzpzRf//3f2vAgAFdvuZi%2BHw%2Beb1e1dfXy%2BVyGbtupGD%2BLg3zd2mYv0vHHF6aSJg/y7LU0tKilJSUXv%2B7KVjnkZiYqKioqA6rVU1NTR1Wtc5yOp1yOp1BY1dddVWPZXS5XJftfxy9gfm7NMzfpWH%2BLh1zeGku9/nr7ZWrs3jI/TxiY2OVkZGh8vLyoPHy8nKNHTvWplQAACCUsYLVDQUFBZoxY4YyMzOVlZWll19%2BWfv379ecOXPsjgYAAEJQVFFRUZHdIUJdenq6BgwYoMWLF%2Bu3v/2tjh8/rrffflsjR460O5qioqKUnZ2t6Gi68sVg/i4N83dpmL9LxxxeGuav5zgsO353EQAA4DLGM1gAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIVpl566SWlpaWpT58%2BysgWRMa6AAAGgElEQVTI0MaNG%2B2OFLIqKyt13333KSUlRQ6HQ2vXrg06blmWioqKlJKSor59%2Byo7O1t1dXU2pQ0txcXFGj16tOLj4zVo0CA98MAD2r17d9A5fr9feXl5SkxMVFxcnCZNmqQDBw7YlDj0lJSUaMSIEYE3c8zKytKHH34YOM78XZji4mI5HA7l5%2BcHxpjDrhUVFcnhcARtHo8ncJzvfz2HghWGVq9erfz8fC1atEjbtm3TrbfeqtzcXO3fv9/uaCGpra1NI0eO1IoVKzo9vnTpUj3zzDNasWKFqqqq5PF4dOedd6qlpaWXk4aeiooKzZ07V1u2bFF5eblOnTqlnJwctbX974cJ5%2Bfna82aNSotLdWmTZvU2tqqiRMn6vTp0zYmDx2pqalasmSJqqurVV1drXHjxun%2B%2B%2B8P/BBj/rqvqqpKL7/8skaMGBE0zhye27Bhw9TQ0BDYamtrA8f4/teDLISdf/zHf7TmzJkTNHbddddZTzzxhE2Jwocka82aNYH9M2fOWB6Px1qyZElg7MSJE5bb7bb%2B/d//3Y6IIa2pqcmSZFVUVFiWZVnHjh2zYmJirNLS0sA5X3/9tXXFFVdYZWVldsUMef3797deffVV5u8CtLS0WEOGDLHKy8ut22%2B/3Xrssccsy%2BLfwfN58sknrZEjR3Z6jO9/PYsVrDBz8uRJ1dTUKCcnJ2g8JydHmzdvtilV%2BNq3b58aGxuD5tPpdOr2229nPjvR3NwsSUpISJAk1dTUqL29PWj%2BUlJSlJ6ezvx14vTp0yotLVVbW5uysrKYvwswd%2B5c3XvvvZowYULQOHN4fnv27FFKSorS0tI0depU7d27VxLf/3oab90aZo4cOaLTp093%2BKDppKSkDh9IjfM7O2edzedXX31lR6SQZVmWCgoKdMsttyg9PV3SN/MXGxur/v37B53Lv4/BamtrlZWVpRMnTujKK6/UmjVrdMMNN2j79u3MXzeUlpaqpqZG1dXVHY7x7%2BC53XTTTXrrrbc0dOhQHTp0SL/%2B9a81duxY1dXV8f2vh1GwwpTD4Qjatyyrwxi6j/k8v3nz5mnHjh3atGnTec9l/oJde%2B212r59u44dO6Y//vGPmjlzpioqKro8n/n7X/X19Xrsscf08ccfq0%2BfPt1%2BHXP4jdzc3MCfhw8frqysLF199dVauXKlxowZI4nvfz2FW4RhJjExUVFRUR3%2Bz6ypqanD/4Xg/M7%2BNg3zeW55eXl677339Oc//1mpqamBcY/Ho5MnT%2Bro0aNB5zN/wWJjY3XNNdcoMzNTxcXFGjlypJYvX878dUNNTY2ampqUkZGh6OhoRUdHq6KiQs8//7yio6OVlJTEHF6AuLg4DR8%2BXHv27OH7Xw%2BjYIWZ2NhYZWRkqLy8PGi8vLxcY8eOtSlV%2BEpLS5PH4wmaz5MnT6qiooL51Df/Jztv3jy9%2B%2B67%2BvTTT5WWlhZ0PCMjQzExMUHz19DQoJ07dzJ/52BZlvx%2BP/PXDePHj1dtba22b98e2DIzMzV9%2BvTAn5nD7vP7/fr888%2BVnJzM97%2BeZtvj9bhopaWlVkxMjPXaa69Zu3btsvLz8624uDjryy%2B/tDtaSGppabG2bdtmbdu2zZJkPfPMM9a2bdusr776yrIsy1qyZInldrutd99916qtrbV%2B8IMfWMnJyZbP57M5uf0effRRy%2B12Wxs2bLAaGhoC2//8z/8EzpkzZ46VmppqrV%2B/3tq6das1btw4a%2BTIkdapU6dsTB46CgsLrcrKSmvfvn3Wjh07rIULF1pXXHGF9fHHH1uWxfxdjG//FqFlMYfnMn/%2BfGvDhg3W3r17rS1btlgTJ0604uPjAz8v%2BP7XcyhYYerFF1%2B0Bg8ebMXGxlqjRo0K/No8Ovrzn/9sSeqwzZw507Ksb35V%2Bcknn7Q8Ho/ldDqt2267zaqtrbU3dIjobN4kWW%2B88UbgnOPHj1vz5s2zEhISrL59%2B1oTJ0609u/fb1/oEPOjH/0o8N/qwIEDrfHjxwfKlWUxfxfj/xYs5rBrU6ZMsZKTk62YmBgrJSXFmjx5slVXVxc4zve/nuOwLMuyZ%2B0MAADg8sQzWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADPt//kgS%2BxwTMoEAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common507595831772605039">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:13%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">47.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">43.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">50.8</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">48.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">43.4</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">45.6</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">44.1</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">47.5</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">43.1</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (47)</td>
        <td class="number">57</td>
        <td class="number">48.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">25</td>
        <td class="number">21.4%</td>
        <td>
            <div class="bar" style="width:44%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme507595831772605039">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">7</td>
        <td class="number">6.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">37.7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:15%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">39.8</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:15%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40.6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:15%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40.7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:15%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">51.4</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">52.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">52.6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">52.8</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">54.4</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Femur right">Femur right<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>60</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>51.3%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>23.1%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>27</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>40.437</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>53.6</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>9.4%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram-5526005910680823130">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAbRJREFUeJzt3EGOk3AYxuGPxi0coIFbeAjv5No7eQhvAekBaFy4sLjQunF8kxnNn2H6PMs25YOWXyi0odu2bavGlmWpaZpaj%2BXg5nmucRybznzXdNovfd9X1c8NHoZhj1XgQNZ1rWmafu83Le0SSNd1VVU1DINADuL9x8/Pfs2XTx/%2B6zrc95uWTs0nwoEIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBDscmdFHsNruBvjv3IEgUAgEAgEAoFAIBAIXMV6QC%2B5uvSoHEEgEAgEr%2B4r1lv4cYm3wxEEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAsMtfTbZtq6qqdV3/eO77t6/PXt5Ty%2BHvXvIet/LUZ3l/7L7ftNRtO0xdlqWmaWo9loOb57nGcWw6c5dAbrdbXS6X6vu%2Buq5rPZ6D2batrtdrnc/nOp3anhXsEggchZN0CAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBIIftHVQxnUqstsAAAAASUVORK5CYII%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives-5526005910680823130,#minihistogram-5526005910680823130"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives-5526005910680823130">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles-5526005910680823130"
                                                  aria-controls="quantiles-5526005910680823130" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram-5526005910680823130" aria-controls="histogram-5526005910680823130"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common-5526005910680823130" aria-controls="common-5526005910680823130"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme-5526005910680823130" aria-controls="extreme-5526005910680823130"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles-5526005910680823130">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>42.425</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>45.4</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>47.875</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>51.465</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>53.6</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>53.6</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>5.45</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>15.483</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.38291</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>3.1035</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>40.437</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>9.9261</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-2.1624</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>3639.3</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>239.74</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram-5526005910680823130">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAH7JJREFUeJzt3XuQlfV9%2BPHPyrILQXblortsIJmt0UTlMhFshHohgBgqMSmT0RS1kJBUDVI3aK3INOK0YRkTr0O01XirNrOaqVg7NcSlUS5DaWSFcknG2mgEZHG11V2gsCA%2Bvz/8eerpYkT9wrPHfb1mzozn%2B5w9fPYr7r59zrNny7IsywIAgGSOynsAAICPG4EFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiZXnPUBP8NZbb8X27dujf//%2BUVZWlvc4ANAjZFkWO3fujLq6ujjqqCN7TklgHQHbt2%2BPYcOG5T0GAPRIW7dujaFDhx7RP1NgHQH9%2B/ePiLf/BVdVVeU8DQD0DB0dHTFs2LDC9%2BEjSWAdAe%2B8LFhVVSWwAOAIy%2BPyHBe5AwAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDE/LJnADhEY%2BYvzXuED2Tt97%2BU9wg9ljNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAepfGxsYoKyuLhoaGwlpnZ2fMmTMnBg8eHP369Yvzzz8/tm3bluOUAEB3J7D%2Bv2eeeSbuuuuuGDlyZNF6Q0NDLFmyJJqammLVqlWxa9eumDp1ahw4cCCnSQGA7k5gRcSuXbvioosuirvvvjsGDBhQWG9vb4977rknbrrpppg0aVJ8/vOfj4ceeig2btwYy5Yty3FiAKA7E1gRMXv27DjvvPNi0qRJRestLS2xf//%2BmDx5cmGtrq4uhg8fHqtXr37P5%2Bvs7IyOjo6iGwDQc5TnPUDempqaoqWlJdauXdvl2I4dO6KioqLorFZERE1NTezYseM9n7OxsTFuuOGG5LMCAKWhR5/B2rp1a1x55ZXx93//99GnT59D/rgsy6KsrOw9j8%2BbNy/a29sLt61bt6YYFwAoET06sFpaWqKtrS1Gjx4d5eXlUV5eHsuXL4/bb789ysvLo6amJvbt2xevv/560ce1tbVFTU3Nez5vZWVlVFVVFd0AgJ6jRwfWxIkTY%2BPGjbF%2B/frCbcyYMXHRRRcV/rl3797R3Nxc%2BJjW1tbYtGlTjBs3LsfJAYDurEdfg9W/f/8YPnx40Vq/fv1i0KBBhfVZs2bFVVddFYMGDYqBAwfG1VdfHSNGjOhyQTwAwDt6dGAdiltuuSXKy8vjggsuiD179sTEiRPj/vvvj169euU9GgDQTZVlWZblPcTHXUdHR1RXV0d7e7vrsQBK2Jj5S/Me4QNZ%2B/0v5T1CrvL8/tujr8ECADgcBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAifX4wLrzzjtj5MiRUVVVFVVVVTF27Nj42c9%2BVjje2dkZc%2BbMicGDB0e/fv3i/PPPj23btuU4MQDQ3fX4wBo6dGgsWrQo1q5dG2vXro0JEybEV77yldi8eXNERDQ0NMSSJUuiqakpVq1aFbt27YqpU6fGgQMHcp4cAOiuyrIsy/IeorsZOHBg/OAHP4ivfe1rceyxx8aDDz4YF154YUREbN%2B%2BPYYNGxZPPPFEnHvuuYf0fB0dHVFdXR3t7e1RVVV1OEcH4DAaM39p3iN8IGu//6W8R8hVnt9/e/wZrHc7cOBANDU1xe7du2Ps2LHR0tIS%2B/fvj8mTJxceU1dXF8OHD4/Vq1fnOCkA0J2V5z1Ad7Bx48YYO3Zs7N27N44%2B%2BuhYsmRJnHzyybF%2B/fqoqKiIAQMGFD2%2BpqYmduzY8Z7P19nZGZ2dnYX7HR0dh212AKD7cQYrIj772c/G%2BvXrY82aNXH55ZfHjBkz4le/%2BtV7Pj7LsigrK3vP442NjVFdXV24DRs27HCMDQB0UwIrIioqKuIzn/lMjBkzJhobG2PUqFFx2223RW1tbezbty9ef/31ose3tbVFTU3Nez7fvHnzor29vXDbunXr4f4UAIBuRGAdRJZl0dnZGaNHj47evXtHc3Nz4Vhra2ts2rQpxo0b954fX1lZWXjbh3duAEDP0eOvwbruuutiypQpMWzYsNi5c2c0NTXF008/HUuXLo3q6uqYNWtWXHXVVTFo0KAYOHBgXH311TFixIiYNGlS3qMDAN1Ujw%2BsV155JS655JJobW2N6urqGDlyZCxdujTOOeeciIi45ZZbory8PC644ILYs2dPTJw4Me6///7o1atXzpMDAN2V98E6ArwPFsDHg/fBKi3eBwsA4GNEYAEAJCawAAASK9nAeuihh2Lv3r15jwEA0EXJBtbcuXOjtrY2Lr300vjlL3%2BZ9zgAAAUlG1jbt2%2BPe%2B%2B9N1pbW%2BOMM86IU045JW666aZ49dVX8x4NAOjhSjawysvLY9q0afH444/Hli1bYsaMGXHvvffG0KFDY9q0afHP//zP4R0oAIA8lGxgvVttbW1MnDgxxo8fH2VlZbF27dqYPn16nHDCCbFy5cq8xwMAepiSDqzXXnstbr311hg1alT8wR/8QbS1tcVjjz0WL730Urz88ssxderU%2BJM/%2BZO8xwQAepiS/VU5f/RHfxRPPPFE1NfXx7e%2B9a2YMWNGHHvssYXjRx99dFxzzTVx%2B%2B235zglANATlWxgVVVVxbJly%2BLMM898z8cMGTIknn/%2B%2BSM4FQBACQfWAw888L6PKSsri%2BOPP/4ITAMA8L9K9hqs7373u7F48eIu6z/60Y/iqquuymEiAIC3lWxg/fSnP43TTz%2B9y/rYsWPj4YcfzmEiAIC3lWxgvfbaazFgwIAu61VVVfHaa6/lMBEAwNtKNrCOP/74%2BPnPf95l/ec//3nU19fnMBEAwNtK9iL3hoaGaGhoiP/6r/%2BKCRMmRETEv/zLv8SNN94YP/zhD3OeDgDoyUo2sL797W/H3r17Y%2BHChXH99ddHRMTQoUPj9ttvj29%2B85s5TwcA9GQlG1gREXPmzIk5c%2BZEa2tr9O3bN4455pi8RwIAKO3AeseQIUPyHgEAoKBkL3J/9dVX4xvf%2BEZ86lOfij59%2BkRFRUXRDQAgLyV7BmvmzJnxm9/8Jv78z/88hgwZEmVlZXmPBAAQESUcWCtWrIgVK1bE5z//%2BbxHAQAoUrIvEQ4dOtRZKwCgWyrZwLrlllti3rx5sW3btrxHAQAoUrIvEV5yySWxc%2BfO%2BPSnPx1VVVXRu3fvouNtbW05TQYA9HQlG1iLFi3KewQAgIMq2cCaNWtW3iMAABxUyV6DFRHx29/%2BNhYsWBCXXHJJ4SXBJ598Mn7961/nPBkA0JOVbGCtXLkyTjnllFi%2BfHk88sgjsWvXroiIePbZZ%2BN73/teztMBAD1ZyQbWX/zFX8SCBQviqaeeKnrn9gkTJsSaNWtynAwA6OlKNrA2bNgQX/va17qsH3fccfHqq6/mMBEAwNtKNrCOOeaY2LFjR5f19evXxyc/%2BckcJgIAeFvJBtbXv/71uPbaa%2BPVV18tvKP7v/3bv8XVV18dF198cc7TAQA9WckG1sKFC6O2tjaGDBkSu3btipNPPjnGjRsXp512WvzlX/5l3uMBAD1Yyb4PVkVFRTz88MPxH//xH/Hss8/GW2%2B9Faeeemp87nOfy3s0AKCHK9nAeseJJ54YJ554Yt5jAAAUlGxg/emf/unvPH7XXXcdoUkAAIqVbGC1trYW3d%2B/f39s3rw5du7cGWeddVZOUwEAlHBg/dM//VOXtTfffDMuv/zyOOmkk3KYCADgbSX7U4QHU15eHldffXX84Ac/yHsUAKAH%2B1gFVkTECy%2B8EPv37897DACgByvZlwivueaaovtZlkVra2s8/vjjcdFFF%2BU0FQBACQfWv/7rvxbdP%2Bqoo%2BLYY4%2BNRYsWxbe//e2cpgIAKOHAWrlyZd4jAAAc1MfuGiwAgLyV7Bms0047rfBLnt/PL3/5y8M8DQDA/yrZwPriF78Yf/u3fxsnnnhijB07NiIi1qxZE88991xceumlUVlZmfOEAEBPVbKB9cYbb8Ts2bNj4cKFRevz58%2BPV155JX784x/nNBkA0NOV7DVYjzzySHzjG9/osj5z5sz46U9/msNEAABvK9nAqqysjNWrV3dZX716tZcHAYBclexLhH/2Z38Wl112Waxbty5OP/30iHj7Gqy77747rrvuupynAwB6spINrPnz50d9fX3cdtttce%2B990ZExEknnRR33313TJ8%2BPefpAICerGQDKyJi%2BvTpYgoA6HZK9hqsiIiOjo64//7743vf%2B168/vrrERHx7//%2B79Ha2przZABAT1ayZ7A2bdoUkyZNik984hOxdevWmDlzZgwYMCAeeeSR2LZtWzzwwAN5jwgA9FAlewbru9/9bkyfPj1%2B85vfRJ8%2BfQrr5513XqxYsSLHyQCAnq5kA%2BuZZ56J73znO11%2BXc4nP/nJD/QSYWNjY5x22mnRv3//OO644%2BKrX/1qPPfcc0WP6ezsjDlz5sTgwYOjX79%2Bcf7558e2bduSfB4AwMdPyQZWRUVF7Nq1q8v6888/H4MHDz7k51m%2BfHnMnj071qxZE83NzfHmm2/G5MmTY/fu3YXHNDQ0xJIlS6KpqSlWrVoVu3btiqlTp8aBAweSfC4AwMdLyV6Ddf7558df/dVfxcMPPxwREWVlZfHyyy/HtddeG9OmTTvk51m6dGnR/fvuuy%2BOO%2B64aGlpibPOOiva29vjnnvuiQcffDAmTZoUEREPPfRQDBs2LJYtWxbnnntuuk8KAPhYKNkzWDfddFNs3749amtrY8%2BePTFhwoT4vd/7vejTp0%2BX30/4QbS3t0dExMCBAyMioqWlJfbv3x%2BTJ08uPKauri6GDx9%2B0HeSBwAo2TNY1dXVsXr16mhubo5nn3023nrrrTj11FPj3HPP7XJd1qHKsizmzp0bZ5xxRgwfPjwiInbs2BEVFRUxYMCAosfW1NTEjh07Dvo8nZ2d0dnZWbjf0dHxoeYBAEpTSQbW/v374w//8A/jjjvuiMmTJxedXfoorrjiitiwYUOsWrXqfR%2BbZdl7hlxjY2PccMMNSWYCgA9rzPyl7/%2BgbmLt97%2BU9whJleRLhL17945169Z96DNVBzNnzpx4/PHH46mnnoqhQ4cW1mtra2Pfvn2FNzJ9R1tbW9TU1Bz0uebNmxft7e2F29atW5PNCQB0fyUZWBERF198cdx3330f%2BXmyLIsrrrgiHn300fjFL34R9fX1RcdHjx4dvXv3jubm5sJaa2trbNq0KcaNG3fQ56ysrIyqqqqiGwDQc5TkS4TvWLx4cSxbtizGjBkT/fr1Kzp24403HtJzzJ49O37yk5/EP/7jP0b//v0L11VVV1dH3759o7q6OmbNmhVXXXVVDBo0KAYOHBhXX311jBgxovBThQAA71aygdXS0hIjR46MiIgNGzYUHfsgLx3eeeedERExfvz4ovX77rsvZs6cGRERt9xyS5SXl8cFF1wQe/bsiYkTJ8b9998fvXr1%2BvCfAADwsVWWZVmW9xAfxAsvvBD19fVJr7863Do6OqK6ujra29u9XAhQwkrpovFSczgucs/z%2B2/JXYN1wgknxKuvvlq4f%2BGFF8Yrr7yS40QAAMVKLrD%2B7wm3J554oujX2gAA5K3kAgsAoLsrucAqKyvrcv1VKV2PBQB8/JXcTxFmWRYzZ86MysrKiIjYu3dvXHbZZV3epuHRRx/NYzwAgNILrBkzZhTdv/jii3OaBADg4EousFK8ezsAwOFUctdgAQB0dwILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkJjAAgBITGABACQmsAAAEhNYAACJCSwAgMQEFgBAYgILACAxgQUAkFh53gMA0HONmb807xHgsHAGCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkVp73AHw0Y%2BYvzXuEQ7b2%2B1/KewQAOCKcwQIASKzHB9aKFSviy1/%2BctTV1UVZWVk89thjRcezLIsFCxZEXV1d9O3bN8aPHx%2BbN2/OaVoAoBT0%2BMDavXt3jBo1KhYvXnzQ4zfeeGPcfPPNsXjx4njmmWeitrY2zjnnnNi5c%2BcRnhQAKBU9/hqsKVOmxJQpUw56LMuyuPXWW2P%2B/Pkxbdq0iIh44IEHoqamJn7yk5/EpZdeeiRHBQBKRI8/g/W7vPjii7Fjx46YPHlyYa2ysjLOPvvsWL169Xt%2BXGdnZ3R0dBTdAICeQ2D9Djt27IiIiJqamqL1mpqawrGDaWxsjOrq6sJt2LBhh3VOAKB7EViHoKysrOh%2BlmVd1t5t3rx50d7eXrht3br1cI8IAHQjPf4arN%2BltrY2It4%2BkzVkyJDCeltbW5ezWu9WWVkZlZWVh30%2BAKB7cgbrd6ivr4/a2tpobm4urO3bty%2BWL18e48aNy3EyAKA76/FnsHbt2hX/%2BZ//Wbj/4osvxvr162PgwIHxqU99KhoaGmLhwoVxwgknxAknnBALFy6MT3ziEzF9%2BvQcpwYAurMeH1hr166NL37xi4X7c%2BfOjYiIGTNmxP333x/XXHNN7NmzJ77zne/E66%2B/Hl/4whfiySefjP79%2B%2Bc1MgDQzfX4wBo/fnxkWfaex8vKymLBggWxYMGCIzcUAFDSXIMFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwAIASExgAQAkJrAAABITWAAAiQksAIDEBBYAQGICCwAgMYEFAJCYwDpEd9xxR9TX10efPn1i9OjRsXLlyrxHAgC6KYF1CB5%2B%2BOFoaGiI%2BfPnx7p16%2BLMM8%2BMKVOmxJYtW/IeDQDohgTWIbj55ptj1qxZ8a1vfStOOumkuPXWW2PYsGFx55135j0aANANlec9QHe3b9%2B%2BaGlpiWuvvbZoffLkybF69eqDfkxnZ2d0dnYW7re3t0dEREdHR/L5DnTuTv6ch8vh%2BPyB0lZKX8M4vA7H94h3njPLsuTP/X4E1vt47bXX4sCBA1FTU1O0XlNTEzt27DjoxzQ2NsYNN9zQZX3YsGGHZcZSUX1T3hMA0F0dzu8RO3fujOrq6sP3BxyEwDpEZWVlRfezLOuy9o558%2BbF3LlzC/ffeuut%2BO///u8YNGjQe37Mh9HR0RHDhg2LrVu3RlVVVbLn7UnsYRr28aOzhx%2BdPUzj47SPWZbFzp07o66u7oj/2QLrfQwePDh69erV5WxVW1tbl7Na76isrIzKysqitWOOOeawzVhVVVXy/xHkzR6mYR8/Onv40dnDND4u%2B3ikz1y9w0Xu76OioiJGjx4dzc3NRevNzc0xbty4nKYCALozZ7AOwdy5c%2BOSSy6JMWPGxNixY%2BOuu%2B6KLVu2xGWXXZb3aABAN9RrwYIFC/IeorsbPnx4DBo0KBYuXBg//OEPY8%2BePfHggw/GqFGj8h4tevXqFePHj4/ycq38YdnDNOzjR2cPPzp7mIZ9/OjKsjx%2BdhEA4GPMNVgAAIkJLACAxAQWAEBiAgsAIDGBVaLuuOOOqK%2Bvjz59%2BsTo0aNj5cqVeY/Ura1YsSK%2B/OUvR11dXZSVlcVjjz1WdDzLsliwYEHU1dVF3759Y/z48bF58%2Bacpu2eGhsb47TTTov%2B/fvHcccdF1/96lfjueeeK3pMZ2dnzJkzJwYPHhz9%2BvWL888/P7Zt25bTxN3PnXfeGSNHjiy8gePYsWPjZz/7WeG4/fvgGhsbo6ysLBoaGgpr9vH9LViwIMrKyoputbW1heO%2BJn50AqsEPfzww9HQ0BDz58%2BPdevWxZlnnhlTpkyJLVu25D1at7V79%2B4YNWpULF68%2BKDHb7zxxrj55ptj8eLF8cwzz0RtbW2cc845sXPnziM8afe1fPnymD17dqxZsyaam5vjzTffjMmTJ8fu3f/7y3obGhpiyZIl0dTUFKtWrYpdu3bF1KlT48CBAzlO3n0MHTo0Fi1aFGvXro21a9fGhAkT4itf%2BUrhG5f9%2B2CeeeaZuOuuu2LkyJFF6/bx0JxyyinR2tpauG3cuLFwzNfEBDJKzu///u9nl112WdHa5z73uezaa6/NaaLSEhHZkiVLCvffeuutrLa2Nlu0aFFhbe/evVl1dXX2N3/zN3mMWBLa2tqyiMiWL1%2BeZVmWvfHGG1nv3r2zpqamwmNefvnl7KijjsqWLl2a15jd3oABA7If//jH9u8D2rlzZ3bCCSdkzc3N2dlnn51deeWVWZb5e3iorr/%2B%2BmzUqFEHPeZrYhrOYJWYffv2RUtLS0yePLloffLkybF69eqcpiptL774YuzYsaNoTysrK%2BPss8%2B2p79De3t7REQMHDgwIiJaWlpi//79RftYV1cXw4cPt48HceDAgWhqaordu3fH2LFj7d8HNHv27DjvvPNi0qRJRev28dA9//zzUVdXF/X19fH1r389XnjhhYjwNTEVb9FaYl577bU4cOBAl180XVNT0%2BUXUnNo3tm3g%2B3pSy%2B9lMdI3V6WZTF37tw444wzYvjw4RHx9j5WVFTEgAEDih7r72axjRs3xtixY2Pv3r1x9NFHx5IlS%2BLkk0%2BO9evX279D1NTUFC0tLbF27doux/w9PDRf%2BMIX4u/%2B7u/ixBNPjFdeeSX%2B%2Bq//OsaNGxebN2/2NTERgVWiysrKiu5nWdZljQ/Gnh66K664IjZs2BCrVq1638fax2Kf/exnY/369fHGG2/EP/zDP8SMGTNi%2BfLl7/l4%2B1ds69atceWVV8aTTz4Zffr0OeSPs4/FpkyZUvjnESNGxNixY%2BP444%2BPBx54IE4//fSI8DXxo/ISYYkZPHhw9OrVq8v/ibW1tXX5vw0OzTs/OWNPD82cOXPi8ccfj6eeeiqGDh1aWK%2BtrY19%2B/bF66%2B/XvR4%2B1isoqIiPvOZz8SYMWOisbExRo0aFbfddpv9O0QtLS3R1tYWo0ePjvLy8igvL4/ly5fH7bffHuXl5VFTU2MfP4R%2B/frFiBEj4vnnn/c1MRGBVWIqKipi9OjR0dzcXLTe3Nwc48aNy2mq0lZfXx%2B1tbVFe7pv375Yvny5PX2XLMviiiuuiEcffTR%2B8YtfRH19fdHx0aNHR%2B/evYv2sbW1NTZt2mQff4csy6Kzs9P%2BHaKJEyfGxo0bY/369YXbmDFj4qKLLir8s3384Do7O%2BPXv/51DBkyxNfEVHK7vJ4PrampKevdu3d2zz33ZL/61a%2ByhoaGrF%2B/ftlvf/vbvEfrtnbu3JmtW7cuW7duXRYR2c0335ytW7cue%2Bmll7Isy7JFixZl1dXV2aOPPppt3Lgx%2B%2BM//uNsyJAhWUdHR86Tdx%2BXX355Vl1dnT399NNZa2tr4fY///M/hcdcdtll2dChQ7Nly5Zlzz77bDZhwoRs1KhR2Ztvvpnj5N3HvHnzshUrVmQvvvhitmHDhuy6667LjjrqqOzJJ5/Mssz%2BfVjv/inCLLOPh%2BKqq67Knn766eyFF17I1qxZk02dOjXr379/4fuIr4kfncAqUT/60Y%2ByT3/601lFRUV26qmnFn5UnoN76qmnsojocpsxY0aWZW//WPL111%2Bf1dbWZpWVldlZZ52Vbdy4Md%2Bhu5mD7V9EZPfdd1/hMXv27MmuuOKKbODAgVnfvn2zqVOnZlu2bMlv6G7mm9/8ZuG/22OPPTabOHFiIa6yzP59WP83sOzj%2B7vwwguzIUOGZL17987q6uqyadOmZZs3by4c9zXxoyvLsizL59wZAMDHk2uwAAASE1gAAIkJLACAxAQWAEBiAgsAIDGBBQCQmMACAEhMYAEAJCawAAASE1gAAIkJLACAxAQWAEBiAgsAIDGBBQCQmMACAEhMYAEAJCawAAASE1gAAIn9P%2B%2Bkudaz8hF/AAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common-5526005910680823130">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">46.1</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">45.8</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">47.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">48.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">41.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40.7</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">45.4</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">44.2</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">43.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (49)</td>
        <td class="number">58</td>
        <td class="number">49.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">27</td>
        <td class="number">23.1%</td>
        <td>
            <div class="bar" style="width:47%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme-5526005910680823130">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">11</td>
        <td class="number">9.4%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">39.5</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40.7</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">41.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:19%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">41.6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">51.6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">51.7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">52.3</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">52.5</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">53.6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Grave Number">Grave Number<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="alert">
            <th>Distinct count</th>
            <td>116</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>99.1%</td>
        </tr>
        <tr class="ignore">
            <th>Missing (%)</th>
            <td>0.0%</td>
        </tr>
        <tr class="ignore">
            <th>Missing (n)</th>
            <td>0</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable6976805429671404438">
    <table class="mini freq">
        <tr class="">
    <th>G818</th>
    <td>
        <div class="bar" style="width:2%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 1.7%">
            &nbsp;
        </div>
        2
    </td>
</tr><tr class="">
    <th>G828</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="">
    <th>G885</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="other">
    <th>Other values (113)</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 96.6%">
            113
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable6976805429671404438, #minifreqtable6976805429671404438"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable6976805429671404438">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">G818</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G828</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G885</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G841</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G58</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G326</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G40</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G305</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G73</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G341</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (106)</td>
        <td class="number">106</td>
        <td class="number">90.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Height in grave">Height in grave<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>54</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>46.2%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>159.85</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>185</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>1.7%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram1138865514347627407">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAbBJREFUeJzt3EGOk2AYx%2BGXxi0coIFbeAjv5No7eQhvAekBaFy4sLjQunHmn8xoPorzPEua8lL4fmnaBLpt27ZqbFmWmqap9VgObp7nGsex6cx3Taf90vd9Vf38wMMw7HEIHMi6rjVN0%2B9109IugXRdV1VVwzAI5CDef/z84vd8%2BfThnx7Dfd20dGo%2BEQ5EIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBDscsstb8Mj3Kb7t3yDQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQPBwT1b8H57G9%2Bhec47fql0C2batqqrWdf3jte/fvr54f0/th%2Be95hy38tS1vG%2B7r5uWum2Hqcuy1DRNrcdycPM81ziOTWfuEsjtdqvL5VJ931fXda3HczDbttX1eq3z%2BVynU9ufzbsEAkfhXywIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEgh9oeFDEabr9oAAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives1138865514347627407,#minihistogram1138865514347627407"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives1138865514347627407">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles1138865514347627407"
                                                  aria-controls="quantiles1138865514347627407" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram1138865514347627407" aria-controls="histogram1138865514347627407"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common1138865514347627407" aria-controls="common1138865514347627407"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme1138865514347627407" aria-controls="extreme1138865514347627407"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles1138865514347627407">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>148</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>155</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>161</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>170</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>179.1</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>185</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>185</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>15</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>23.246</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.14542</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>37.616</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>159.85</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>10.768</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-5.6231</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>18703</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>540.38</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram1138865514347627407">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3Xt4VOWBx/HfSJIB0mTklkwiEbMs1kuAcnGReIFyCeQRsUsVLYigyEpFNAKLRNYKXU0olkstlRWLgFoX9Flg7ULBsEUuT6BAgArYRZRUgiQEbJgJCJMI7/7hOssYbsKbnJzM9/M853mY95w5%2Bb2%2Bwfw4czLjMcYYAQAAwJqrnA4AAADQ0FCwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMCyGKcDRIMzZ87o0KFDSkhIkMfjcToOAABRwRijyspKpaam6qqr6vaaEgWrDhw6dEhpaWlOxwAAICqVlJSodevWdfo1KVh1ICEhQdLXC5yYmOhwGgAAokMwGFRaWlr453BdomDVgW9eFkxMTKRgAQBQx5y4PYeb3AEAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWRX3Buu666%2BTxeGpsY8aMkSSFQiGNHTtWLVu2VHx8vAYOHKiDBw86nBoAANRnUV%2Bwtm7dqtLS0vBWUFAgSbrvvvskSTk5OVq2bJkWL16sjRs36vjx4xowYIBOnz7tZGwAAFCPeYwxxukQ9UlOTo7%2B67/%2BS/v27VMwGFSrVq305ptv6v7775f0/x/cvHLlSvXr1%2B%2BSzhkMBuXz%2BRQIBPioHAAA6oiTP3%2Bj/grW2aqqqvTWW2/pkUcekcfjUVFRkaqrq5WVlRU%2BJjU1VRkZGSosLDzveUKhkILBYMQGAACiBx/2fJbly5fr2LFjGjFihCSprKxMcXFxatasWcRxycnJKisrO%2B958vPzNXXq1NqMCgBwQNfJq5yO8J1se7G/0xGiFlewzjJ//nxlZ2crNTX1gscZYy74ydy5ubkKBALhraSkxHZUAABQj3EF6/989tlnWrNmjZYuXRoe8/v9qqqqUkVFRcRVrPLycmVmZp73XF6vV16vt1bzAgCA%2BosrWP9nwYIFSkpK0l133RUe69Kli2JjY8O/WShJpaWl2r179wULFgAAiG5cwZJ05swZLViwQMOHD1dMzP//J/H5fBo5cqTGjx%2BvFi1aqHnz5powYYLat2%2BvPn36OJgYAADUZxQsSWvWrNGBAwf0yCOP1Ng3a9YsxcTEaPDgwTp58qR69%2B6thQsXqlGjRg4kBQAAbsD7YNUB3gcLABoGfovQXXgfLAAAgAaEggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsGS9Pnnn%2BvBBx9UixYt1LRpU/3gBz9QUVFReL8xRlOmTFFqaqqaNGminj17as%2BePQ4mBgAA9VnUF6yKigrddtttio2N1R/%2B8Ad99NFHmjFjhq6%2B%2BurwMdOnT9fMmTM1Z84cbd26VX6/X3379lVlZaWDyQEAQH0V43QAp/3iF79QWlqaFixYEB677rrrwn82xmj27NmaPHmyBg0aJElatGiRkpOT9fbbb%2Buxxx6r68gAAKCei/orWO%2B99566du2q%2B%2B67T0lJSerUqZNee%2B218P7i4mKVlZUpKysrPOb1etWjRw8VFhY6ERkAANRzUV%2Bw9u/fr7lz56pdu3ZavXq1Ro8erSeffFJvvPGGJKmsrEySlJycHPG85OTk8L5vC4VCCgaDERsAAIgeUf8S4ZkzZ9S1a1fl5eVJkjp16qQ9e/Zo7ty5euihh8LHeTyeiOcZY2qMfSM/P19Tp06tvdAAAKBei/orWCkpKbrpppsixm688UYdOHBAkuT3%2ByWpxtWq8vLyGle1vpGbm6tAIBDeSkpKaiE5AACor6K%2BYN12223au3dvxNjHH3%2BsNm3aSJLS09Pl9/tVUFAQ3l9VVaV169YpMzPznOf0er1KTEyM2AAAQPSI%2BpcIn376aWVmZiovL0%2BDBw/Wli1bNG/ePM2bN0/S1y8N5uTkKC8vT%2B3atVO7du2Ul5enpk2basiQIQ6nBwAA9VHUF6xbbrlFy5YtU25urn7%2B858rPT1ds2fP1tChQ8PHTJw4USdPntTjjz%2BuiooKdevWTe%2B//74SEhIcTA4AAOorjzHGOB2ioQsGg/L5fAoEArxcCAAu1nXyKqcjfCfbXuzvdARHOfnzN%2BrvwQIAALCNggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwLOoL1pQpU%2BTxeCI2v98f3m%2BM0ZQpU5SamqomTZqoZ8%2Be2rNnj4OJAQBAfRf1BUuSbr75ZpWWloa3Xbt2hfdNnz5dM2fO1Jw5c7R161b5/X717dtXlZWVDiYGAAD1GQVLUkxMjPx%2Bf3hr1aqVpK%2BvXs2ePVuTJ0/WoEGDlJGRoUWLFunLL7/U22%2B/7XBqAABQX1GwJO3bt0%2BpqalKT0/XAw88oP3790uSiouLVVZWpqysrPCxXq9XPXr0UGFh4XnPFwqFFAwGIzYAABA9or5gdevWTW%2B88YZWr16t1157TWVlZcrMzNQXX3yhsrIySVJycnLEc5KTk8P7ziU/P18%2Bny%2B8paWl1eocAABA/RL1BSs7O1s//vGP1b59e/Xp00crVqyQJC1atCh8jMfjiXiOMabG2Nlyc3MVCATCW0lJSe2EBwAA9VLUF6xvi4%2BPV/v27bVv377wbxN%2B%2B2pVeXl5jataZ/N6vUpMTIzYAABA9KBgfUsoFNJf/vIXpaSkKD09XX6/XwUFBeH9VVVVWrdunTIzMx1MCQAA6rMYpwM4bcKECbr77rt17bXXqry8XC%2B88IKCwaCGDx8uj8ejnJwc5eXlqV27dmrXrp3y8vLUtGlTDRkyxOnoAACgnor6gnXw4EH95Cc/0dGjR9WqVSvdeuut2rx5s9q0aSNJmjhxok6ePKnHH39cFRUV6tatm95//30lJCQ4nBwAANRXHmOMcTpEQxcMBuXz%2BRQIBLgfCwBcrOvkVU5H%2BE62vdjf6QiOcvLnL/dgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABY5tqC9dZbb%2BnUqVNOxwAAAKjBtQVr3Lhx8vv9euyxx7Rlyxan4wAAAIS5tmAdOnRIr7/%2BukpLS3X77bfr5ptv1owZM3TkyBGnowEAgCjn2oIVExOjQYMG6b333tOBAwc0fPhwvf7662rdurUGDRqkFStWyBjjdEwAABCFXFuwzub3%2B9W7d2/17NlTHo9H27Zt05AhQ9SuXTtt2LDB6XgAACDKuLpgHT16VLNnz1bHjh112223qby8XMuXL9dnn32mzz//XAMGDNBDDz3kdEwAABBlYpwOcLn%2B8R//UStXrlR6eroeffRRDR8%2BXK1atQrv/973vqeJEyfq5ZdfdjAlAACIRq4tWImJiVqzZo3uuOOO8x6TkpKiffv21WEqAAAAFxesRYsWXfQYj8ejtm3b1kEaAACA/%2Bfae7CefvppzZkzp8b4b37zG40fP96BRAAAAF9zbcF69913deutt9YY7969u5YsWeJAIgAAgK%2B59iXCo0ePqlmzZjXGExMTdfToUQcSAQBQv3SdvMrpCJds24v9nY5glWuvYLVt21arV6%2BuMb569Wqlp6c7kAgAAOBrrr2ClZOTo5ycHH3xxRfq1auXJOm///u/NX36dP3yl790OB0AAIhmri1Yo0aN0qlTp5SXl6fnn39ektS6dWu9/PLLeuSRRxxOBwAAoplrXyKUpLFjx6q0tFSff/65/va3v%2BnAgQNXVK7y8/Pl8XiUk5MTHguFQho7dqxatmyp%2BPh4DRw4UAcPHrQRHwAANFCuLljfSElJ0dVXX31F59i6davmzZunDh06RIzn5ORo2bJlWrx4sTZu3Kjjx49rwIABOn369BV9PQAA0HC5tmAdOXJEDz/8sK699lo1btxYcXFxEdt3cfz4cQ0dOlSvvfZaxG8mBgIBzZ8/XzNmzFCfPn3UqVMnvfXWW9q1a5fWrFlje0oAAKCBcO09WCNGjNCnn36qf/7nf1ZKSoo8Hs9ln2vMmDG666671KdPH73wwgvh8aKiIlVXVysrKys8lpqaqoyMDBUWFqpfv35XNAcAANAwubZgrV%2B/XuvXr1enTp2u6DyLFy9WUVGRtm3bVmNfWVmZ4uLiarzfVnJyssrKys57zlAopFAoFH4cDAavKCMAAHAX175E2Lp16yu6aiVJJSUleuqpp/S73/1OjRs3vuTnGWMu%2BLXz8/Pl8/nCW1pa2hXlBAAA7uLagjVr1izl5uZe0W/0FRUVqby8XF26dFFMTIxiYmK0bt06vfzyy4qJiVFycrKqqqpUUVER8bzy8nIlJyef97y5ubkKBALhraSk5LIzAgAA93HtS4TDhg1TZWWl2rRpo8TERMXGxkbsLy8vv%2Bg5evfurV27dkWMPfzww7rhhhv0zDPPKC0tTbGxsSooKNDgwYMlSaWlpdq9e7emT59%2B3vN6vV55vd7LmBUAAGgIXFuwpk2bdsXnSEhIUEZGRsRYfHy8WrRoER4fOXKkxo8frxYtWqh58%2BaaMGGC2rdvrz59%2Blzx1wcAAA2TawvWyJEj6%2BTrzJo1SzExMRo8eLBOnjyp3r17a%2BHChWrUqFGdfH0AAOA%2BHmOMcTrE5frrX/%2BqhQsX6tNPP9WMGTOUlJSk999/X2lpabrxxhudjhcWDAbl8/kUCASUmJjodBwAwGXqOnmV0xEarG0v9rd%2BTid//rr2JvcNGzbo5ptv1rp16/TOO%2B/o%2BPHjkqTt27frZz/7mcPpAABANHNtwXrmmWc0ZcoUrV27NuKd23v16qXNmzc7mAwAAEQ71xasDz/8UPfee2%2BN8aSkJB05csSBRAAAAF9zbcG6%2Buqrz/lu6jt37tQ111zjQCIAAICvubZgPfDAA5o0aZKOHDkSflf1P/3pT5owYYIefPBBh9MBAIBo5tqClZeXJ7/fr5SUFB0/flw33XSTMjMzdcstt%2Bi5555zOh4AAIhirn0frLi4OC1ZskQff/yxtm/frjNnzqhz58664YYbnI4GAACinGsL1jeuv/56XX/99U7HAAAACHNtwfqnf/qnC%2B6fN29eHSUBAACI5NqCVVpaGvG4urpae/bsUWVlpe68806HUgEAALi4YP3%2B97%2BvMfbVV1/ppz/9ab36mBwAABB9XPtbhOcSExOjCRMm6KWXXnI6CgAAiGINqmBJ0v79%2B1VdXe10DAAAEMVc%2BxLhxIkTIx4bY1RaWqr33ntPQ4cOdSgVAACAiwvWpk2bIh5fddVVatWqlaZNm6ZRo0Y5lAoAAMDFBWvDhg1ORwAAADinBncPFgAAgNNcewXrlltuCX/I88Vs2bKlltMAAAD8P9cWrB/%2B8Id69dVXdf3116t79%2B6SpM2bN2vv3r167LHH5PV6HU4IAACilWsL1rFjxzRmzBjl5eVFjE%2BePFmHDx/Wb3/7W4eSAQCAaOfae7DeeecdPfzwwzXGR4wYoXfffdeBRAAAAF9zbcHyer0qLCysMV5YWMjLgwAAwFGufYnwySef1OjRo7Vjxw7deuutkr6%2BB%2Bu1117Ts88%2B63A6AAAQzVxbsCZPnqz09HT96le/0uuvvy5JuvHGG/Xaa69pyJAhDqcDAADRzLUFS5KGDBlCmQIAAPWOa%2B/BkqRgMKiFCxfqZz/7mSoqKiRJf/7zn1VaWupwMgAAEM1cewVr9%2B7d6tOnj5o2baqSkhKNGDFCzZo10zvvvKODBw9q0aJFTkcEAABRyrVXsJ5%2B%2BmkNGTJEn376qRo3bhwev%2Buuu7R%2B/XoHkwEAgGjn2itYW7du1dy5c2t8XM4111zDS4QAAMBRrr2CFRcXp%2BPHj9cY37dvn1q2bOlAIgAAgK%2B5tmANHDhQ//qv/6qvvvpKkuTxePT5559r0qRJGjRokMPpAABANHNtwZoxY4YOHTokv9%2BvkydPqlevXvq7v/s7NW7cuMbnEwIAANQl196D5fP5VFhYqIKCAm3fvl1nzpxR586d1a9fvxr3ZQEAANQlV17Bqq6uVt%2B%2BffXJJ58oKytLkyZN0rPPPqv%2B/ft/53I1d%2B5cdejQQYmJiUpMTFT37t31hz/8Ibw/FApp7NixatmypeLj4zVw4EAdPHjQ9pQAAEAD4sqCFRsbqx07dli5UtW6dWtNmzZN27Zt07Zt29SrVy/dc8892rNnjyQpJydHy5Yt0%2BLFi7Vx40YdP35cAwYM0OnTp6/4awMAgIbJY4wxToe4HDk5OYqPj9eLL75o/dzNmzfXSy%2B9pHvvvVetWrXSm2%2B%2Bqfvvv1%2BSdOjQIaWlpWnlypXq16/fJZ0vGAzK5/MpEAgoMTHRel4AQN3oOnmV0xEarG0v9rd%2BTid//rr2HixJmjNnjtasWaOuXbsqPj4%2BYt/06dO/8/lOnz6td999VydOnFD37t1VVFSk6upqZWVlhY9JTU1VRkaGCgsLz1uwQqGQQqFQ%2BHEwGPzOWQAAgHu5tmAVFRWpQ4cOkqQPP/wwYt93felw165d6t69u06dOqXvfe97WrZsmW666Sbt3LlTcXFxatasWcTxycnJKisrO%2B/58vPzNXXq1O%2BUAQAANByuK1j79%2B9Xenq6NmzYYO2c3//%2B97Vz504dO3ZM//Ef/6Hhw4dr3bp15z3eGHPBEpebm6tx48aFHweDQaWlpVnLCwAA6jfX3eTerl07HTlyJPz4/vvv1%2BHDh6/onHFxcfr7v/97de3aVfn5%2BerYsaN%2B9atfye/3q6qqShUVFRHHl5eXKzk5%2Bbzn83q94d9K/GYDAADRw3UF69v35K9cuVInTpyw/jVCoZC6dOmi2NhYFRQUhPeVlpZq9%2B7dyszMtPo1AQBAw%2BG6lwhte/bZZ5Wdna20tDRVVlZq8eLF%2BuCDD7Rq1Sr5fD6NHDlS48ePV4sWLdS8eXNNmDBB7du3V58%2BfZyODgAA6inXFSyPx1Pj/qcreT%2Bsw4cPa9iwYSotLZXP51OHDh20atUq9e3bV5I0a9YsxcTEaPDgwTp58qR69%2B6thQsXqlGjRlc0DwAA0HC57n2wrrrqKmVnZ8vr9UqSfv/736tXr1413qZh6dKlTsQ7J94HCwAaBt4Hq/bwPlgOGz58eMTjBx980KEkAAAA5%2Ba6grVgwQKnIwAAAFyQ636LEAAAoL6jYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlUV%2Bw8vPzdcsttyghIUFJSUn60Y9%2BpL1790YcEwqFNHbsWLVs2VLx8fEaOHCgDh486FBiAABQ30V9wVq3bp3GjBmjzZs3q6CgQF999ZWysrJ04sSJ8DE5OTlatmyZFi9erI0bN%2Br48eMaMGCATp8%2B7WByAABQX8U4HcBpq1atini8YMECJSUlqaioSHfeeacCgYDmz5%2BvN998U3369JEkvfXWW0pLS9OaNWvUr18/J2IDAIB6LOqvYH1bIBCQJDVv3lySVFRUpOrqamVlZYWPSU1NVUZGhgoLCx3JCAAA6reov4J1NmOMxo0bp9tvv10ZGRmSpLKyMsXFxalZs2YRxyYnJ6usrOyc5wmFQgqFQuHHwWCw9kIDAIB6hytYZ3niiSf04Ycf6t///d8veqwxRh6P55z78vPz5fP5wltaWprtqAAAoB6jYP2fsWPH6r333tPatWvVunXr8Ljf71dVVZUqKioiji8vL1dycvI5z5Wbm6tAIBDeSkpKajU7AACoX6K%2BYBlj9MQTT2jp0qX64x//qPT09Ij9Xbp0UWxsrAoKCsJjpaWl2r17tzIzM895Tq/Xq8TExIgNAABEj6i/B2vMmDF6%2B%2B239Z//%2BZ9KSEgI31fl8/nUpEkT%2BXw%2BjRw5UuPHj1eLFi3UvHlzTZgwQe3btw//ViEAAMDZor5gzZ07V5LUs2fPiPEFCxZoxIgRkqRZs2YpJiZGgwcP1smTJ9W7d28tXLhQjRo1quO0AADADaK%2BYBljLnpM48aN9etf/1q//vWv6yARAABwu6i/BwsAAMA2ChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAsqgvWOvXr9fdd9%2Bt1NRUeTweLV%2B%2BPGK/MUZTpkxRamqqmjRpop49e2rPnj0OpQUAAG4Q9QXrxIkT6tixo%2BbMmXPO/dOnT9fMmTM1Z84cbd26VX6/X3379lVlZWUdJwUAAG4R43QAp2VnZys7O/uc%2B4wxmj17tiZPnqxBgwZJkhYtWqTk5GS9/fbbeuyxx%2BoyKgAAcImov4J1IcXFxSorK1NWVlZ4zOv1qkePHiosLDzv80KhkILBYMQGAACiBwXrAsrKyiRJycnJEePJycnhfeeSn58vn88X3tLS0mo1JwAAqF8oWJfA4/FEPDbG1Bg7W25urgKBQHgrKSmp7YgAAKAeifp7sC7E7/dL%2BvpKVkpKSni8vLy8xlWts3m9Xnm93lrPBwAA6ieuYF1Aenq6/H6/CgoKwmNVVVVat26dMjMzHUwGAADqs6i/gnX8%2BHF98skn4cfFxcXauXOnmjdvrmuvvVY5OTnKy8tTu3bt1K5dO%2BXl5alp06YaMmSIg6kBAEB9FvUFa9u2bfrhD38Yfjxu3DhJ0vDhw7Vw4UJNnDhRJ0%2Be1OOPP66Kigp169ZN77//vhISEpyKDAAA6jmPMcY4HaKhCwaD8vl8CgQCSkxMdDoOAOAydZ28yukIDda2F/tbP6eTP3%2B5BwsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACwjIIFAABgGQULAADAMgoWAACAZRQsAAAAyyhYAAAAllGwAAAALKNgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGBZjNMBAADRq%2BvkVU5HAGoFV7AAAAAso2ABAABYRsECAACwjHuwXM5N9y9se7G/0xEAAKgTXMECAACwjIIFAABgGQULAADAMgoWAACAZRSsS/TKK68oPT1djRs3VpcuXbRhwwanIwEAgHqKgnUJlixZopycHE2ePFk7duzQHXfcoezsbB04cMDpaAAAoB6iYF2CmTNnauTIkXr00Ud14403avbs2UpLS9PcuXOdjgYAAOoh3gfrIqqqqlRUVKRJkyZFjGdlZamwsPCczwmFQgqFQuHHgUBAkhQMBq3nOx06Yf2ctaU25g/A3dz0/zDUrtr4GfHNOY0x1s99MRSsizh69KhOnz6t5OTkiPHk5GSVlZWd8zn5%2BfmaOnVqjfG0tLRayegWvhlOJwAA1Fe1%2BTOisrJSPp%2Bv9r7AOVCwLpHH44l4bIypMfaN3NxcjRs3Lvz4zJkz%2Btvf/qYWLVqc9zmXIxgMKi0tTSUlJUpMTLR23vqIuTZMzLVhYq4NkxvnaoxRZWWlUlNT6/xrU7AuomXLlmrUqFGNq1Xl5eU1rmp9w%2Bv1yuv1RoxdffXVtZYxMTHRNd/sV4q5NkzMtWFirg2T2%2BZa11euvsFN7hcRFxenLl26qKCgIGK8oKBAmZmZDqUCAAD1GVewLsG4ceM0bNgwde3aVd27d9e8efN04MABjR492uloAACgHmo0ZcqUKU6HqO8yMjLUokUL5eXl6Ze//KVOnjypN998Ux07dnQ6mho1aqSePXsqJqbhd2Xm2jAx14aJuTZM0TTXK%2BUxTvzuIgAAQAPGPVgAAACWUbAAAAAso2ABAABYRsECAACwjILlUq%2B88orS09PVuHFjdenSRRs2bHA60hXLz8/XLbfcooSEBCUlJelHP/qR9u7dG3FMz5495fF4IrYHHnjAocSXb8qUKTXm4ff7w/uNMZoyZYpSU1PVpEkT9ezZU3v27HEw8eW77rrraszV4/FozJgxktwOp7cGAAAJxUlEQVS9puvXr9fdd9%2Bt1NRUeTweLV%2B%2BPGL/paxjRUWFhg0bJp/PJ5/Pp2HDhunYsWN1OY1LcqG5VldX65lnnlH79u0VHx%2Bv1NRUPfTQQzp06FDEOc71vfDtz3mtDy62riNGjKgxj1tvvTXimFAopLFjx6ply5aKj4/XwIEDdfDgwbqcxiW52FzP9XfX4/HopZdeCh/jlnWtaxQsF1qyZIlycnI0efJk7dixQ3fccYeys7N14MABp6NdkXXr1mnMmDHavHmzCgoK9NVXXykrK0snTkR%2BGOyoUaNUWloa3l599VWHEl%2BZm2%2B%2BOWIeu3btCu%2BbPn26Zs6cqTlz5mjr1q3y%2B/3q27evKisrHUx8ebZu3Roxz2/etPe%2B%2B%2B4LH%2BPWNT1x4oQ6duyoOXPmnHP/pazjkCFDtHPnTq1atUqrVq3Szp07NWzYsLqawiW70Fy//PJLbd%2B%2BXc8995y2b9%2BupUuX6uOPP9bAgQNrHPvzn/88Yq3/5V/%2BpS7ifycXW1dJ6t%2B/f8Q8Vq5cGbE/JydHy5Yt0%2BLFi7Vx40YdP35cAwYM0OnTp2s7/ndysbmePcfS0lK9/vrr8ng8%2BvGPfxxxnBvWtc4ZuM4//MM/mNGjR0eM3XDDDWbSpEkOJaod5eXlRpJZt25deKxHjx7mqaeecjCVHc8//7zp2LHjOfedOXPG%2BP1%2BM23atPDYqVOnjM/nM//2b/9WVxFrzVNPPWXatm1rzpw5Y4xpOGsqySxbtiz8%2BFLW8aOPPjKSzObNm8PHbNq0yUgy//M//1N34b%2Bjb8/1XLZs2WIkmc8%2B%2Byw81qZNGzNr1qzajmfVueY6fPhwc88995z3OceOHTOxsbFm8eLF4bHPP//cXHXVVWbVqlW1lvVKXcq63nPPPaZXr14RY25c17rAFSyXqaqqUlFRkbKysiLGs7KyVFhY6FCq2hEIBCRJzZs3jxj/3e9%2Bp5YtW%2Brmm2/WhAkTXHlVR5L27dun1NRUpaen64EHHtD%2B/fslScXFxSorK4tYY6/Xqx49erh%2BjauqqvTWW2/pkUceifjg84aypme7lHXctGmTfD6funXrFj7m1ltvlc/nc/1aBwIBeTyeGp/D%2Botf/EItWrTQD37wA7344ouqqqpyKOGV%2BeCDD5SUlKTrr79eo0aNUnl5eXhfUVGRqqurI9Y%2BNTVVGRkZrl7Xw4cPa8WKFRo5cmSNfQ1lXW3irVhd5ujRozp9%2BnSND5pOTk6u8YHUbmaM0bhx43T77bcrIyMjPD506FClp6fL7/dr9%2B7dys3N1Z///OcanxVZ33Xr1k1vvPGGrr/%2Beh0%2BfFgvvPCCMjMztWfPnvA6nmuNP/vsMyfiWrN8%2BXIdO3ZMI0aMCI81lDX9tktZx7KyMiUlJdV4blJSkqv/Pp86dUqTJk3SkCFDIj4U%2BKmnnlLnzp3VrFkzbdmyRbm5uSouLtZvf/tbB9N%2Bd9nZ2brvvvvUpk0bFRcX67nnnlOvXr1UVFQkr9ersrIyxcXFqVmzZhHPc/v/pxctWqSEhAQNGjQoYryhrKttFCyXOvtf/9LXheTbY272xBNP6MMPP9TGjRsjxkeNGhX%2Bc0ZGhtq1a6euXbtq%2B/bt6ty5c13HvGzZ2dnhP7dv317du3dX27ZttWjRovDNsg1xjefPn6/s7GylpqaGxxrKmp7PxdbxXGvq5rWurq7WAw88oDNnzuiVV16J2Pf000%2BH/9yhQwc1a9ZM9957b/jqh1vcf//94T9nZGSoa9euatOmjVasWFGjfJzNzesqSa%2B//rqGDh2qxo0bR4w3lHW1jZcIXaZly5Zq1KhRjX8FlZeX1/iXsluNHTtW7733ntauXavWrVtf8NjOnTsrNjZW%2B/btq6N0tSM%2BPl7t27fXvn37wr9N2NDW%2BLPPPtOaNWv06KOPXvC4hrKml7KOfr9fhw8frvHcI0eOuHKtq6urNXjwYBUXF6ugoCDi6tW5fPOPiU8%2B%2BaQu4tWalJQUtWnTJvw96/f7VVVVpYqKiojj3Px3eMOGDdq7d%2B9F//5KDWddrxQFy2Xi4uLUpUuXGi%2BfFBQUKDMz06FUdhhj9MQTT2jp0qX64x//qPT09Is%2BZ8%2BePaqurlZKSkodJKw9oVBIf/nLX5SSkhJ%2BuezsNa6qqtK6detcvcYLFixQUlKS7rrrrgse11DW9FLWsXv37goEAtqyZUv4mD/96U8KBAKuW%2BtvytW%2Bffu0Zs2aS7pysWPHDkly/Vp/8cUXKikpCc%2BjS5cuio2NjVj70tJS7d6923Xr%2Bo358%2BerS5cu6tix40WPbSjresUcvMEel2nx4sUmNjbWzJ8/33z00UcmJyfHxMfHm7/%2B9a9OR7siP/3pT43P5zMffPCBKS0tDW9ffvmlMcaYTz75xEydOtVs3brVFBcXmxUrVpgbbrjBdOrUyXz11VcOp/9uxo8fbz744AOzf/9%2Bs3nzZjNgwACTkJAQXsNp06YZn89nli5danbt2mV%2B8pOfmJSUFBMMBh1OfnlOnz5trr32WvPMM89EjLt9TSsrK82OHTvMjh07jCQzc%2BZMs2PHjvBvzl3KOvbv39906NDBbNq0yWzatMm0b9/eDBgwwKkpndeF5lpdXW0GDhxoWrdubXbu3Bnx9zcUChljjCksLAw/Z//%2B/WbJkiUmNTXVDBw40OGZ1XShuVZWVprx48ebwsJCU1xcbNauXWu6d%2B9urrnmmoh1HT16tGndurVZs2aN2b59u%2BnVq5fp2LFjvfu%2Bvtj3sDHGBAIB07RpUzN37twaz3fTutY1CpZL/eY3vzFt2rQxcXFxpnPnzhFvZeBWks65LViwwBhjzIEDB8ydd95pmjdvbuLi4kzbtm3Nk08%2Bab744gtng1%2BG%2B%2B%2B/36SkpJjY2FiTmppqBg0aZPbs2RPef%2BbMGfP8888bv99vvF6vufPOO82uXbscTHxlVq9ebSSZvXv3Roy7fU3Xrl17zu/Z4cOHG2MubR2/%2BOILM3ToUJOQkGASEhLM0KFDTUVFhQOzubALzbW4uPi8f3/Xrl1rjDGmqKjIdOvWzfh8PtO4cWPz/e9/3zz//PPmxIkTzk7sHC401y%2B//NJkZWWZVq1amdjYWHPttdea4cOHmwMHDkSc4%2BTJk%2BaJJ54wzZs3N02aNDEDBgyocUx9cLHvYWOMefXVV02TJk3MsWPHajzfTeta1zzGGFOrl8gAAACiDPdgAQAAWEbBAgAAsIyCBQAAYBkFCwAAwDIKFgAAgGUULAAAAMsoWAAAAJZRsAAAACyjYAEAAFhGwQIAALCMggUAAGAZBQsAAMAyChYAAIBlFCwAAADLKFgAAACWUbAAAAAso2ABAABYRsECAACw7H8BuFpEpHVnpAQAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common1138865514347627407">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">170.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">159.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">154.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">161.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">157.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">160.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">155.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">164.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:6%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">174.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">167.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (44)</td>
        <td class="number">76</td>
        <td class="number">65.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme1138865514347627407">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">142.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">144.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">148.0</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">149.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">180.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">182.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">183.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">184.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">185.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Hyperplasia"><s>Hyperplasia</s><br/>
            <small>Highly correlated</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is highly correlated with <a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Correlation</th>
            <td>0.90091</td>
        </tr>
    </table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_ID">ID<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>117</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>100.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Mean</th>
                    <td>61.778</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>1</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>120</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram3971479117883528351">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAa1JREFUeJzt3TGO01AUhtHriNZOH8W7YBHsiZqS/bCI2UWiLMAWBQV5FBAamF%2BMhJ5j6ZwyVnQd%2BX2ybpWhtdaqs%2Bv1WvM89x7Lzl0ulzqfz11nvus67ZdxHKvq5w%2BepmmLW2BHlmWpeZ5/n5ueNglkGIaqqpqm6b8E8v7jlzd/5%2BXThy5zeJv0XB7npqdD94mwIwKBQCAQbLKDPAP7BP/CGwQCgUAgEAiebgexG/BMvEEgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBIJN/uW2tVZVVcuy/HHt%2B7evvW%2BHJ/K3M/H47HFuetokkHVdq6pqnuctxvPEjp9fv7auax2Px343U1VD2yDL%2B/1et9utxnGsYRh6j2dnWmu1rmudTqc6HPpuBZsEAnthSYdAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAg%2BAFY1EZDsqyirAAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives3971479117883528351,#minihistogram3971479117883528351"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives3971479117883528351">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles3971479117883528351"
                                                  aria-controls="quantiles3971479117883528351" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram3971479117883528351" aria-controls="histogram3971479117883528351"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common3971479117883528351" aria-controls="common3971479117883528351"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme3971479117883528351" aria-controls="extreme3971479117883528351"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles3971479117883528351">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>1</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>8.8</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>33</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>62</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>91</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>114.2</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>120</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>119</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>58</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>34.234</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.55415</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.1874</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>61.778</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>29.472</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-0.020433</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>7228</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>1172</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram3971479117883528351">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3XtwVOX9x/HPksAGaLLchCQkYHBwQG4ioHJRARVFLrWMWuQW8FJQQEKqEMQLaCGISqlGsNCKTBFBp0DRKhoUAwwgkARBVEBBiEAMWNyEWwjk%2Bf3BsL%2BuARI7Dzl72PdrZmd6zp7d/fJ0IG/Pnux6jDFGAAAAsKaK0wMAAABcbggsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAywgsAAAAyyKdHiAclJaW6sCBA4qOjpbH43F6HAAAwoIxRkVFRYqPj1eVKpV7TonAqgQHDhxQYmKi02MAABCW8vLylJCQUKmvSWBVgujoaEln/w%2BOiYlxeBoAAMJDYWGhEhMTAz%2BHKxOBVQnOvS0YExNDYAEAUMmcuDyHi9wBAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsC/vAWr16tfr06aP4%2BHh5PB4tW7YscF9JSYnGjx%2BvVq1aqWbNmoqPj9eQIUN04MABBycGAAChLuwD69ixY2rTpo0yMjLK3Hf8%2BHHl5OTo6aefVk5OjpYsWaKdO3eqb9%2B%2BDkwKAADcwmOMMU4PESo8Ho%2BWLl2qu%2B%2B%2B%2B4LHbNq0Sddff7327t2rRo0aVeh5CwsL5fP55Pf7%2BaocAAAqiZM/f/kuwl/J7/fL4/GoVq1aFzymuLhYxcXFge3CwsLKGA0AAIQIAutXOHnypNLS0jRgwICLlnB6eromT55ciZO5Q/uJK5we4VfZPOVOp0f4Vdy2vgDw39z2b255wv4arIoqKSlR//79VVpaqlmzZl302AkTJsjv9wdueXl5lTQlAAAIBZzBqoCSkhLdd9992rNnjz799NNy38f1er3yer2VNB0AAAg1BFY5zsXVrl27tGrVKtWtW9fpkQAAQIgL%2B8A6evSovv3228D2nj17tGXLFtWpU0fx8fG65557lJOTo/fff19nzpxRfn6%2BJKlOnTqqVq2aU2MDAIAQFvaBtXnzZnXr1i2wnZqaKklKTk7WpEmTtHz5cknStddeG/S4VatWqWvXrpU2JwAAcI%2BwD6yuXbvqYh8FxseEAQCAX4vfIgQAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALAs0ukBgFDVfuIKp0cAALgUZ7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsI7AAAAAsC/vAWr16tfr06aP4%2BHh5PB4tW7Ys6H5jjCZNmqT4%2BHhVr15dXbt21fbt2x2aFgAAuEHYB9axY8fUpk0bZWRknPf%2B6dOna8aMGcrIyNCmTZsUGxur22%2B/XUVFRZU8KQAAcItIpwdwWs%2BePdWzZ8/z3meM0cyZMzVx4kT169dPkjR//nw1aNBACxcu1PDhwytzVAAA4BJhfwbrYvbs2aP8/Hz16NEjsM/r9eqWW27RunXrLvi44uJiFRYWBt0AAED4CPszWBeTn58vSWrQoEHQ/gYNGmjv3r0XfFx6eromT558SWc7p/3EFZXyOgAAoOI4g1UBHo8naNsYU2bff5swYYL8fn/glpeXd6lHBAAAIYQzWBcRGxsr6eyZrLi4uMD%2BgoKCMme1/pvX65XX673k8wEAgNDEGayLSEpKUmxsrDIzMwP7Tp06paysLHXq1MnByQAAQCgL%2BzNYR48e1bfffhvY3rNnj7Zs2aI6deqoUaNGSklJ0dSpU9W0aVM1bdpUU6dOVY0aNTRgwAAHpwYAAKEs7ANr8%2BbN6tatW2A7NTVVkpScnKw333xT48aN04kTJ/Too4/qyJEjuuGGG/Txxx8rOjraqZEBAECI8xhjjNNDXO4KCwvl8/nk9/sVExNj9bn5LUIAwOVg85Q7rT/npfz5Wx6uwQIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwAIAALCMwCrH6dOn9dRTTykpKUnVq1dXkyZN9Nxzz6m0tNTp0QAAQIiKdHqAUPfCCy/o9ddf1/z589WiRQtt3rxZw4YNk8/n05gxY5weDwAAhCACqxzr16/Xb3/7W/Xq1UuSdOWVV%2Brtt9/W5s2bHZ4MAACEKt4iLEeXLl30ySefaOfOnZKkL774QmvXrtVdd93l8GQAACBUcQarHOPHj5ff71ezZs0UERGhM2fOaMqUKbr//vsv%2BJji4mIVFxcHtgsLCytjVAAAECI4g1WOxYsXa8GCBVq4cKFycnI0f/58vfTSS5o/f/4FH5Oeni6fzxe4JSYmVuLEAADAaR5jjHF6iFCWmJiotLQ0jRw5MrDvT3/6kxYsWKBvvvnmvI853xmsxMRE%2Bf1%2BxcTEWJ2v/cQVVp8PAAAnbJ5yp/XnLCwslM/nuyQ/f8vDW4TlOH78uKpUCT7RFxERcdGPafB6vfJ6vZd6NAAAEKIIrHL06dNHU6ZMUaNGjdSiRQvl5uZqxowZeuCBB5weDQAAhCgCqxyvvvqqnn76aT366KMqKChQfHy8hg8frmeeecbp0QAAQIgisMoRHR2tmTNnaubMmU6PAgAAXILfIgQAALCMwAIAALCMwAIAALDMtYG1YMECnTx50ukxAAAAynBtYKWmpio2NlbDhw/Xxo0bnR4HAAAgwLWBdeDAAb3xxhs6ePCgunTpohYtWujll1/WoUOHnB4NAACEOdcGVmRkpPr166fly5dr3759Sk5O1htvvKGEhAT169dP//73v8W3AAEAACe4NrD%2BW2xsrG699VZ17dpVHo9Hmzdv1oABA9S0aVOtWbPG6fEAAECYcXVgHT58WDNnzlSbNm3UuXNnFRQUaNmyZdq7d6/279%2Bv3r17a8iQIU6PCQAAwoxrP8n9d7/7nT744AMlJSXpoYceUnJysq644orA/b/5zW80btw4vfLKKw5OCQAAwpFrAysmJkYrV67UTTfddMFj4uLitGvXrkqcCgAAwMWBNX/%2B/HKP8Xg8uuqqqyphGgAAgP/n2muwxo4dq4yMjDL7X3vtNf3xj390YCIAAICzXBtY7777rm688cYy%2Bzt27KjFixc7MBEAAMBZrg2sw4cPq3bt2mX2x8TE6PDhww5MBAAAcJZrA%2Buqq67SRx99VGb/Rx99pKSkJAcmAgAAOMu1F7mnpKQoJSVFP/30k7p37y5J%2BuSTTzR9%2BnS99NJLDk8HAADCmWsD6%2BGHH9bJkyc1depUPfvss5KkhIQEvfLKK3rggQccng4AAIQz1waWJI0ePVqjR4/WwYMHVb16ddWqVcvpkQAAANwdWOfExcU5PQIAAECAay9yP3TokIYNG6ZGjRopKipK1apVC7oBAAA4xbVnsIYOHarvvvtOTzzxhOLi4uTxeJweCQAAQJKLA2v16tVavXq12rZt6/QoAAAAQVz7FmFCQgJnrQAAQEhybWD9%2Bc9/1oQJE/TDDz84PQoAAEAQ175FOHjwYBUVFalx48aKiYlR1apVg%2B4vKChwaDIAABDuXBtY06ZNc3oEAACA83JtYD344INOjwAAAHBerr0GS5K%2B//57TZo0SYMHDw68Jfjxxx/r66%2B/dngyAAAQzlwbWGvWrFGLFi2UlZWld955R0ePHpUk5eTk6JlnnnF4OgAAEM5cG1jjx4/XpEmTtGrVqqBPbu/evbs2bNjg4GQAACDcuTawtm7dqnvuuafM/vr16%2BvQoUMOTAQAAHCWawOrVq1ays/PL7N/y5YtatiwoQMTAQAAnOXawOrfv7/S0tJ06NChwCe6f/7553r88cc1aNAgh6cDAADhzLWBNXXqVMXGxiouLk5Hjx7VNddco06dOqlDhw56%2BumnnR4PAACEMdd%2BDla1atW0ePFi7dy5Uzk5OSotLdV1112nZs2aOT0aAAAIc64NrHOuvvpqXX311U6PAQAAEODawPrDH/5w0fvnzJlTSZMAAAAEc21gHTx4MGi7pKRE27dvV1FRkW6%2B%2BWaHpgIAAHBxYL333ntl9p0%2BfVqPPPKImjdv7sBEAAAAZ7n2twjPJzIyUo8//rhefPFFq8%2B7f/9%2BDRo0SHXr1lWNGjV07bXXKjs72%2BprAACAy4drz2BdyO7du1VSUmLt%2BY4cOaLOnTurW7du%2BvDDD1W/fn199913qlWrlrXXAAAAlxfXBta4ceOCto0xOnjwoJYvX66BAwdae50XXnhBiYmJmjdvXmDflVdeae35AQDA5ce1gbV%2B/fqg7SpVquiKK67QtGnT9PDDD1t7neXLl%2BuOO%2B7Qvffeq6ysLDVs2FCPPvqo1dcAAACXF9cG1po1ayrldXbv3q3Zs2crNTVVTz75pDZu3KjHHntMXq9XQ4YMOe9jiouLVVxcHNguLCyslFkBAEBocG1gVZbS0lK1b99eU6dOlSS1bdtW27dv1%2BzZsy8YWOnp6Zo8eXJljgkAAEKIawOrQ4cOgS95Ls/GjRv/59eJi4vTNddcE7SvefPm%2Buc//3nBx0yYMEGpqamB7cLCQiUmJv7PMwAAAHdxbWB169ZNf/3rX3X11VerY8eOkqQNGzZox44dGj58uLxer5XX6dy5s3bs2BG0b%2BfOnWrcuPEFH%2BP1eq29PgAAcB/XBtbPP/%2BskSNHBt66O2fixIn68ccf9be//c3K64wdO1adOnXS1KlTdd9992njxo2aM2cOX8UDAAAuyLUfNPrOO%2B9o2LBhZfYPHTpU7777rrXX6dChg5YuXaq3335bLVu21PPPP6%2BZM2da/SgIAABweXHtGSyv16t169apadOmQfvXrVtn/e253r17q3fv3lafEwAAXL5cG1iPPfaYRowYodzcXN14442Szl6DNXfuXD355JMOTwcAAMKZawNr4sSJSkpK0l/%2B8he98cYbks7%2Bdt/cuXM1YMAAh6cDAADhzLWBJUkDBgwgpgAAQMhx7UXu0tnPl3rzzTf1zDPP6MiRI5KkL774QgcPHnR4MgAAEM5cewbryy%2B/1G233aYaNWooLy9PQ4cOVe3atfXOO%2B/ohx9%2B0Pz5850eEQAAhCnXnsEaO3asBgwYoO%2B%2B%2B05RUVGB/b169dLq1asdnAwAAIQ7157B2rRpk2bPnl3m63IaNmzIW4QAAMBRrj2DVa1aNR09erTM/l27dqlevXoOTAQAAHCWawOrb9%2B%2Bev7553X69GlJksfj0f79%2B5WWlqZ%2B/fo5PB0AAAhnrg2sl19%2BWQcOHFBsbKxOnDih7t27q0mTJoqKiirz/YQAAACVybXXYPl8Pq1bt06ZmZnKyclRaWmprrvuOt1xxx1lrssCAACoTK4MrJKSEt11112aNWuWevTooR49ejg9EgAAQIAr3yKsWrWqcnNzOVMFAABCkisDS5IGDRqkefPmOT0GAABAGa58i/CcjIwMrVy5Uu3bt1fNmjWD7ps%2BfbpDUwEAgHDn2sDKzs5W69atJUlbt24Nuo%2B3DgEAgJNcF1i7d%2B9WUlKS1qxZ4/QoAAAA5%2BW6a7CaNm2qQ4cOBbZ///vf68cff3RwIgAAgGCuCyxjTND2Bx98oGPHjjk0DQAAQFmuCywAAIBQ57rA8ng8ZS5i56J2AAAQSlx3kbsxRkOHDpXX65UknTx5UiNGjCjzMQ1LlixxYjwAAAD3BVZycnLQ9qBBgxyaBAAA4PxcF1h8ejsAAAh1rrsGCwAAINQRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWAAAAJYRWL9Senq6PB6PUlJSnB4FAACEKALrV9i0aZPmzJmj1q1bOz0KAAAIYQRWBR09elQDBw7U3LlzVbt2bafHAQAAIYzAqqCRI0eqV69euu2228o9tri4WIWFhUE3AAAQPiKdHsANFi1apOzsbG3evLlCx6enp2vy5MmXeCoAABCqOINVjry8PI0ZM0ZvvfWWoqKiKvSYCRMmyO/3B255eXmXeEoAABBKOINVjuzsbBUUFKhdu3aBfWfOnNHq1auVkZGh4uJiRUREBD3G6/XK6/VW9qgAACBEEFjluPXWW7Vt27agfcOGDVOzZs00fvz4MnEFAABAYJUjOjpaLVu2DNpXs2ZN1a1bt8x%2BAAAAiWuwAAAArOMM1v/gs88%2Bc3oEAAAQwjiDBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBBQAAYBmBVQHp6enq0KGDoqOjVb9%2Bfd19993asWOH02MBAIAQRWBVQFZWlkaOHKkNGzYoMzNTp0%2BfVo8ePXTs2DGnRwMAACEo0ukB3GDFihVB2/PmzVP9%2BvWVnZ2tm2%2B%2B2aGpAABAqCKw/gd%2Bv1%2BSVKdOnfPeX1xcrOLi4sB2YWFhpcwFAABCA28R/krGGKWmpqpLly5q2bLleY9JT0%2BXz%2BcL3BITEyt5SgAA4CQC61caNWqUtm7dqrfffvuCx0yYMEF%2Bvz9wy8vLq8QJAQCA03iL8FcYPXq0li9frtWrVyshIeGCx3m9Xnm93kqcDAAAhBICqwKMMRo9erSWLl2qzz77TElJSU6PBAAAQhiBVQEjR47UwoUL9a9//UvR0dHKz8%2BXJPl8PlWvXt3h6QAAQKjhGqwKmD17tvx%2Bv7p27aq4uLjAbfHixU6PBgAAQhBnsCrAGOP0CAAAwEU4gwUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgQUAAGAZgVVBs2bNUlJSkqKiotSuXTutWbPG6ZEAAECIIrAqYPHixUpJSdHEiROVm5urm266ST179tS%2BffucHg0AAIQgAqsCZsyYoQcffFAPPfSQmjdvrpkzZyoxMVGzZ892ejQAABCCIp0eINSdOnVK2dnZSktLC9rfo0cPrVu37ryPKS4uVnFxcWDb7/dLkgoLC63Pd6b4mPXnBACgsl2Kn5HnntMYY/25y0NglePw4cM6c%2BaMGjRoELS/QYMGys/PP%2B9j0tPTNXny5DL7ExMTL8mMAAC4ne/lS/fcRUVF8vl8l%2B4FzoPAqiCPxxO0bYwps%2B%2BcCRMmKDU1NbBdWlqq//znP6pbt%2B4FH1MRhYWFSkxMVF5enmJiYv7n57mcsUYVwzqVjzWqGNapfKxR%2BS7VGhljVFRUpPj4eGvPWVEEVjnq1auniIiIMmerCgoKypzVOsfr9crr9Qbtq1WrlrWZYmJi%2BEtaDtaoYlin8rFGFcM6lY81Kt%2BlWKPKPnN1Dhe5l6NatWpq166dMjMzg/ZnZmaqU6dODk0FAABCGWewKiA1NVWDBw9W%2B/bt1bFjR82ZM0f79u3TiBEjnB4NAACEoIhJkyZNcnqIUNeyZUvVrVtXU6dO1UsvvaQTJ07oH//4h9q0aVPps0RERKhr166KjKSNL4Q1qhjWqXysUcWwTuVjjcp3ua2Rxzjxu4sAAACXMa7BAgAAsIzAAgAAsIzAAgAAsIzAAgAAsIzAcpFZs2YpKSlJUVFRateundasWeP0SI5JT09Xhw4dFB0drfr16%2Bvuu%2B/Wjh07go4pLi7W6NGjVa9ePdWsWVN9%2B/bVDz/84NDEzktPT5fH41FKSkpgH2sk7d%2B/X4MGDVLdunVVo0YNXXvttcrOzg7cb4zRpEmTFB8fr%2BrVq6tr167avn27gxNXvtOnT%2Bupp55SUlKSqlevriZNmui5555TaWlp4JhwW6fVq1erT58%2Bio%2BPl8fj0bJly4Lur8h6HDlyRIMHD5bP55PP59PgwYP1888/V%2BYf45K72DqVlJRo/PjxatWqlWrWrKn4%2BHgNGTJEBw4cCHoOt64TgeUSixcvVkpKiiZOnKjc3FzddNNN6tmzp/bt2%2Bf0aI7IysrSyJEjtWHDBmVmZur06dPq0aOHjh37/y%2B/TklJ0dKlS7Vo0SKtXbtWR48eVe/evXXmzBkHJ3fGpk2bNGfOHLVu3Tpof7iv0ZEjR9S5c2dVrVpVH374ob766iu9/PLLQd%2B8MH36dM2YMUMZGRnatGmTYmNjdfvtt6uoqMjBySvXCy%2B8oNdff10ZGRn6%2BuuvNX36dL344ot69dVXA8eE2zodO3ZMbdq0UUZGxnnvr8h6DBgwQFu2bNGKFSu0YsUKbdmyRYMHD66sP0KluNg6HT9%2BXDk5OXr66aeVk5OjJUuWaOfOnerbt2/Qca5dJwNXuP76682IESOC9jVr1sykpaU5NFFoKSgoMJJMVlaWMcaYn3/%2B2VStWtUsWrQocMz%2B/ftNlSpVzIoVK5wa0xFFRUWmadOmJjMz09xyyy1mzJgxxhjWyBhjxo8fb7p06XLB%2B0tLS01sbKyZNm1aYN/JkyeNz%2Bczr7/%2BemWMGBJ69eplHnjggaB9/fr1M4MGDTLGsE6SzNKlSwPbFVmPr776ykgyGzZsCByzfv16I8l88803lTd8JfrlOp3Pxo0bjSSzd%2B9eY4y714kzWC5w6tQpZWdnq0ePHkH7e/TooXXr1jk0VWjx%2B/2SpDp16kiSsrOzVVJSErRm8fHxatmyZdit2ciRI9WrVy/ddtttQftZI2n58uVq37697r33XtWvX19t27bV3LlzA/fv2bNH%2Bfn5QWvk9Xp1yy23hM0aSVKXLl30ySefaOfOnZKkL774QmvXrtVdd90liXX6pYqsx/r16%2BXz%2BXTDDTcEjrnxxhvl8/nCcs3O8fv98ng8gbPIbl6ny%2BPjUi9zhw8f1pkzZ8p8uXSDBg3KfAl1ODLGKDU1VV26dFHLli0lSfn5%2BapWrZpq164ddGy4rdmiRYuUnZ2tzZs3l7mPNZJ2796t2bNnKzU1VU8%2B%2BaQ2btyoxx57TF6vV0OGDAmsw/n%2B7u3du9eJkR0xfvx4%2Bf1%2BNWvWTBERETpz5oymTJmi%2B%2B%2B/X5JYp1%2BoyHrk5%2Berfv36ZR5bv379sPn790snT55UWlqaBgwYEPjCZzevE4HlIh6PJ2jbGFNmXzgaNWqUtm7dqrVr15Z7bDitWV5ensaMGaOPP/5YUVFRFX5cOK1RaWmp2rdvr6lTp0qS2rZtq%2B3bt2v27NkaMmRI4Lhw/7u3ePFiLViwQAsXLlSLFi20ZcsWpaSkKD4%2BXsnJyYHjwn2dfqm89Tjf2oTrmpWUlKh///4qLS3VrFmzgu5z6zrxFqEL1KtXTxEREWVqvaCgoMx/IYWb0aNHa/ny5Vq1apUSEhIC%2B2NjY3Xq1CkdOXIk6PhwWrPs7GwVFBSoXbt2ioyMVGRkpLKysvTKK68oMjJSDRo0CPs1iouL0zXXXBO0r3nz5oFfHomNjZWksP%2B798QTTygtLU39%2B/dXq1atNHjwYI0dO1bp6emSWKdfqsh6xMbG6scffyzz2EOHDoXdmpWUlOi%2B%2B%2B7Tnj17lJmZGTh7Jbl7nQgsF6hWrZratWunzMzMoP2ZmZnq1KmTQ1M5yxijUaNGacmSJfr000%2BVlJQUdH%2B7du1UtWrVoDU7ePCgvvzyy7BZs1tvvVXbtm3Tli1bArf27dtr4MCBgf8d7mvUuXPnMh/vsXPnTjVu3FiSlJSUpNjY2KA1OnXqlLKyssJmjaSzv%2B1VpUrwj4uIiIjAxzSwTsEqsh4dO3aU3%2B/Xxo0bA8d8/vnn8vv9YbVm5%2BJq165dWrlyperWrRt0v6vXyamr6/HrLFq0yFStWtX8/e9/N1999ZVJSUkxNWvWNN9//73ToznikUceMT6fz3z22Wfm4MGDgdvx48cDx4wYMcIkJCSYlStXmpycHNO9e3fTpk0bc/r0aQcnd9Z//xahMazRxo0bTWRkpJnOppvsAAAB8klEQVQyZYrZtWuXeeutt0yNGjXMggULAsdMmzbN%2BHw%2Bs2TJErNt2zZz//33m7i4OFNYWOjg5JUrOTnZNGzY0Lz//vtmz549ZsmSJaZevXpm3LhxgWPCbZ2KiopMbm6uyc3NNZLMjBkzTG5ubuC33yqyHnfeeadp3bq1Wb9%2BvVm/fr1p1aqV6d27t1N/pEviYutUUlJi%2BvbtaxISEsyWLVuC/i0vLi4OPIdb14nAcpHXXnvNNG7c2FSrVs1cd911gY8kCEeSznubN29e4JgTJ06YUaNGmTp16pjq1aub3r17m3379jk3dAj4ZWCxRsa89957pmXLlsbr9ZpmzZqZOXPmBN1fWlpqnn32WRMbG2u8Xq%2B5%2BeabzbZt2xya1hmFhYVmzJgxplGjRiYqKso0adLETJw4MeiHYLit06pVq877b1BycrIxpmLr8dNPP5mBAwea6OhoEx0dbQYOHGiOHDniwJ/m0rnYOu3Zs%2BeC/5avWrUq8BxuXSePMcZU3vkyAACAyx/XYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFhGYAEAAFj2f7nb%2BggXaRCIAAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common3971479117883528351">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">120</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">46</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">34</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">35</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">36</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">37</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">38</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">39</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">40</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">41</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (107)</td>
        <td class="number">107</td>
        <td class="number">91.5%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme3971479117883528351">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">1</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">116</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">117</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">118</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">119</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">120</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Incisor 2nd largest age"><s>Incisor 2nd largest age</s><br/>
            <small>Highly correlated</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is highly correlated with <a href="#pp_var_Incisor largest age"><code>Incisor largest age</code></a> and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Correlation</th>
            <td>0.90384</td>
        </tr>
    </table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Incisor largest age">Incisor largest age<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>7</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>6.0%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>35.9%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>42</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Mean</th>
                    <td>1.24</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>4</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>35.9%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram-4332127071032318910">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAcdJREFUeJzt3TGO01AUhtHriNZewMheBBJbQGJBdEh0LIeelp5d2MoCHFFQEE8BoQn6NRmN7BjOKRPF15b9yXnVa5ZlWWpl0zTVMAxrj2XnxnGsvu9Xnflq1Wm/tW1bVb8uuOu6LU6BHZnnuYZh%2BPPcrGmTQJqmqaqqruuuAnnz4cvNx/v26d2LnBf37fLcrOmw%2BkTYEYFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgWCT/UF4WbfuqWI/lafzBoFAIBAIBAKBQGCRzpP8r5ureoNAIBAIBAKBNcidec5//Xuc8a/wBoFAIBAIBAKBQLDJIn1Zlqqqmuf56rufP77ffLy/HWevnnP99%2Br1%2B883/%2Bbrx7dXn13u7%2BW5WVOzbDB1mqYahmHtsezcOI7V9/2qMzcJ5Hw%2B1/F4rLZtq2matcezM8uy1Ol0qoeHhzoc1l0VbBII7IVFOgQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBALBIyGfUi0Y1865AAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives-4332127071032318910,#minihistogram-4332127071032318910"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives-4332127071032318910">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles-4332127071032318910"
                                                  aria-controls="quantiles-4332127071032318910" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram-4332127071032318910" aria-controls="histogram-4332127071032318910"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common-4332127071032318910" aria-controls="common-4332127071032318910"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme-4332127071032318910" aria-controls="extreme-4332127071032318910"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles-4332127071032318910">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>2.5</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>3.5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>4</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>4</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>2.5</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>1.4574</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>1.1753</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.5842</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>1.24</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.3888</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.44981</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>93</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>2.1241</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram-4332127071032318910">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzt3Xt0VeWd//HPMZcTwCQSArk0kUkVQQiwgDAlXBQBA1G8lLqAAZloKSMW0BgYS2BNJ8yMhLEiotRM8Qa12NAOQnGJSFBJYBBLYpCbQyliCSUh0MGchMHDxf37w%2BH8PCYhlz7JPjvn/Vprr8V%2B9rMfvg97bfPx2TvnuCzLsgQAAABjrrO7AAAAgI6GgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhoXaXUAw%2BOqrr3Tq1ClFRkbK5XLZXQ4AAEHBsizV1tYqMTFR113XvmtKBKx2cOrUKSUnJ9tdBgAAQamiokJJSUnt%2BncSsNpBZGSkpK8vcFRUlM3VAAAQHDwej5KTk30/h9sTAasdXH0sGBUVRcACAKCd2fF6Di%2B5AwAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwvuzZ4dIWb7W7hGYrfWqC3SUAANAuWMECAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyA9Q35%2BflyuVzKzs72tXm9Xs2bN0%2BxsbHq0qWL7r33Xp08edLGKgEAQKAjYP2fvXv3avXq1RowYIBfe3Z2tjZu3KjCwkLt2rVLdXV1mjhxoq5cuWJTpQAAINARsCTV1dVp%2BvTpeumll9S1a1dfe01NjV555RUtX75c48aN06BBg/SrX/1KBw4c0Pbt222sGAAABDIClqQ5c%2Bbo7rvv1rhx4/zay8rKdOnSJWVkZPjaEhMTlZqaqt27d7d3mQAAwCFC7S7AboWFhSorK1NpaWm9Y1VVVQoPD/db1ZKkuLg4VVVVNTqm1%2BuV1%2Bv17Xs8HnMFAwCAgBfUK1gVFRV6/PHHtW7dOkVERDT7PMuy5HK5Gj2en5%2Bv6Oho35acnGyiXAAA4BBBHbDKyspUXV2tIUOGKDQ0VKGhoSouLtbzzz%2Bv0NBQxcXF6eLFizp37pzfedXV1YqLi2t03NzcXNXU1Pi2ioqKtp4KAAAIIEH9iHDs2LE6cOCAX9vDDz%2BsPn366Cc/%2BYmSk5MVFhamoqIiTZ48WZJUWVmpgwcP6umnn250XLfbLbfb3aa1AwCAwBXUASsyMlKpqal%2BbV26dFG3bt187TNnztT8%2BfPVrVs3xcTEaMGCBerfv3%2B9F%2BIBAACuCuqA1RwrVqxQaGioJk%2BerAsXLmjs2LFas2aNQkJC7C4NAAAEKJdlWZbdRXR0Ho9H0dHRqqmpUVRUlNGx0xZvNTpeWyp9aoLdJQAAgkhb/vxtSlC/5A4AANAWCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGBX3AKigo0IABAxQVFaWoqCilp6frnXfe8R0fPXq0XC6X3zZ16lQbKwYAAIEu1O4C7JaUlKRly5bp5ptvliStXbtW9913n8rLy9WvXz9J0qxZs/Qv//IvvnM6depkS60AAMAZgj5g3XPPPX77Tz31lAoKCrRnzx5fwOrcubPi4%2BPtKA8AADhQ0D8i/KYrV66osLBQ58%2BfV3p6uq993bp1io2NVb9%2B/bRgwQLV1tZecxyv1yuPx%2BO3AQCA4BH0K1iSdODAAaWnp%2BvLL7/U9ddfr40bN6pv376SpOnTpyslJUXx8fE6ePCgcnNz9cknn6ioqKjR8fLz87VkyZL2Kh8AAAQYl2VZlt1F2O3ixYs6ceKEvvjiC23YsEEvv/yyiouLfSHrm8rKypSWlqaysjINHjy4wfG8Xq%2B8Xq9v3%2BPxKDk5WTU1NYqKijJae9rirUbHa0ulT02wuwQAQBDxeDyKjo5uk5%2B/TWEFS1J4eLjvJfe0tDTt3btXK1eu1C9%2B8Yt6fQcPHqywsDAdPXq00YDldrvldrvbtGYAABC4eAerAZZl%2Ba1AfdOhQ4d06dIlJSQktHNVAADAKYJ%2BBWvRokXKzMxUcnKyamtrVVhYqB07dmjr1q06duyY1q1bp7vuukuxsbE6fPiw5s%2Bfr0GDBmnEiBF2lw4AAAJU0Aes06dPa8aMGaqsrFR0dLQGDBigrVu36s4771RFRYXee%2B89rVy5UnV1dUpOTtbdd9%2Btf/7nf1ZISIjdpQMAgAAV9AHrlVdeafRYcnKyiouL27EaAADQEfAOFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGFBH7AKCgo0YMAARUVFKSoqSunp6XrnnXd8x71er%2BbNm6fY2Fh16dJF9957r06ePGljxQAAINAFfcBKSkrSsmXLVFpaqtLSUo0ZM0b33XefDh06JEnKzs7Wxo0bVVhYqF27dqmurk4TJ07UlStXbK4cAAAEKpdlWZbdRQSamJgY/exnP9MDDzyg7t276/XXX9eUKVMkSadOnVJycrK2bNmi8ePHN2s8j8ej6Oho1dTUKCoqymitaYu3Gh2vLZU%2BNcHuEgAAQaQtf/42JehXsL7pypUrKiws1Pnz55Wenq6ysjJdunRJGRkZvj6JiYlKTU3V7t27Gx3H6/XK4/H4bQAAIHgQsCQdOHBA119/vdxut2bPnq2NGzeqb9%2B%2BqqqqUnh4uLp27erXPy4uTlVVVY2Ol5%2Bfr%2BjoaN%2BWnJzc1lMAAAABhIAlqXfv3tq3b5/27NmjRx99VFlZWTp8%2BHCj/S3LksvlavR4bm6uampqfFtFRUVblA0AAAJUqN0FBILw8HDdfPPNkqS0tDTt3btXK1eu1JQpU3Tx4kWdO3fObxWrurpaw4cPb3Q8t9stt9vd5nUDAIDAxApWAyzLktfr1ZAhQxQWFqaioiLfscrKSh08ePCaAQsAAAS3oF/BWrRokTIzM5WcnKza2loVFhZqx44d2rp1q6KjozVz5kzNnz9f3bp1U0xMjBYsWKD%2B/ftr3LhxdpcOAAACVNAHrNOnT2vGjBmqrKxUdHS0BgwYoK1bt%2BrOO%2B%2BUJK1YsUKhoaGaPHmyLly4oLFjx2rNmjUKCQmxuXIAABCo%2BBysdsDnYH2Nz8ECALQnPgcLAACgAyFgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMc2zA%2BtWvfqUvv/zS7jIAAADqcWzAysnJUXx8vB555BH9/ve/t7scAAAAH8cGrFOnTunVV19VZWWlRo4cqX79%2Bmn58uU6c%2BaM3aUBAIAg59iAFRoaqkmTJmnz5s06ceKEsrKy9OqrryopKUmTJk3S22%2B/Lcuy7C4TAAAEIccGrG%2BKj4/X2LFjNXr0aLlcLpWWlmratGnq1auXdu7caXd5AAAgyDg6YJ09e1bPPfecBg4cqBEjRqi6ulqbNm3Sn/70J/35z3/WxIkT9fd///d2lwkAAIJMqN0FtNb3v/99bdmyRSkpKfrRj36krKwsde/e3Xf8%2Buuv15NPPqnnn3/exioBAEAwcmzAioqK0vbt2zVq1KhG%2ByQkJOjo0aPtWBUAAICDHxGuXbv2muFKklwul2666aZr9snPz9fQoUMVGRmpHj166P7779eRI0f8%2Blx9t%2Bub29SpU//qOQAAgI7JsQHriSee0KpVq%2Bq1//znP9f8%2BfObPU5xcbHmzJmjPXv2qKioSJcvX1ZGRobOnz/v12/WrFmqrKz0bb/4xS/%2B6jkAAICOybGPCH/7299q06ZN9drT09OVn5%2Bv5cuXN2ucrVu3%2Bu2/9tpr6tGjh8rKynTbbbf52jt37qz4%2BPi/rmgAABAUHLuCdfbsWXXt2rVee1RUlM6ePdvqcWtqaiRJMTExfu3r1q1TbGys%2BvXrpwULFqi2trbVfwcAAOjYHLuCddNNN%2Bndd9/Vj3/8Y7/2d999VykpKa0a07Is5eTkaOTIkUpNTfW1T58%2BXSkpKYqPj9fBgweVm5urTz75REVFRQ2O4/V65fV6ffsej6dV9QAAAGdybMDKzs5Wdna2/vKXv2jMmDGSpPfee09PP/20nnnmmVaNOXfuXO3fv1%2B7du3ya581a5bvz6mpqerVq5fS0tL08ccfa/DgwfXGyc/P15IlS1pVAwAAcD6X5eDvk3nhhRe0dOlSnT59WpKUlJSkvLw8/fCHP2zxWPPmzdOmTZtUUlLS5AqYZVlyu916/fXXNWXKlHrHG1rBSk5OVk1NjaKiolpc27WkLd7adKcAUfrUBLtLAAAEEY/Ho%2Bjo6Db5%2BdsUx65gSV%2BHonnz5qmyslKdOnXSDTfc0OIxLMvSvHnztHHjRu3YsaNZjxcPHTqkS5cuKSEhocHjbrdbbre7xbUAAICOwdEB66rGgk5zzJkzR2%2B88YZ%2B97vfKTIyUlVVVZKk6OhoderUSceOHdO6det01113KTY2VocPH9b8%2BfM1aNAgjRgxwtQUAABAB%2BLY3yI8c%2BaMHn74Yd14442KiIhQeHi439ZcBQUFqqmp0ejRo5WQkODb1q9fL0kKDw/Xe%2B%2B9p/Hjx6t379567LHHlJGRoe3btyskJKStpgcAABzMsStYDz30kI4dO6Z//Md/VEJCglwuV6vGaeoVtOTkZBUXF7dqbAAAEJwcG7BKSkpUUlKiQYMG2V0KAACAH8c%2BIkxKSmr1qhUAAEBbcmzAWrFihXJzc3Xy5Em7SwEAAPDj2EeEM2bMUG1trXr27KmoqCiFhYX5Ha%2BurrapMgAAEOwcG7CWLVtmdwkAAAANcmzAmjlzpt0lAAAANMix72BJ0ueff668vDzNmDHD90hw27Zt%2BvTTT22uDAAABDPHBqydO3eqX79%2BKi4u1m9%2B8xvV1dVJkj7%2B%2BGP99Kc/tbk6AAAQzBwbsH7yk58oLy9PH3zwgd8nt48ZM0Z79uyxsTIAABDsHBuw9u/frwceeKBee48ePXTmzBkbKgIAAPiaYwPWDTfc4Pti5m/at2%2BfvvOd79hQEQAAwNccG7CmTp2qhQsX6syZM75PdP/oo4%2B0YMECPfjggzZXBwAAgpljA9bSpUsVHx%2BvhIQE1dXVqW/fvho%2BfLiGDh2qf/qnf7K7PAAAEMQc%2BzlY4eHhWr9%2Bvf7whz/o448/1ldffaXBgwerT58%2BdpcGAACCnGMD1lW33HKLbrnlFrvLAAAA8HFswPqHf/iHax5fvXp1O1UCAADgz7EBq7Ky0m//0qVLOnTokGpra3XbbbfZVBUAAICDA9Zbb71Vr%2B3y5ct69NFHdeutt9pQEQAAwNcc%2B1uEDQkNDdWCBQv0s5/9zO5SAABAEOtQAUuSPvvsM126dMnuMgAAQBBz7CPCJ5980m/fsixVVlZq8%2BbNmj59uk1VAQAAODhgffjhh3771113nbp3765ly5Zp1qxZNlUFAADg4IC1c%2BdOu0sAAABoUId7BwsAAMBujl3BGjp0qO9Lnpvy%2B9//vo2rAQAA%2BP8cu4J1xx136MiRI7IsS8OGDdOwYcMkSUeOHNHo0aM1fvx433Yt%2Bfn5Gjp0qCIjI9WjRw/df//9OnLkiF8fr9erefPmKTY2Vl26dNG9996rkydPttncAACAszl2BeuLL77QnDlztHTpUr/2xYsX6/Tp03r55ZebNU5xcbHmzJmjoUOH6vLly1q8eLEyMjJ0%2BPBhdenSRZKUnZ2tt956S4WFherWrZvmz5%2BviRMnqqysTCEhIcbnBgAAnM1lWZZldxGtccMNN2jv3r3q1auXX/vRo0eVlpammpqaVo175swZ9ejRQ8XFxbrttttUU1Oj7t276/XXX9eUKVMkSadOnVJycrK2bNnS5AqZJHk8HkVHR6umpkZRUVGtqqsxaYu3Gh2vLZU%2BNcHuEgAAQaQtf/42xbGPCN1ut3bv3l2vfffu3XK73a0e92owi4mJkSSVlZXp0qVLysjI8PVJTExUampqg38/AACAYx8RPvbYY5o9e7bKy8t971/t2bNHL730khYtWtSqMS3LUk5OjkaOHKnU1FRJUlVVlcLDw9W1a1e/vnFxcaqqqmpwHK/XK6/X69v3eDytqgcAADiTYwPW4sWLlZKSopUrV%2BrVV1%2BVJN1666166aWXNG3atFaNOXfuXO3fv1%2B7du1qsq9lWY3%2BFmN%2Bfr6WLFnSqhoAAIDzOTZgSdK0adNaHaa%2Bbd68edq8ebNKSkqUlJTka4%2BPj9fFixd17tw5v1Ws6upqDR8%2BvMGxcnNzlZOT49v3eDxKTk42UicAAAh8jn0HS/o6uKxZs0Y//elPde7cOUnSJ598osrKymaPYVmW5s6dqzfffFPvv/%2B%2BUlJS/I4PGTJEYWFhKioq8rVVVlbq4MGDjQYst9utqKgovw0AAAQPx65gHTx4UOPGjVPnzp1VUVGhhx56SF27dtVvfvMbnTx5UmvXrm3WOHPmzNEbb7yh3/3ud4qMjPS9VxUdHa1OnTopOjpaM2fO1Pz589WtWzfFxMRowYIF6t%2B/v8aNG9eWUwQAAA7l2BWsJ554QtOmTdOxY8cUERHha7/77rtVUlLS7HEKCgpUU1Oj0aNHKyEhwbetX7/e12fFihW6//77NXnyZI0YMUKdO3fWW2%2B9xWdgAQCABjl2BWvv3r0qKCio96L5d77znRY/ImxKRESEXnjhBb3wwgstrhMAAAQfx65ghYeHq66url770aNHFRsba0NFAAAAX3NswLr33nv1r//6r7p8%2BbIkyeVy6c9//rMWLlyoSZMm2VwdAAAIZo4NWMuXL9epU6cUHx%2BvCxcuaMyYMfrud7%2BriIiIet9PCAAA0J4c%2Bw5WdHS0du/eraKiIn388cf66quvNHjwYI0fP77RDwAFAABoD44MWJcuXdJdd92lF198URkZGX7fEwgAAGA3Rz4iDAsLU3l5OStVAAAgIDkyYEnSgw8%2BqNdee83uMgAAAOpx5CPCq1atWqXt27crLS1NXbp08Tv29NNP21QVAAAIdo4NWGVlZRowYIAkaf/%2B/X7HeHQIAADs5LiA9dlnnyklJUU7d%2B60uxQAAIAGOe4drF69eunMmTO%2B/SlTpuj06dM2VgQAAODPcQHr298duGXLFp0/f96magAAAOpzXMACAAAIdI4LWC6Xq95L7LzUDgAAAonjXnK3LEsPPfSQ3G63JOnLL7/U7Nmz631Mw5tvvmlHeQAAAM4LWFlZWX77Dz74oE2VAAAANMxxAYtPbwcAAIHOce9gAQAABDoCFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMCwoA9YJSUluueee5SYmCiXy6VNmzb5HX/ooYd8XzB9dRs2bJhN1QIAACcI%2BoB1/vx5DRw4UKtWrWq0z4QJE1RZWenbtmzZ0o4VAgAAp3HcdxGalpmZqczMzGv2cbvdio%2BPb6eKAACA0wX9ClZz7NixQz169NAtt9yiWbNmqbq6%2Bpr9vV6vPB6P3wYAAIIHAasJmZmZWrdund5//30tX75ce/fu1ZgxY%2BT1ehs9Jz8/X9HR0b4tOTm5HSsGAAB2C/pHhE2ZMmWK78%2BpqalKS0tTz5499fbbb2vSpEkNnpObm6ucnBzfvsfjIWQBABBECFgtlJCQoJ49e%2Bro0aON9nG73XK73e1YFQAACCQ8Imyhv/zlL6qoqFBCQoLdpQAAgAAV9CtYdXV1%2BuMf/%2BjbP378uPbt26eYmBjFxMQoLy9PP/jBD5SQkKDPP/9cixYtUmxsrL7//e/bWDUAAAhkQR%2BwSktLdccdd/j2r747lZWVpYKCAh04cEC//OUv9cUXXyghIUF33HGH1q9fr8jISLtKBgAAAS7oA9bo0aNlWVajx9999912rAYAAHQEvIMFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMCzoP2gUADqatMVb7S6h2UqfmmB3CUCbYAULAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGBY0AeskpIS3XPPPUpMTJTL5dKmTZv8jluWpby8PCUmJqpTp04aPXq0Dh06ZFO1AADACYI%2BYJ0/f14DBw7UqlWrGjz%2B9NNP69lnn9WqVau0d%2B9excfH684771RtbW07VwoAAJwi1O4C7JaZmanMzMwGj1mWpeeee06LFy/WpEmTJElr165VXFyc3njjDT3yyCPtWSoAAHCIoF/Bupbjx4%2BrqqpKGRkZvja3263bb79du3fvbvQ8r9crj8fjtwEAgOBBwLqGqqoqSVJcXJxfe1xcnO9YQ/Lz8xUdHe3bkpOT27ROAAAQWAhYzeByufz2Lcuq1/ZNubm5qqmp8W0VFRVtXSIAAAggQf8O1rXEx8dL%2BnolKyEhwddeXV1db1Xrm9xut9xud5vXBwAAAhMrWNeQkpKi%2BPh4FRUV%2BdouXryo4uJiDR8%2B3MbKAABAIAv6Fay6ujr98Y9/9O0fP35c%2B/btU0xMjG688UZlZ2dr6dKl6tWrl3r16qWlS5eqc%2BfOmjZtmo1VAwCAQBb0Aau0tFR33HGHbz8nJ0eSlJWVpTVr1ujJJ5/UhQsX9OMf/1jnzp3T9773PW3btk2RkZF2lQwAAAJc0Aes0aNHy7KsRo%2B7XC7l5eUpLy%2Bv/YoCAACOxjtYAAAAhgX9ChYAAM2Vtnir3SW0SOlTE%2BwuIWixggUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwGpCXl6eXC6X3xYfH293WQAAIICF2l2AE/Tr10/bt2/37YeEhNhYDQAACHQErGYIDQ1l1QoAADQbjwib4ejRo0pMTFRKSoqmTp2qzz77zO6SAABAAGMFqwnf%2B9739Mtf/lK33HKLTp8%2BrX/7t3/T8OHDdejQIXXr1q3Bc7xer7xer2/f4/G0V7kAACAAELCakJmZ6ftz//79lZ6erptuuklr165VTk5Og%2Bfk5%2BdryZIl7VUigDaWtnir3SV0WPzboqPiEWELdenSRf3799fRo0cb7ZObm6uamhrfVlFR0Y4VAgAAu7GC1UJer1effvqpRo0a1Wgft9stt9vdjlUBAIBAwgpWExYsWKDi4mIdP35cH330kR544AF5PB5lZWXZXRoAAAhQrGA14eTJk/q7v/s7nT17Vt27d9ewYcO0Z88e9ezZ0%2B7SAABAgCJgNaGwsNDuEgAAgMPwiBAAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADAs1O4CAABA20hbvNXuEpqt9KkJdpdgFCtYAAAAhhGwAAAADCNgNdOLL76olJQURUREaMiQIdq5c6fdJQEAgABFwGqG9evXKzs7W4sXL1Z5eblGjRqlzMxMnThxwu7SAABAACJgNcOzzz6rmTNn6kc/%2BpFuvfVWPffcc0pOTlZBQYHdpQEAgADEbxE24eLFiyorK9PChQv92jMyMrR79%2B4Gz/F6vfJ6vb79mpoaSZLH4zFe3xXveeNjtpW2mD/QHpx0nwFO1RY/I66OaVmW8bGbQsBqwtmzZ3XlyhXFxcX5tcfFxamqqqrBc/Lz87VkyZJ67cnJyW1So1NEL7e7AgBAoGrLnxG1tbWKjo5uu7%2BgAQSsZnK5XH77lmXVa7sqNzdXOTk5vv2vvvpK//M//6Nu3bo1ek5reDweJScnq6KiQlFRUcbGtVtHnZfUcefWUeclMTcn6qjzkjru3NpqXpZlqba2VomJicbGbC4CVhNiY2MVEhJSb7Wqurq63qrWVW63W26326/thhtuaLMao6KiOtSNdlVHnZfUcefWUeclMTcn6qjzkjru3NpiXu29cnUVL7k3ITw8XEOGDFFRUZFfe1FRkYYPH25TVQAAIJCxgtUMOTk5mjFjhtLS0pSenq7Vq1frxIkTmj17tt2lAQCAABSSl5eXZ3cRgS41NVXdunXT0qVL9cwzz%2BjChQt6/fXXNXDgQLtLU0hIiEaPHq3Q0I6VlTvqvKSOO7eOOi%2BJuTlRR52X1HHn1tHm5bLs%2BN1FAACADox3sABp7HFOAAAIRElEQVQAAAwjYAEAABhGwAIAADCMgAUAAGAYASvAvfjii0pJSVFERISGDBminTt3XrP/hg0b1LdvX7ndbvXt21cbN25sp0pbpiXzWrNmjVwuV73tyy%2B/bMeKm1ZSUqJ77rlHiYmJcrlc2rRpU5PnFBcXa8iQIYqIiNB3v/td/cd//Ec7VNpyLZ3bjh07Grxm//3f/91OFTdPfn6%2Bhg4dqsjISPXo0UP333%2B/jhw50uR5TrjPWjM3J9xrBQUFGjBggO8DKdPT0/XOO%2B9c8xwnXC%2Bp5XNzwvVqSH5%2Bvlwul7Kzs6/ZzynXrTEErAC2fv16ZWdna/HixSovL9eoUaOUmZmpEydONNj/ww8/1JQpUzRjxgx98sknmjFjhiZPnqyPPvqonSu/tpbOS/r6030rKyv9toiIiHasumnnz5/XwIEDtWrVqmb1P378uO666y6NGjVK5eXlWrRokR577DFt2LChjSttuZbO7aojR474XbNevXq1UYWtU1xcrDlz5mjPnj0qKirS5cuXlZGRofPnG/9yZ6fcZ62ZmxT491pSUpKWLVum0tJSlZaWasyYMbrvvvt06NChBvs75XpJLZ%2BbFPjX69v27t2r1atXa8CAAdfs56Tr1igLAetv//ZvrdmzZ/u19enTx1q4cGGD/SdPnmxNmDDBr238%2BPHW1KlT26zG1mjpvF577TUrOjq6PUozRpK1cePGa/Z58sknrT59%2Bvi1PfLII9awYcPasrS/WnPm9sEHH1iSrHPnzrVTVWZUV1dbkqzi4uJG%2BzjlPvu25szNifeaZVlW165drZdffrnBY069Xldda25Ou161tbVWr169rKKiIuv222%2B3Hn/88Ub7Ov26WZZlsYIVoC5evKiysjJlZGT4tWdkZGj37t0NnvPhhx/W6z9%2B/PhG%2B9uhNfOSpLq6OvXs2VNJSUmaOHGiysvL27rUNtfY9SotLdWlS5dsqsqsQYMGKSEhQWPHjtUHH3xgdzlNqqmpkSTFxMQ02scJ91lDmjM3yVn32pUrV1RYWKjz588rPT29wT5OvV7NmZvkrOs1Z84c3X333Ro3blyTfZ163b6JgBWgzp49qytXrtT7Qum4uLh6Xzx9VVVVVYv626E18%2BrTp4/WrFmjzZs369e//rUiIiI0YsQIHT16tD1KbjONXa/Lly/r7NmzNlVlRkJCglavXq0NGzbozTffVO/evTV27FiVlJTYXVqjLMtSTk6ORo4cqdTU1Eb7OeE%2B%2B7bmzs0p99qBAwd0/fXXy%2B12a/bs2dq4caP69u3bYF%2BnXa%2BWzM0p10uSCgsLVVZWpvz8/Gb1d9p1a0jH%2BDz6DszlcvntW5ZVr%2B2v6W%2BXltQ5bNgwDRs2zLc/YsQIDR48WC%2B88IKef/75Nq2zrTX079BQu9P07t1bvXv39u2np6eroqJCzzzzjG677TYbK2vc3LlztX//fu3atavJvk65z65q7tyccq/17t1b%2B/bt0xdffKENGzYoKytLxcXFjQYRJ12vlszNKderoqJCjz/%2BuLZt29ai98OcdN0awgpWgIqNjVVISEi9tF5dXV0v1V8VHx/fov52aM28vu26667T0KFDA/L/0lqisesVGhqqbt262VRV2xk2bFjAXrN58%2BZp8%2BbN%2BuCDD5SUlHTNvk64z76pJXP7tkC918LDw3XzzTcrLS1N%2Bfn5GjhwoFauXNlgX6ddr5bM7dsC9XqVlZWpurpaQ4YMUWhoqEJDQ1VcXKznn39eoaGhunLlSr1znHbdGkLAClDh4eEaMmSIioqK/NqLioo0fPjwBs9JT0%2Bv13/btm2N9rdDa%2Bb1bZZlad%2B%2BfUpISGiLEttNY9crLS1NYWFhNlXVdsrLywPumlmWpblz5%2BrNN9/U%2B%2B%2B/r5SUlCbPccJ9JrVubg2N4YR7zbIseb3eBo855Xo15lpza6hvIF6vsWPH6sCBA9q3b59vS0tL0/Tp07Vv3z6FhITUO8fp100Sv0UYyAoLC62wsDDrlVdesQ4fPmxlZ2dbXbp0sT7//HPLsixrxowZfr9591//9V9WSEiItWzZMuvTTz%2B1li1bZoWGhlp79uyxawoNaum88vLyrK1bt1rHjh2zysvLrYcfftgKDQ21PvroI7um0KDa2lqrvLzcKi8vtyRZzz77rFVeXm796U9/sizLshYuXGjNmDHD1/%2Bzzz6zOnfubD3xxBPW4cOHrVdeecUKCwuz/vM//9OuKTSqpXNbsWKFtXHjRusPf/iDdfDgQWvhwoWWJGvDhg12TaFBjz76qBUdHW3t2LHDqqys9G3/%2B7//6%2Bvj1PusNXNzwr2Wm5trlZSUWMePH7f2799vLVq0yLruuuusbdu2WZbl3OtlWS2fmxOuV2O%2B/VuETr5ujSFgBbif//znVs%2BePa3w8HBr8ODBfr9iffvtt1tZWVl%2B/X/7299avXv3tsLCwqw%2BffoE3A%2B0q1oyr%2BzsbOvGG2%2B0wsPDre7du1sZGRnW7t27baj62q5%2BNMG3t6tzycrKsm6//Xa/c3bs2GENGjTICg8Pt/7mb/7GKigoaP/Cm6Glc/v3f/9366abbrIiIiKsrl27WiNHjrTefvtte4q/hobmJMl67bXXfH2cep%2B1Zm5OuNd%2B%2BMMf%2Bv7b0b17d2vs2LG%2BAGJZzr1eltXyuTnhejXm2wHLydetMS7L%2Br%2B3agEAAGAE72ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADDs/wFD8Rix/f79UwAAAABJRU5ErkJggg%3D%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common-4332127071032318910">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">42</td>
        <td class="number">35.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">13</td>
        <td class="number">11.1%</td>
        <td>
            <div class="bar" style="width:31%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:22%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">42</td>
        <td class="number">35.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme-4332127071032318910">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">42</td>
        <td class="number">35.9%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">13</td>
        <td class="number">11.1%</td>
        <td>
            <div class="bar" style="width:31%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:22%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:39%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.5</td>
        <td class="number">13</td>
        <td class="number">11.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">5</td>
        <td class="number">4.3%</td>
        <td>
            <div class="bar" style="width:39%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:69%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:8%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Incisor number">Incisor number<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>9</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>7.7%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (%)</th>
                    <td>34.2%</td>
                </tr>
                <tr class="alert">
                    <th>Missing (n)</th>
                    <td>40</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Mean</th>
                    <td>1.5844</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>7</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>37.6%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram3539603168765053218">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAAc1JREFUeJzt3bHO0lAYx%2BG3xLVcAGkvwsRbMPGC3EzcvBx3V3fvgoYLgDg4SB0UF8w/H0ZP6cfzjBB4T6G/9HRqN8/zXI1N01TjOLYey8rt9/sahqHpzBdNp/3S931V/Tzg7Xa7xBJYkePxWOM4/j5vWlokkK7rqqpqu91eBfLq3aebv%2B/Lhzf/ZF3ct8t509Km%2BURYEYFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBALBIg/xfBQeSLp%2BriAQCAQCW6w702JbZuv3dK4gEAgEgmexxbJl%2BP8e9Td%2BFoH8jUf9w7mNLRYEi1xB5nmuqqrj8Xj13vdvX1sv58n%2BtN6k1bHc67pevv1482c%2Bv3999drl%2BC7nTUvdvMDUaZpqHMfWY1m5/X5fwzA0nblIIOfzuQ6HQ/V9X13XtR7PyszzXKfTqXa7XW02be8KFgkE1sJNOgQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBALBD/B1YSJyGps/AAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives3539603168765053218,#minihistogram3539603168765053218"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives3539603168765053218">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles3539603168765053218"
                                                  aria-controls="quantiles3539603168765053218" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram3539603168765053218" aria-controls="histogram3539603168765053218"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common3539603168765053218" aria-controls="common3539603168765053218"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme3539603168765053218" aria-controls="extreme3539603168765053218"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles3539603168765053218">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>3</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>6</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>7</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>7</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>3</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>2.1235</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>1.3402</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-0.52016</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>1.5844</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.8715</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.95481</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>122</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>4.5092</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>1016.0 B</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram3539603168765053218">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAH/9JREFUeJzt3X9wVOXB9vFry5IFY7KSAAkxwaYIggYoJlSDqAgYTUG0jIIDYlC0RmMkBqpgpsrT1oRBEWVQpkEFldqgU0GcKhBaDDAplUQoP3QQi5UgiQGL%2BcFggHDeP3jZmgYE7b1752S/n5md6Z5N1msrffg%2BZ082HsdxHAEAAMCYH9keAAAA0N4QWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIYRWAAAAIZ5bQ8IBydOnND%2B/fsVFRUlj8djew4AAGHBcRw1NDQoISFBP/pRaM8pEVghsH//fiUlJdmeAQBAWKqqqlJiYmJI/5kEVghERUVJOvkvODo62vIaAADCQ319vZKSkgJ/D4cSgRUCp94WjI6OJrAAAAgxG5fncJE7AACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYQQWAACAYfyyZ5dLK1hle8I5q3jyRtsTAAAICc5gAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgAQAAGEZgfUtRUZE8Ho/y8vICx5qampSbm6uuXbsqMjJSY8aM0b59%2ByyuBAAAbR2B9f9t3rxZxcXFGjBgQIvjeXl5Wr58uUpKSrRx40Y1NjZq9OjRam5utrQUAAC0dQSWpMbGRk2cOFGLFi1Sly5dAsfr6ur00ksvae7cuRo5cqQGDRqkpUuXavv27Vq7dq3FxQAAoC0jsCTl5ORo1KhRGjlyZIvjlZWVOnbsmDIyMgLHEhISlJKSovLy8lDPBAAALuG1PcC2kpISVVZWqqKiotVjNTU1ioiIaHFWS5Li4uJUU1NzxudsampSU1NT4H59fb25wQAAoM0L6zNYVVVVmjp1qv7whz%2BoU6dO5/x9juPI4/Gc8fGioiL5/f7ALSkpycRcAADgEmEdWJWVlaqtrVVqaqq8Xq%2B8Xq/Kyso0f/58eb1excXF6ejRozp06FCL76utrVVcXNwZn3fmzJmqq6sL3KqqqoL9UgAAQBsS1m8RjhgxQtu3b29x7K677lLfvn316KOPKikpSR07dlRpaanGjRsnSaqurtaOHTs0Z86cMz6vz%2BeTz%2BcL6nYAANB2hXVgRUVFKSUlpcWxyMhIxcbGBo5PmTJF06ZNU2xsrGJiYjR9%2BnT179%2B/1QXxAAAAp4R1YJ2LefPmyev1aty4cTpy5IhGjBihJUuWqEOHDranAQCANsrjOI5je0R7V19fL7/fr7q6OkVHRxt97rSCVUafL5gqnrzR9gQAQBgJ5t%2B/ZxPWF7kDAAAEA4EFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgGIEFAABgWNgH1sKFCzVgwABFR0crOjpa6enpeu%2B99wKPNzU1KTc3V127dlVkZKTGjBmjffv2WVwMAADaurAPrMTERM2ePVsVFRWqqKjQ8OHDdfPNN2vnzp2SpLy8PC1fvlwlJSXauHGjGhsbNXr0aDU3N1teDgAA2iqP4ziO7RFtTUxMjJ566indeuut6tatm1577TWNHz9ekrR//34lJSXp3Xff1Q033HBOz1dfXy%2B/36%2B6ujpFR0cb3ZpWsMro8wVTxZM32p4AAAgjwfz792zC/gzWtzU3N6ukpESHDx9Wenq6KisrdezYMWVkZAS%2BJiEhQSkpKSovLz/j8zQ1Nam%2Bvr7FDQAAhA8CS9L27dt1/vnny%2BfzKTs7W8uXL9ell16qmpoaRUREqEuXLi2%2BPi4uTjU1NWd8vqKiIvn9/sAtKSkp2C8BAAC0IQSWpEsuuURbt27Vpk2bdP/99ysrK0sfffTRGb/ecRx5PJ4zPj5z5kzV1dUFblVVVcGYDQAA2iiv7QFtQUREhC6%2B%2BGJJUlpamjZv3qznnntO48eP19GjR3Xo0KEWZ7Fqa2s1ZMiQMz6fz%2BeTz%2BcL%2Bm4AANA2cQbrNBzHUVNTk1JTU9WxY0eVlpYGHquurtaOHTu%2BM7AAAEB4C/szWI899pgyMzOVlJSkhoYGlZSU6P3339eqVavk9/s1ZcoUTZs2TbGxsYqJidH06dPVv39/jRw50vZ0AADQRoV9YH355ZeaNGmSqqur5ff7NWDAAK1atUrXX3%2B9JGnevHnyer0aN26cjhw5ohEjRmjJkiXq0KGD5eUAAKCt4nOwQoDPwTqJz8ECAIQSn4MFAADQjhBYAAAAhhFYAAAAhhFYAAAAhhFYAAAAhhFYAAAAhhFYAAAAhrk2sJYuXapvvvnG9gwAAIBWXBtY%2Bfn5io%2BP13333acPPvjA9hwAAIAA1wbW/v379fLLL6u6ulpDhw7VZZddprlz5%2BrAgQO2pwEAgDDn2sDyer0aO3asVq5cqb179yorK0svv/yyEhMTNXbsWP35z38WvwUIAADY4NrA%2Brb4%2BHiNGDFCw4YNk8fjUUVFhSZMmKDevXtrw4YNtucBAIAw4%2BrAOnjwoJ599lkNHDhQV111lWpra7VixQp9/vnn%2BuKLLzR69GjdeeedtmcCAIAw47U94If6xS9%2BoXfffVfJycm65557lJWVpW7dugUeP//88/XII49o/vz5FlcCAIBw5NrAio6O1tq1a3X11Vef8Wt69Oih3bt3h3AVAACAiwPrlVdeOevXeDwe9erVKwRrAAAA/sO112A9/PDDWrBgQavjzz//vKZNm2ZhEQAAwEmuDaw333xTV155Zavj6enpWrZsmYVFAAAAJ7k2sA4ePKguXbq0Oh4dHa2DBw9aWAQAAHCSawOrV69eWr16davjq1evVnJysoVFAAAAJ7n2Ive8vDzl5eXpq6%2B%2B0vDhwyVJf/nLXzRnzhw9/fTTltcBAIBw5trAuvfee/XNN9%2BosLBQTzzxhCQpMTFR8%2BfP19133215HQAACGeuDSxJys3NVW5urqqrq9W5c2ddcMEFticBAAC4O7BO6dGjh%2B0JAAAAAa69yP3AgQO666671LNnT3Xq1EkREREtbgAAALa49gzW5MmT9c9//lO/%2BtWv1KNHD3k8HtuTAAAAJLk4sNavX6/169dr0KBBtqcAAAC04Nq3CBMTEzlrBQAA2iTXBta8efM0c%2BZM7du3z/YUAACAFlz7FuGkSZPU0NCgiy66SNHR0erYsWOLx2tray0tAwAA4c61gTV79mzbEwAAAE7LtYE1ZcoU2xMAAABOy7XXYEnSv/71L82aNUuTJk0KvCW4Zs0affzxx5aXAQCAcObawNqwYYMuu%2BwylZWV6Y033lBjY6Mk6cMPP9Tjjz9ueR0AAAhnrg2sRx99VLNmzdK6detafHL78OHDtWnTJovLAABAuHNtYG3btk233nprq%2BPdu3fXgQMHLCwCAAA4ybWBdcEFF6impqbV8a1bt%2BrCCy%2B0sAgAAOAk1wbW7bffrhkzZujAgQOBT3T/%2B9//runTp%2BuOO%2B6wvA4AAIQz1wZWYWGh4uPj1aNHDzU2NurSSy/VkCFDNHjwYP3617%2B2PQ8AAIQx134OVkREhJYtW6ZPPvlEH374oU6cOKHLL79cffv2tT0NAACEOdcG1il9%2BvRRnz59bM8AAAAIcG1g/fKXv/zOx4uLi0O0BAAAoCXXBlZ1dXWL%2B8eOHdPOnTvV0NCga665xtIqAAAAFwfWO%2B%2B80%2BrY8ePHdf/996tfv34WFgEAAJzk2p8iPB2v16vp06frqaeesj0FAACEsXYVWJK0Z88eHTt2zPYMAAAQxlz7FuEjjzzS4r7jOKqurtbKlSs1ceJES6sAAABcHFh/%2B9vfWtz/0Y9%2BpG7dumn27Nm69957La0CAABwcWBt2LDB9gQAAIDTanfXYAEAANjm2jNYgwcPDvyS57P54IMPgrwGAADgP1wbWNddd51%2B//vfq0%2BfPkpPT5ckbdq0Sbt27dJ9990nn89neSEAAAhXrg2sr7/%2BWjk5OSosLGxxvKCgQF9%2B%2BaVefPFFS8sAAEC4c%2B01WG%2B88YbuuuuuVscnT56sN99808IiAACAk1wbWD6fT%2BXl5a2Ol5eX8/YgAACwyrVvET700EPKzs7Wli1bdOWVV0o6eQ3WokWL9Nhjj1leBwAAwplrA6ugoEDJycl67rnn9PLLL0uS%2BvXrp0WLFmnChAmW1wEAgHDm2sCSpAkTJhBTAACgzXHtNViSVF9fryVLlujxxx/XoUOHJEn/%2BMc/VF1dfc7PUVRUpMGDBysqKkrdu3fXLbfcol27drX4mqamJuXm5qpr166KjIzUmDFjtG/fPqOvBQAAtB%2BuDawdO3aoT58%2B%2Bs1vfqOioqJAYL3xxhuaMWPGOT9PWVmZcnJytGnTJpWWlur48ePKyMjQ4cOHA1%2BTl5en5cuXq6SkRBs3blRjY6NGjx6t5uZm468LAAC4n2vfInz44Yc1YcIEzZ07V9HR0YHjo0aN0sSJE8/5eVatWtXi/uLFi9W9e3dVVlbqmmuuUV1dnV566SW99tprGjlypCRp6dKlSkpK0tq1a3XDDTeYeUEAAKDdcO0ZrM2bN%2BuBBx5o9etyLrzwwu/1FuF/q6urkyTFxMRIkiorK3Xs2DFlZGQEviYhIUEpKSmn/ZgI6eRbivX19S1uAAAgfLg2sCIiItTY2Njq%2BO7du9W1a9cf9JyO4yg/P19Dhw5VSkqKJKmmpkYRERHq0qVLi6%2BNi4tTTU3NaZ%2BnqKhIfr8/cEtKSvpBewAAgDu5NrDGjBmj3/72tzp%2B/LgkyePx6IsvvtCMGTM0duzYH/ScDz74oLZt26Y//vGPZ/1ax3HO%2BMumZ86cqbq6usCtqqrqB%2B0BAADu5NrAmjt3rvbv36/4%2BHgdOXJEw4cP109%2B8hN16tSp1e8nPBe5ublauXKl1q1bp8TExMDx%2BPh4HT16NHAR/Sm1tbWKi4s77XP5fD5FR0e3uAEAgPDh2ovc/X6/ysvLVVpaqg8//FAnTpzQ5ZdfrhtuuOGMZ5ZOx3Ec5ebmavny5Xr//feVnJzc4vHU1FR17NhRpaWlGjdunCSpurpaO3bs0Jw5c4y%2BJgAA0D64MrCOHTumn//853rhhReUkZHR4gL07ysnJ0evv/663n77bUVFRQWuq/L7/ercubP8fr%2BmTJmiadOmKTY2VjExMZo%2Bfbr69%2B8f%2BKlCAACAb3NlYHXs2FFbtmz5XmeqzmThwoWSpGHDhrU4vnjxYk2ePFmSNG/ePHm9Xo0bN05HjhzRiBEjtGTJEnXo0OF//ucDAID2x%2BM4jmN7xA%2BRl5enyMhIPfnkk7annFV9fb38fr/q6uqMX4%2BVVrDq7F/URlQ8eaPtCQCAMBLMv3/PxpVnsE5ZsGCB1q5dq7S0NEVGRrZ4jOujAACALa4NrMrKSg0YMECStG3bthaPmXjrEAAA4IdyXWDt2bNHycnJ2rBhg%2B0pAAAAp%2BW6z8Hq3bu3Dhw4ELg/fvx4ffnllxYXAQAAtOS6wPrva/LfffddHT582NIaAACA1lwXWAAAAG2d6wLL4/G0uoidi9oBAEBb4rqL3B3H0eTJk%2BXz%2BSRJ33zzjbKzs1t9TMNbb71lYx4AAID7AisrK6vF/TvuuMPSEgAAgNNzXWAtXrzY9gQAAIDv5LprsAAAANo6AgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMCwsA%2Bs9evX66abblJCQoI8Ho9WrFjR4nHHcTRr1iwlJCSoc%2BfOGjZsmHbu3GlpLQAAcIOwD6zDhw9r4MCBWrBgwWkfnzNnjp555hktWLBAmzdvVnx8vK6//no1NDSEeCkAAHALr%2B0BtmVmZiozM/O0jzmOo2effVYFBQUaO3asJOmVV15RXFycXn/9dd13332hnAoAAFwi7M9gfZfPPvtMNTU1ysjICBzz%2BXy69tprVV5ebnEZAABoy8L%2BDNZ3qampkSTFxcW1OB4XF6fPP//8jN/X1NSkpqamwP36%2BvrgDAQAAG0SZ7DOgcfjaXHfcZxWx76tqKhIfr8/cEtKSgr2RAAA0IYQWN8hPj5e0n/OZJ1SW1vb6qzWt82cOVN1dXWBW1VVVVB3AgCAtoXA%2Bg7JycmKj49XaWlp4NjRo0dVVlamIUOGnPH7fD6foqOjW9wAAED4CPtrsBobG/Xpp58G7n/22WfaunWrYmJi1LNnT%2BXl5amwsFC9e/dW7969VVhYqPPOO08TJkywuBoAALRlYR9YFRUVuu666wL38/PzJUlZWVlasmSJHnnkER05ckQPPPCADh06pCuuuEJr1qxRVFSUrckAAKCN8ziO49ge0d7V19fL7/errq7O%2BNuFaQWrjD5fMFU8eaPtCQCAMBLMv3/PhmuwAAAADAv7twgBhJ6bzrxKnH0F8P1xBgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwAgsAAMAwr%2B0BANDWpRWssj3he6l48kbbE84Z/92iveIMFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGEEFgAAgGFe2wMQPtIKVtme8L1UPHmj7QkA2hj%2B7xjOFWewAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADONjGoAz4MexAQA/FGewAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADCOwAAAADPPaHgAAAIIjrWCV7QnnrOLJG21PMIozWAAAAIYRWAAAAIYRWOfohRdeUHJysjp16qTU1FRt2LDB9iQAANBGEVjnYNmyZcrLy1NBQYG2bNmiq6%2B%2BWpmZmdq7d6/taQAAoA0isM7BM888oylTpuiee%2B5Rv3799OyzzyopKUkLFy60PQ0AALRB/BThWRw9elSVlZWaMWNGi%2BMZGRkqLy8/7fc0NTWpqakpcL%2Burk6SVF9fb3xfc9Nh488JdwrGn69g4c9tcPFnAW4UjD%2B3p57TcRzjz302BNZZHDx4UM3NzYqLi2txPC4uTjU1Naf9nqKiIv3f//1fq%2BNJSUlB2QhIkn%2Bu7QVoK/izADcK5p/bhoYG%2Bf3%2B4P0DToPAOkcej6fFfcdxWh07ZebMmcrPzw/cP3HihP79738rNjb2jN/zQ9TX1yspKUlVVVWKjo429rxtHa87vF63FL6vPVxftxS%2Br53XbfZ1O46jhoYGJSQkGHvOc0VgnUXXrl3VoUOHVmeramtrW53VOsXn88nn87U4dsEFFwRtY3R0dFj9D/EUXnf4CdfXHq6vWwrf187rNifUZ65O4SL3s4iIiFBqaqpKS0tbHC8tLdWQIUMsrQIAAG0ZZ7DOQX5%2BviZNmqS0tDSlp6eruLhYe/fuVXZ2tu1pAACgDeowa9asWbZHtHUpKSmKjY1VYWGhnn76aR05ckSvvfaaBg4caHuaOnTooGHDhsnrDa9W5nWH1%2BuWwve1h%2BvrlsL3tfO628fr9jg2fnYRAACgHeMaLAAAAMMILAAAAMMILAAAAMMILAAAAMMILJd64YUXlJycrE6dOik1NVUbNmywPSno1q9fr5tuukkJCQnyeDxasWKF7UkhUVRUpMGDBysqKkrdu3fXLbfcol27dtmeFRILFy7UgAEDAh8%2BmJ6ervfee8/2rJAqKiqSx%2BNRXl6e7SlBN2vWLHk8nha3%2BPh427NC4osvvtAdd9yh2NhYnXfeefrpT3%2BqyspK27OC7sc//nGrf%2Bcej0c5OTm2p/3PCCwXWrZsmfLy8lRQUKAtW7bo6quvVmZmpvbu3Wt7WlAdPnxYAwcO1IIFC2xPCamysjLl5ORo06ZNKi0t1fHjx5WRkaHDh9v/L8lNTEzU7NmzVVFRoYqKCg0fPlw333yzdu7caXtaSGzevFnFxcUaMGCA7Skhc9lll6m6ujpw2759u%2B1JQXfo0CFdddVV6tixo9577z199NFHmjt3blB/A0hbsXnz5hb/vk99qPdtt91meZkBDlznZz/7mZOdnd3iWN%2B%2BfZ0ZM2ZYWhR6kpzly5fbnmFFbW2tI8kpKyuzPcWKLl26OC%2B%2B%2BKLtGUHX0NDg9O7d2yktLXWuvfZaZ%2BrUqbYnBd0TTzzhDBw40PaMkHv00UedoUOH2p7RJkydOtXp1auXc%2BLECdtT/mecwXKZo0ePqrKyUhkZGS2OZ2RkqLy83NIqhFJdXZ0kKSYmxvKS0GpublZJSYkOHz6s9PR023OCLicnR6NGjdLIkSNtTwmp3bt3KyEhQcnJybr99tu1Z88e25OCbuXKlUpLS9Ntt92m7t27a9CgQVq0aJHtWSF39OhRLV26VHfffbc8Ho/tOf8zAstlDh48qObm5la/aDouLq7VL6RG%2B%2BM4jvLz8zV06FClpKTYnhMS27dv1/nnny%2Bfz6fs7GwtX75cl156qe1ZQVVSUqLKykoVFRXZnhJSV1xxhV599VWtXr1aixYtUk1NjYYMGaKvvvrK9rSg2rNnjxYuXKjevXtr9erVys7O1kMPPaRXX33V9rSQWrFihb7%2B%2BmtNnjzZ9hQj2sfn0Yeh/657x3HaRfHjuz344IPatm2bNm7caHtKyFxyySXaunWrvv76a/3pT39SVlaWysrK2m1kVVVVaerUqVqzZo06depke05IZWZmBv5z//79lZ6erl69eumVV15Rfn6%2BxWXBdeLECaWlpamwsFCSNGjQIO3cuVMLFy7UnXfeaXld6Lz00kvKzMxUQkKC7SlGcAbLZbp27aoOHTq0OltVW1vb6qwW2pfc3FytXLlS69atU2Jiou05IRMREaGLL75YaWlpKioq0sCBA/Xcc8/ZnhU0lZWVqq2tVWpqqrxer7xer8rKyjR//nx5vV41NzfbnhgykZGR6t%2B/v3bv3m17SlD16NGj1f/D0K9fv3b/g0vf9vnnn2vt2rW65557bE8xhsBymYiICKWmpgZ%2B0uKU0tJSDRkyxNIqBJPjOHrwwQf11ltv6a9//auSk5NtT7LKcRw1NTXZnhE0I0aM0Pbt27V169bALS0tTRMnTtTWrVvVoUMH2xNDpqmpSR9//LF69Ohhe0pQXXXVVa0%2BeuWTTz7RRRddZGlR6C1evFjdu3fXqFGjbE8xhrcIXSg/P1%2BTJk1SWlqa0tPTVVxcrL179yo7O9v2tKBqbGzUp59%2BGrj/2WefaevWrYqJiVHPnj0tLguunJwcvf7663r77bcVFRUVOHvp9/vVuXNny%2BuC67HHHlNmZqaSkpLU0NCgkpISvf/%2B%2B1q1apXtaUETFRXV6vq6yMhIxcbGtvvr7qZPn66bbrpJPXv2VG1trX73u9%2Bpvr5eWVlZtqcF1cMPP6whQ4aosLBQ48aN0wcffKDi4mIVFxfbnhYSJ06c0OLFi5WVlSWvtx1lid0fYsQP9fzzzzsXXXSRExER4Vx%2B%2BeVh8SP769atcyS1umVlZdmeFlSne82SnMWLF9ueFnR333134M95t27dnBEjRjhr1qyxPSvkwuVjGsaPH%2B/06NHD6dixo5OQkOCMHTvW2blzp%2B1ZIfHOO%2B84KSkpjs/nc/r27esUFxfbnhQyq1evdiQ5u3btsj3FKI/jOI6dtAMAAGifuAYLAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAMAILAADAsP8HnLu%2BWkCMPdwAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common3539603168765053218">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">44</td>
        <td class="number">37.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:21%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">40</td>
        <td class="number">34.2%</td>
        <td>
            <div class="bar" style="width:91%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme3539603168765053218">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">44</td>
        <td class="number">37.6%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:10%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2.0</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:7%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:21%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:14%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">3.0</td>
        <td class="number">9</td>
        <td class="number">7.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5.0</td>
        <td class="number">6</td>
        <td class="number">5.1%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.0</td>
        <td class="number">4</td>
        <td class="number">3.4%</td>
        <td>
            <div class="bar" style="width:45%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7.0</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:12%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Location">Location<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>2</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>1.7%</td>
        </tr>
        <tr class="ignore">
            <th>Missing (%)</th>
            <td>0.0%</td>
        </tr>
        <tr class="ignore">
            <th>Missing (n)</th>
            <td>0</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable7364642232363276182">
    <table class="mini freq">
        <tr class="">
    <th>Ribe</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 99.1%">
            116
        </div>
        
    </td>
</tr><tr class="">
    <th>G216</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable7364642232363276182, #minifreqtable7364642232363276182"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable7364642232363276182">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">Ribe</td>
        <td class="number">116</td>
        <td class="number">99.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">G216</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Notes">Notes<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="alert">
            <th>Distinct count</th>
            <td>55</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>47.0%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>44.4%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>52</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable4875146107334851312">
    <table class="mini freq">
        <tr class="">
    <th>T?nder mere end 1/3 slidte</th>
    <td>
        <div class="bar" style="width:20%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 8.5%">
            &nbsp;
        </div>
        10
    </td>
</tr><tr class="">
    <th>Kranie mangler</th>
    <td>
        <div class="bar" style="width:4%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 1.7%">
            &nbsp;
        </div>
        2
    </td>
</tr><tr class="">
    <th>Female?</th>
    <td>
        <div class="bar" style="width:4%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 1.7%">
            &nbsp;
        </div>
        2
    </td>
</tr><tr class="other">
    <th>Other values (51)</th>
    <td>
        <div class="bar" style="width:98%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 43.6%">
            51
        </div>
        
    </td>
</tr><tr class="missing">
    <th>(Missing)</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 44.4%">
            52
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable4875146107334851312, #minifreqtable4875146107334851312"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable4875146107334851312">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">T?nder mere end 1/3 slidte</td>
        <td class="number">10</td>
        <td class="number">8.5%</td>
        <td>
            <div class="bar" style="width:20%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Kranie mangler</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Female?</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:4%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Der er nok noget lepra k?rende.I 2 kasser.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Lidt lepra har man jo altid.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">T?nder faldet ud og remodeleret. Begge l?rben kn?kket postmortalt.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">hyperplasier sm?</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">T?nder mere end 1/3 slidt ned. Kranie udtaget til Ribe Middelalder. Har kigget p? det. Udstilling kommet hjem.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">den hvor underk?ben sidder p? i jord.</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">"sjov" sk?v hj?rnetand</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:2%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (44)</td>
        <td class="number">44</td>
        <td class="number">37.6%</td>
        <td>
            <div class="bar" style="width:84%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">52</td>
        <td class="number">44.4%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Sex">Sex<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>3</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>2.6%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>1.7%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>2</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable6569388946240910871">
    <table class="mini freq">
        <tr class="">
    <th>Male</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 57.3%">
            67
        </div>
        
    </td>
</tr><tr class="">
    <th>Female</th>
    <td>
        <div class="bar" style="width:71%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 41.0%">
            48
        </div>
        
    </td>
</tr><tr class="missing">
    <th>(Missing)</th>
    <td>
        <div class="bar" style="width:3%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 1.7%">
            &nbsp;
        </div>
        2
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable6569388946240910871, #minifreqtable6569388946240910871"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable6569388946240910871">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">Male</td>
        <td class="number">67</td>
        <td class="number">57.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Female</td>
        <td class="number">48</td>
        <td class="number">41.0%</td>
        <td>
            <div class="bar" style="width:71%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">2</td>
        <td class="number">1.7%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Signature">Signature<br/>
            <small>Categorical</small>
        </p>
    </div><div class="col-md-3">
    <table class="stats ">
        <tr class="">
            <th>Distinct count</th>
            <td>4</td>
        </tr>
        <tr>
            <th>Unique (%)</th>
            <td>3.4%</td>
        </tr>
        <tr class="alert">
            <th>Missing (%)</th>
            <td>2.6%</td>
        </tr>
        <tr class="alert">
            <th>Missing (n)</th>
            <td>3</td>
        </tr>
    </table>
</div>
<div class="col-md-6 collapse in" id="minifreqtable-2523218422070524154">
    <table class="mini freq">
        <tr class="">
    <th>MWOD</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 95.7%">
            112
        </div>
        
    </td>
</tr><tr class="">
    <th>MEWOD</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="">
    <th>Mwod</th>
    <td>
        <div class="bar" style="width:1%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 0.9%">
            &nbsp;
        </div>
        1
    </td>
</tr><tr class="missing">
    <th>(Missing)</th>
    <td>
        <div class="bar" style="width:3%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 2.6%">
            &nbsp;
        </div>
        3
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable-2523218422070524154, #minifreqtable-2523218422070524154"
       aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable-2523218422070524154">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">MWOD</td>
        <td class="number">112</td>
        <td class="number">95.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">MEWOD</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">Mwod</td>
        <td class="number">1</td>
        <td class="number">0.9%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="missing">
        <td class="fillremaining">(Missing)</td>
        <td class="number">3</td>
        <td class="number">2.6%</td>
        <td>
            <div class="bar" style="width:3%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Site_Number"><s>Site_Number</s><br/>
            <small>Constant</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is constant and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Constant value</th>
            <td>ASR1015</td>
        </tr>
    </table>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Teeth Scorable"><s>Teeth Scorable</s><br/>
            <small>Highly correlated</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is highly correlated with <a href="#pp_var_Canine largest age"><code>Canine largest age</code></a> and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Correlation</th>
            <td>0.90509</td>
        </tr>
    </table>
</div>
</div>
    <div class="row headerrow highlight">
        <h1>Correlations</h1>
    </div>
    <div class="row variablerow">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAsAAAAJ0CAYAAAAcdJWMAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzs3XlcVOX%2BwPHPsCibggqI%2Bwp43XEBRSVFu265ZrhlLrmQC0ZZdvVm5VJqmpUL9dPU3FKvW3bVrNTUFtFyycxdQ0QFRJRNhIH5/TEwMYrGPAdB7nzfrxevA2fm%2BzzPHM7MfOc7zzlHZzAYDAghhBBCCGElbIp7AEIIIYQQQhQlSYCFEEIIIYRVkQRYCCGEEEJYFUmAhRBCCCGEVZEEWAghhBBCWBVJgIUQQgghhFWRBFgIIYQQQlgVSYCFEEIIIYRVkQRYCCGEEEJYFbviHoAQQpRUV69epWPHjg%2B93d7eHhcXF2rWrEn79u15/vnncXFxKcIRCiGEyI9OLoUshBBq8ibAPj4%2BDyS3mZmZ3Lp1i5iYGAAqV67MypUrqVGjRpGPVQghxF8kARZCCEV5E%2BBVq1YREBCQ7/0iIyMZO3YsKSkp%2BPn5sX79%2BqIcphBCiPvIHGAhhHjMAgICeOWVVwA4duwYv//%2BezGPSAghrJskwEIIUQSefvpp0%2B8nTpwoxpEIIYSQg%2BCEEKIIlClTxvR7amqq2W1Hjhxh9erVHD16lNu3b1O2bFmaNm3KkCFDaN26db7tJSUlsX79evbv38%2BFCxdISUnB0dGR6tWr06FDB1544QVcXV3NYnx9fQH48ccfmT17Nnv27MHGxoYGDRqwfPly7OzsOHHiBCtXruSPP/7g%2BvXrlC5dmlq1atGpUycGDRqU70F86enprF%2B/np07d3LhwgUyMzOpWLEigYGBjBgxgpo1a5rdPzIykhdeeIEmTZqwdu1aVq9ezbZt24iKisLe3p4GDRowZMgQOnXqpLKphRDib0kCLIQQRSAqKsr0u5eXl%2Bn3efPmsXTpUgBcXV3x8fEhLi6OPXv2sGfPHkaOHMlrr71m1taff/7JsGHDuH79OnZ2dlSvXp0qVaoQExPDqVOnOHXqFDt27GDz5s04Ozs/MJYJEyZw7NgxfHx8uHXrFh4eHtjZ2fHNN98QHh6OXq%2BnXLly1K1bl9TUVH777TdOnDjB9u3bWb9%2BvVkSfOPGDYYPH86lS5cAqFmzJs7Ozly8eJENGzawbds2Zs%2BeTbdu3R4YR2ZmJqNGjeLnn3%2BmXLly1KlTh8uXL3Po0CEOHTrE22%2B/zcCBA7VteCGEyI9BCCGEkujoaIOPj4/Bx8fHcOjQoUfe9/XXXzf4%2BPgYGjRoYIiPjzcYDAbDF198YfDx8TG0aNHC8OWXX5rum52dbdixY4ehadOmBh8fH8PGjRvN2nr%2B%2BecNPj4%2BhpCQEENsbKxZ3NatWw316tUz%2BPj4GNasWWMWlzvWhg0bGg4fPmwwGAyGrKwsQ2JioiErK8vQpk0bg4%2BPj2Hp0qUGvV5vivv9998NrVq1Mvj4%2BBg%2B/fRT03q9Xm/o1auXwcfHx9C5c2fD6dOnTbclJycbpk6danrMx48fN9126NAh01iaNm1q2L59u%2Bm2pKQkw9ChQw0%2BPj4Gf39/Q2Zm5iO3qxBCqJA5wEII8Zikp6fzxx9/8NZbb7Ft2zYAhg0bhru7OxkZGSxcuBCAd999l549e5ridDod3bp1M1V%2BFy5ciF6vByAhIYHz588DMGPGDDw9Pc3ievfujb%2B/PwBnz57Nd1xdu3alZcuWANjY2ODm5satW7eIj48HICQkBFtbW9P9GzRoQHh4OJ06dcLNzc20/uuvv%2Bb06dOULl2apUuXUq9ePdNtLi4uzJw5k3bt2pGZmcmCBQvyHUtYWBg9evQw/V2mTBnT4759%2BzaXL19%2ByNYVQgh1MgVCCCEKwQsvvPC393nuueeYOHEiYDwbxM2bN3F2dn7oxTR69uzJjBkziI2N5Y8//qBx48ZUqFCBQ4cOkZ6ejoODwwMxWVlZpikK6enp%2BbbbvHnzB9aVK1cOV1dX7ty5w6RJk3jppZdo0qQJNjbGOklISAghISFmMXv37gUgODiYatWq5dvX8OHDOXjwIIcPHyY5OdlsLjRAhw4dHoipU6eO6fekpKR82xVCCC0kARZCiEJw/4UwdDodpUuXxs3NDV9fXzp16kTdunVNt%2BdWcTMzMxk8ePBD27W1tSU7O5tLly7RuHFj03oHBweuX7/OiRMnuHLlCtHR0Vy8eJHTp0%2BTlpYGQHZ2dr5tenh45NvPpEmTePPNN9m/fz/79%2B/H1dWVgIAA2rRpQ/v27c3mLgOm6myDBg0eOv7c27KysoiKiqJhw4Zmt1esWPGBmLyJfVZW1kPbFkIIVZIACyFEIfj3v//90Ath5Cc5ORmAjIwMjh49%2Brf3z1sJvXTpEnPnzmX//v1mSa6LiwstWrQgLi6OM2fOPLSt/CrHYKzy1qhRgxUrVvDTTz9x584dvvnmG7755ht0Oh3t27fn7bffNiXCKSkpAA9UdfPK%2B6Hg/rNfgPFy0Y9ikGs1CSEeA0mAhRCiGDg6OgLGCumWLVsKHJeQkMDzzz9PQkIClStXJiQkhPr161O7dm2qVq2KTqfj1VdffWQC/CgBAQEEBASQnp7OL7/8wpEjRzh48CCnTp1i3759XL9%2BnW3btqHT6UxnmMhN5vOTN3HP74wUQghRHCQBFkKIYlCrVi3AeEozvV6Pnd2DL8cGg4HIyEi8vLyoXLkypUqVYvPmzSQkJODm5sbmzZspX778A3GxsbEWjycjI4Po6GhSUlJo0qQJDg4OtG3blrZt2xIeHs6OHTt45ZVXOHPmDGfPnqVevXrUrl2bP/74g1OnTj203ZMnTwLGKSHVq1e3eFxCCPE4yFkghBCiGLRs2ZIyZcqQmpr60ArwV199xdChQ%2BnatSs3btwA4OrVqwBUrlw53%2BT3woULHD9%2BHLBs/uyBAwfo1q0bo0ePJiMj44HbAwMDTb/ntpt7ANvevXuJjo7Ot91Vq1YB0LRpU8qWLVvg8QghxOMkCbAQQhQDJycnRo8eDcCsWbPYvHmz2Xze7777jrfeegswnrYst3pau3ZtAM6cOcPu3btN9zcYDBw4cICRI0eSmZkJwN27dws8nqCgIMqVK8ft27eZPHkyt2/fNt2WmprKnDlzAKhUqRLe3t4AdOnSBV9fX%2B7du8eoUaPMpl2kpKTw5ptv8sMPP2BnZ8ekSZMKvnGEEOIxkykQQghRTEaNGkV0dDQbN25kypQpvP/%2B%2B1StWpXY2Fji4uIAaNasGTNnzjTF9OvXj3Xr1hEVFUVYWBhVqlShXLlyXL9%2BnYSEBOzt7fH39%2Bfw4cMWTYUoVaoUH330ES%2B%2B%2BCI7d%2B5kz549VK9eHRsbG6Kjo0lLS8PR0ZHZs2dTqlQpAOzs7FiyZAmjRo3i0qVL9OrVy%2BxKcLmnanvnnXdo0aJF4W48IYTQQBJgIYQoJjqdjhkzZtC5c2fWr1/P8ePHTReWaNq0Kc888wz9%2B/c3JZxgPKvCpk2bWLp0Kfv27ePq1avcvHkTLy8v2rdvz9ChQ3FycqJTp06cOXOGa9euUbly5QKNJyAggP/85z%2BsWLGCX3/9lT///BM7Ozu8vLxo27YtI0aMeKCtqlWrsnnzZr744gu%2B/vprLl68yI0bN6hUqRLt2rVj8ODB1KxZszA3mxBCaKYzyDlmhBBCCCGEFZE5wEIIIYQQwqpIAiyEEEIIIayKJMBCCCGEEMKqSAIshBBCCCEKza1bt3j66aeJjIx86H32799Pjx49aNq0KV27dmXfvn1mty9dupSgoCCaNm3KkCFDuHTpUqGOURJgIYQQQghRKH799Vf69%2B/PlStXHnqfP//8kwkTJjBx4kR%2B%2BeUXJkyYwMsvv2w6dePWrVtZvXo1n332GZGRkTRo0ICwsDAK87wNkgALIYQQQgjNtm7dyqRJkwgPD//b%2B7Vo0YJOnTphZ2dHt27daNmyJRs2bABg48aNDBo0CG9vb0qXLs2rr77KtWvXHllRtpQkwEIIIYQQVi4uLo5Tp06Z/eRekKeg2rZty7fffku3bt0eeb8LFy7g4%2BNjtq5u3bqmq0nef7u9vT01a9Y0u9qkVnIhDPHk0%2BnU4mrVgvPnwdsbLl%2B2ODw5Se2rFp0OnJ0hNRVUv625c8fyGFtb8PKCGzcgK0utX9Xx2tpCpUpw/bpa33muAGwROzuoXBmuXQO93vL4CxfU%2BnVwgMBA%2BOknSE9Xa6NuXctjtD5eACcntTgbGyhfHm7dUv9/FVe/Dg6WxxTG8zjP1aQLTOtzCeDcObU4rft1x7pRah0Xxo7t4mJ5jI0NuLkZ/1GqO1eFCmpxWqm%2BLz7Cho8/ZtGiRWbrxo8fz4QJEwrchoeHR4Hul5qaiqOjo9k6BwcH0tLSCnR7YZAEWPzvcnMzvpu4uRVptzrdXz9FeZkZGxtjnzY26m%2BcJa3vvP0WJXt7Y7/29uoJsIrierz3913UCXBx9GuNz%2BPi2q%2BLbcfO%2B08W9O/fn%2BDgYLN1BU1oLeXo6Ej6fTtZeno6zs7OBbq9MEgCLIQQQghRkjyGDwuenp54enoWerv58fHx4dSpU2brLly4QMOGDQHw9vbm/PnzdOjQAYDMzEz%2B/PPPB6ZNaCFzgIUQQgghRJHp2bMnhw8fZufOnej1enbu3Mnhw4fp1asXAM8%2B%2Byxr1qzhzJkz3Lt3j/nz5%2BPu7k6LFi0KbQxSARZCCCGEKEmKYx6URn5%2Bfrzzzjv07NmTOnXqsHjxYubNm8fUqVOpUqUKCxcupFatWgD069eP5ORkxo0bx61bt2jUqBGffvop9vb2hTYeSYCFEEIIIUqSEpAAnz171uzvY8eOmf3drl072rVrl2%2BsTqdjxIgRjBgx4rGN78nfgkIIIYQQQhQiqQALIYQQQpQkJaAC/KSTLSiEEEIIIayKVICFEEIIIUoSqQBrJgmwEEIIIURJIgmwZpIACyVDhgzB39%2BfPn360LFjRxwdHdHpdBgMBuzs7Khfvz5hYWGFes4%2BIYQQQojCIAmwKBT//e9/qVq1KgDJycmsXr2a4cOHs2LFCkmChRBCiMIkFWDNZAuKQlemTBnGjh3LP//5T%2BbNm1fcwxFCCCGEMCMVYPHYdOjQgUmTJnH37l0cHR0LFBMXF0d8fLzZOo9atfB0c7N8APXqmS8tpPoBOzdOywd0lYvd2NmZL1UYDGpxWvsurn7LlFGLc3IyX6ooVcrymML4H6vG2tqaL4tKYfSr8lwsyc/jYtuvVXZqKJwHrbKDaN25srLU4gqDVIA1kwRYPDblypXDYDCQlJRU4AR4w4YNLFq0yGzd%2BIkTmTBxovpA1q1TCnNW7xGAAj7k/PvW0HmFCuqxWrm7F0%2B/Hh5qcZUqaeu3YUNt8apUH29hKFvWuvotruexlueSl5d6LGjZrzU%2BoYprx1b9xJCQULjjsIQkwJpJAiwem4SEBGxtbXF1dS1wTP/%2B/QkODjZb59GjB3z%2BueUDqFfPmPwOGgRnzlgcnnrwqOV9YnxdcnSEu3chO1upCVJSLI%2BxszMmvwkJoNer9aulEuvuDjdvqvWtpV8PD4iPV%2Bs3OlqtXycnY5Lw%2B%2B%2BQlqbWRrVqlsdofbwADg5qcba2xiQ0KaloC1%2BF0a9KYbIwnsfJyZbHaH0uAVy5ohandb/2r3ZdrePC2LFVPqnY2hqT3%2BTk4q3mimIhCbB4bPbt20ezZs1wsOAd19PTE09PT/OVly9rG8iZM3DfNcgLQvVNL2%2B8ahuZmer96vXq8aqJqNa%2BtW5rvR4yMiyPU0lQ8kpLU29DZby5VB8vaPuWGYx5gmqOUlz9annMJfF5XGz7tZadGrTt2KrTL8C4c5W0BFgqwJpJAiwK3Z07d1i9ejX79u1j5cqVxT0cIYQQ4n%2BLJMCaSQIsCsUzzzyDTqcDwNnZmaZNm7JmzRoaFtckSSGEEEKIh5AEWChZvXq16fezZ88W40iEEEIIKyMVYM1kCwohhBBCCKsiFWAhhBBCiJJEKsCaSQIshBBCCFGSSAKsmWxBIYQQQghhVaQCLIQQQghRkkgFWDPZgkIIIYQQwqpIBVgIIYQQoiSRCrBmkgALIYQQQpQkkgBrJgmweOIlJxmU4mxswBlIPXiU7GzL48uU1Sn1i58fHD2Kc7tmcOyYUhNlypa1PKhJEzhwgIrPBcGJE0r9HvnujlKckxN4eUFCAqSlWR6v1yt1i7MzVKoE8fGQmmp5vIuLWr%2BOjn8tDWq7J8ePWx7j6mp8vGfPwh21f5XScyG37%2BBg4y6t0rfq/9jNDZ5%2BGn75BW7fVmvjuU6JlgfZ2gJlcc5KgqwspX7LlC9leZCNDeCIl%2Btd5X%2BWXX1ntbicjKBWLbX/139/raHUb9myEFQJDpyvRFKSUhPGf5dCv23awI%2Bn3JT77dpVLU4UP0mAhRBCCCFKEqkAayZbUAghhBBCWBWpAAshhBBClCRSAdZMEmAhhBBCiJJEEmDNZAsKIYQQQgirIhVgIYQQQoiSRCrAmskWFEIIIYQQVkUqwEIIIYQQJYlUgDWTBFgIIYQQoiSRBFgz2YJCCCGEEMKqSAVYCCGEEKIkkQqwZrIFgbVr1%2BLr68vKlSuLrM%2BFCxfi6%2BvLK6%2B88sBtGRkZtGrVCl9fXwCuXbuGn58f165dK7LxCSGEEEL8r5IEGGMCPHDgQFatWoVery%2ByfsuVK8d3331HcnKy2fq9e/eSmZlp%2Brty5cocO3aMypUrF9nYhBBCCPGEsrEp/B8rY32P%2BD4///wzCQkJvPHGG2RnZ7N7927TbYmJiYSHh9O8eXM6duzI6tWrqV%2B/PlevXgXgypUrhIaGEhAQQIcOHViwYAEZGRkF7tvb25tatWqxc%2BdOs/WbN2%2Bme/fupr%2BvXr2Kr6%2BvqV9fX19Wr15N586d8fPzY8CAAZw9exaAyMhIU%2BU41xtvvMEbb7wBGCvPEydOZPLkyTRr1oygoCB27drF4sWLCQwMxN/fnyVLlphifX19iYyMNP29ZcsWgoODTX0FBwezbNky2rRpQ/Pmzfnggw/Ys2ePaWwTJkywaJsIIYQQ4m9IAqyZ1c8BXr16NSEhITg4ODBo0CCWL19uSj4nTZqETqdjz549ZGdnM2nSJLKysgBIS0tj2LBhdO/enY8%2B%2Bohbt24RFhZGdnY2r776aoH779OnD1u3bqV///4AxMbGcvLkSYYMGcKGDRseGrdjxw7WrFmDg4MDYWFhzJ07l88%2B%2B6xAfe7evZsPP/yQ2bNnM3/%2BfF599VWGDh3K/v372b9/P%2BPGjaNXr15UqVLlb9uKiYkhPj6e77//np9%2B%2BonRo0fTpk0bNm7cSFJSEs8%2B%2Byw7d%2B6kd%2B/eBRpbXFwc8fHxZuucnDzw9PQsUHxeuc9n5ee1n59aXL165ksVLi6Wx/j4mC8VODmpxTk4mC8tlfO0spijo/nSUjqdWpzWxwvg6mp5TO5uobJ75MrOVovT2rfq/7hMGfOlEltby2M0v4AoxubulDqdct92iu/suXGq8WXLqsUVxn6t8i92djZfWiopSS1OPBmsOgGOiYnh4MGDTJs2DYCQkBAWL17M4cOHqVGjBj/88AO7du3Czc0NgClTppiS4%2B%2B//56MjAxeeeUVdDodlSpVYuLEiYSFhVmUAPfs2ZN58%2BZx%2BfJlatWqxZYtW%2BjWrRulS5d%2BZNyQIUPw8PAAoGvXrnz66acF7rNu3bp06dIFgDZt2rB06VJCQ0Oxt7c3VXevXbtWoAQYYMyYMdjb29O2bVsABg4ciKurK66urnh7e5sq1wWxYcMGFi1aZLZu3LjxhIVNKHAb91NNjjh6VLlPANat0xavatky5dAGGruuU0djA4q8va2r3xYtiqdfAH//4um3VSst0YqZGWjLyrTQ8OnKQ/U1L0e5cor9emjrt1kzbfGqmjZVi9u1q3DHYRErrNgWNqtOgNetW4der6dXr16mdXq9nuXLlxMaGgpA1apVTbdVq1bN9HtMTAy3bt2iZcuWpnUGg4HMzEwSEhKoUKFCgcZQvnx52rdvz7Zt2wgPD2fr1q18%2BOGHD8wLvp%2B7u7vpdzs7OwwGQ4H6A0wJPYBNzpPINacclft3tgVlonI5r5a2OR/By%2BYpA9jY2Fg0tv79%2B5uS8FxOTh6kpha4iTx9G5Pfu3fVql7O7RRfjevVMya/gwbBmTNqbahWgJctg5Ej4dw5pW5PRRxQinNwMCa/Fy9Cerrl8VoqwN7ecP688f9sKS0V4Nx%2BVR4vQEKC5TEuLsbk95dfICVFrV8tFWB/fzh8WK1vLRXgVq3g0CH4m5fFh3o6QKFUZ2NjfNApKeobzd7e8hidzriDpaeDBa%2BdecWnqGXAdnbG5DcxEVQOhzl9WqlbXFyMye/Ro%2Br7tWoFuGlTOH4cpfcYUbJZbQJ87949Nm3axKxZswgMDDStP3fuHKNHj2bMmDGAMdGtVauW6fdcXl5eVK9ena%2B//tq0LiUlhYSEBMqXL2/RWPr06cOMGTMIDAzE2dmZ%2BvXrm827tURuEpqRkUGpUqUA41zmcnk%2B0usseNe3sbExOyAvMTHxgftY0t7f8fT0fGC6Q3Ky%2BvsPGGOV4o8dU%2B8UjMmvahuq3yWCMfk9cUIpNC1NvVswvmertKH12NO7d9XewLQWUVQfL8CdO%2Br9pqSox2t5LmnpW%2Bv/ODkZbt9WDFbNvsG4wVTjtUy9MBiU/1lat7Ver9aG1ikBKSnqbahs6lypqSVwOoNUgDWz2i341VdfodPp6NGjB15eXqafoKAgfHx82LJlCx06dOD999/nzp073Llzh7lz55riO3ToQGpqKsuWLSMjI4OkpCQmT55MeHi4xQnhU089RWZmJjNnzqRfv36aHlf16tWxs7Njx44dAPz0008cOnRIub06deqwe/du9Ho9V65cYdOmTZrGJ4QQQgiN5CA4zazvEedYt24dPXr0wD6fr6j69%2B/Pl19%2ByaxZs9DpdLRv354%2BffpQv359AOzt7XFxcWHlypVERkYSFBREp06dsLGxISIiwuKx2NnZ0bNnT6KionjmmWc0PS5PT0%2BmTJnCkiVLaNasGWvWrKFv377K7b311lucOnUKf39/Xn75Zc0JuhBCCCH%2BNyUkJDB27FhatGhBQEAAs2bNyvf0siNHjsTPz8/sx9fX13RM1s2bN/H19TW7/f7pkVrpDJZM0LQyP/74I82bN8ch52CEs2fP0rt3b44fP/63B6mJwqM678/GxjjHKzVV7ZvEMmUVp3b4%2BRknszVrVrRTIJo0gQMHIChIeQrEke/Uvld3coIGDeDUqaKdAuHsDI0bw2%2B/Fe0UCCcnaNQITp5UnwJx44blMa6u0L49fP990U%2BBcHWF4GDYu7dop0C4ucHTT8O336pPgXiu04NTt/6Wra3xeZiUpD4FImcamkW0HrwAXE9SO62BnZ3xQLb4eLX/16%2B/KnVL2bLGl60DB4p2CkTZstCmDfz4o3q/XbuqxWnWrl3ht3nwoOYmhgwZQsWKFZkxYwY3b97kpZdeonfv3owcOfKRcZs2bWLRokVs3LgRT09P9u3bx4wZM9i7d6/mMT2M1VaAC2LOnDlERESg1%2BtJSUkhIiKCwMBASX6FEEIIIfKIiori8OHDvPbaazg6OlKtWjXGjh3L2rVrHxl36dIlZsyYwbx580zHAJ08eZKGDRs%2B1vFa7UFwBTF//nxmzpxJq1atsLGxoV27dmbzgB9mxYoVfPzxxw%2B9vUePHkyfPr0whyqEEEIIa/EY5uzmdx5%2BD4%2BCn4f//PnzuLm5UbFiRdO6OnXqcO3aNZKSkszOEJXXO%2B%2B8Q%2B/evWmR5/yOJ0%2Be5M6dOzzzzDPcvHmTRo0aMXnyZOrWravwyPInCfAjeHt78/nnn1scN3z4cIYPH/4YRiSEEEIIq/cYEuD8zsM/fvx4Jkwo2Hn4U1NTcbzvxPu5f6elpeWbAP/yyy%2BcOHGCefPmma0vW7YsdevWZdSoUZQqVYqPPvqI4cOHs3PnTspouirOXyQBFkIIIYSwcvmdh9/DgqubODk5cfe%2BE7Ln/u38kMvtbdiwga5duz7Qz/z5883%2B/te//sXmzZv55Zdf6NChQ4HH9CiSAAshhBBClCSPoQKc33n4LeHt7c3t27e5efOm6WJdFy9exMvLK9%2BqrV6vZ8%2BePSxevNhsfUpKCosXL%2Bb55583XZE2KysLvV5vOilBYZCD4IQQQgghhCY1a9akefPmvPvuu6SkpBAdHc2SJUseevrUs2fPcu/ePZrddw1sFxcXfvrpJ%2BbMmUNycjKpqanMmDGDqlWrms0T1koSYCGEEEKIkuQJvRDGxx9/jF6vp2PHjoSEhNCuXTvGjh0LgJ%2BfH9u3bzfdNzo6GldX13zPrLVkyRKys7Pp1KkT7dq1Iz4%2BnqVLl%2BZ77QZVMgVCCCGEEKIkeUKv3Obu7v7Qs2Adu%2B%2B8%2BF26dKFLly753rdKlSoPHJBX2J7MLSiEEEIIIcRjIhVg8cRTveKVvb3xSmEpKZCZaXl8GZWrsQG4uPy1VG1D5bJEKSl/LRUva3TvnlIYdjmvJBkZ6m2oyL1AV1aW2sW6VC7UBX9ddcrW9q/HbimVi3zlxmRnq1/RrTB2a5Xrh6pexS230KXpW1o3N8VAQMspl1T/SaC%2BcwIZN9W7BeNV4DIytLVR1FSeh4XxPC42T2gFuCQpaf9yIYQQQgjrJgmwZrIFhRBCCCGEVZEKsBBCCCFESSIVYM1kCwohhBBCCKsiFWAhhBBCiJJEKsCaSQIshBBCCFGSSAKsmWxBIYQQQghhVaQCLIQQQghRkkgFWDPZgkIIIYQQwqpIBVgIIYQQoiSRCrBmsgWBtWvX4uvry8qVK4usz4ULF%2BLr68srr7zywG0ZGRm0atUKX19fzf1s376d7t27a25HCCGEEE%2BI3GuDF%2BaPlbG%2BR5yPtWvXMnDgQFatWoVery%2ByfsuVK8d3331HcnKy2fq9e/eSmZlZKH307NmTHTt2FEpbQgghhBD/C6w%2BAf75559JSEjgjTfeIDs7m927d5tuS0xMJDw8nObNm9OxY0dWr15N/fr1uXr1KgBXrlwhNDSUgIAAOnTowIIFC8jIyChw397e3tSqVYuI5TMEAAAgAElEQVSdO3eard%2B8efMDVdu9e/cyYMAAWrduTZMmTXj%2B%2Bef5888/AZg2bRqdOnUiNTUVMCb0rVq1IjY2li1bthAcHAxAZGQkwcHBLFu2jDZt2tC8eXM%2B%2BOAD9uzZQ%2BfOnfHz82PChAmmxzBkyBAWLlxoGsPVq1fx9fU1PX5fX182bNhA586dadKkCaGhofz%2B%2B%2B8MGDAAPz8/nn32WaKiogq8PYQQQghRAFIB1szq5wCvXr2akJAQHBwcGDRoEMuXLzcln5MmTUKn07Fnzx6ys7OZNGkSWVlZAKSlpTFs2DC6d%2B/ORx99xK1btwgLCyM7O5tXX321wP336dOHrVu30r9/fwBiY2M5efIkQ4YMYcOGDQDcuHGDiRMn8tFHHxEcHExiYiLjx49n8eLFvP/%2B%2B0yZMoV%2B/frx/vvvM2DAAObOncvChQupWLHiA/3FxMQQHx/P999/z08//cTo0aNp06YNGzduJCkpiWeffZadO3fSu3fvAo3/q6%2B%2BYsOGDWRkZNC9e3fGjh3LihUrqFSpEi%2B%2B%2BCKffPIJ7733XoG3R1xcHPHx8WbrDAYPPDw8C9xGLjs786XFmjRRi/PxMV%2BqSEmxPKZePfOlAmdntThHR/NlUdHab%2BnSanEODuZLFa6ulse4uJgvVajGOjmZL4tKmTLmS/H3SpVSi9P6mlm2rFpcYezXKo9Z6z5935e3ooSx6gQ4JiaGgwcPMm3aNABCQkJYvHgxhw8fpkaNGvzwww/s2rULNzc3AKZMmWJKjr///nsyMjJ45ZVX0Ol0VKpUiYkTJxIWFmZRAtyzZ0/mzZvH5cuXqVWrFlu2bKFbt26UzvPOXL58eXbs2EH16tVJSUnhxo0blCtXjtjYWAAcHBz44IMPCAkJ4fvvv2fYsGEEBQU9tM8xY8Zgb29P27ZtARg4cCCurq64urri7e1tqvAWxPPPP2/aPt7e3tSvX586deoA0KpVK3799dcCtwWwYcMGFi1aZLZu3LjxhIVNsKidvCpUUAw8cEC5TwCWLdMWr2rdOuVQP41da8i9S2S/tWurx9avrx7r768eq1XDhsXTr7bHrNMQqiHW1rZYYitVUu8WwMOjePpt1kxbvKrGjdXivv22cMdhESus2BY2q06A161bh16vp1evXqZ1er2e5cuXExoaCkDVqlVNt1WrVs30e0xMDLdu3aJly5amdQaDgczMTBISEqhQwKyrfPnytG/fnm3bthEeHs7WrVv58MMPzeYF29vb89///pf169ej0%2Bnw8fEhJSUFuzwf0318fGjZsiU//PADzz777CP7LFeuHAC2OS%2BwZfN8bLexscFgMBRo7IAp%2Bc1tzzVPWcvStgD69%2B9vmrKRy2DwICfXt4idnTH5TUgAlandFZ97%2BIeIR/LxMSa/I0fCuXNqbahWgNetg0GD4MwZpW6PfXZUKc7R0dj9mTNw965SE8XSr5YKcO3acOkSpKertXHjhuUxLi7GRPDwYbVdJLcNFU5OxuT3998hLc3yeNVqWZkyfz1m1TY6Blv2OmSi04GFr2FmsrPV4mxtIefbRhXX49SSZzs7Y/IbH6/2mnn%2BvFK3uLgYk9%2BjR9X3a9UKcOPG8Ntvavt0sZIEWDOrTYDv3bvHpk2bmDVrFoGBgab1586dY/To0YwZMwYwJrq1atUy/Z7Ly8uL6tWr8/XXX5vWpaSkkJCQQPny5S0aS58%2BfZgxYwaBgYE4OztTv359IiMjTbfv2rWLNWvW8MUXX1CjRg0AZsyYwbk8ydXOnTs5ceIETz/9NK%2B//jpr1641Jbj30xWwomFjY2N2MF5iYqJyWwXl6emJp6f5dIerV0HLMYF6vWL8iRPqnYIx%2BVVtIylJvd8zZ%2BDYMaXQnGnkyu7e1d5GUfarmp/kSk9XT/jv3FHvNyVFPV5LPgfGREElEb19W1u/ycna27AWFhyKki%2B9Xq0NLS9bYNyvVdtQ/TAL6vu0KNms9iPEV199hU6no0ePHnh5eZl%2BgoKC8PHxYcuWLXTo0IH333%2BfO3fucOfOHebOnWuK79ChA6mpqSxbtoyMjAySkpKYPHky4eHhFieFTz31FJmZmcycOZN%2B/fo9cHtycjI2NjY4ODhgMBg4cOAA27ZtMyWnMTExvPXWW7z55pu8%2B%2B67xMXFPTCNQEWdOnU4ePAgSUlJJCcns3TpUs1tCiGEEEIjOQhOM%2Bt7xDnWrVtHjx49sLe3f%2BC2/v378%2BWXXzJr1ix0Oh3t27enT58%2B1M%2BZtGdvb4%2BLiwsrV64kMjKSoKAgOnXqhI2NDRERERaPxc7Ojp49exIVFcUzzzzzwO19%2BvQhMDCQ7t2706pVKyIiIhg6dCiXL18mIyODSZMm0bp1a3r06IGLiwvvvvsu//d//8eRI0cs3zB5jBkzhgoVKtCxY0d69er1wNQEIYQQQoiSSGewdJKmFfnxxx9p3rw5DjmHep89e5bevXtz/Phxs4PUxONlwTF5ZuztoWJFiI1VmwJRtYHCYfpgPHvEgQMQFFS0UyD8/IyT6Jo1U54C8cNBtZcDZ2dj98eOFe0UCK39qp49wtHReBDbH3%2BoT4G4csXyGFdXCA6GvXvVp0CoHqmvdS6u6vQFNzfo2BH27FFv49m%2B1jUHOOqq2hzgUqWMB7Jdv642BeLkSaVuKVvW%2BHJ54EDRToEoUwZatYJDh9SnQDz9tFqcZsOHF36bK1YUfptPMKutABfEnDlziIiIQK/Xk5KSQkREBIGBgZL8CiGEEKL4yBQIzaz2ILiCmD9/PjNnzqRVq1bY2NjQrl07s3nAD7NixQo%2B/vjjh97eo0cPpk%2BfXphDFUIIIYQQBSQJ8CN4e3vz%2BeefWxw3fPhwhj%2BOryeEEEIIIaywYlvYZAsKIYQQQgirIhVgIYQQQoiSRCrAmkkCLIQQQghRkkgCrJlsQSGEEEIIYVWkAiyEEEIIUZJIBVgz2YJCCCGEEMKqSAVYPPFUL8SUG2cwqLVx5Du1S205OUED4FTEAdLSlJrg3j3LY5ydwQ849tlR5auxtW2nUwvMuQqd34uKV6ELClLr19sbli3Db/FIOH/e8vj4eLV%2B//EP2LyZ%2Bm8%2BC6dPKzXR3M3N8iAfHwheSfCqYXDunFK/1KmjFlejBvjPxH/7vyEqyvJ41Z2ydm3oOI%2BOuybBpUtqbbRfanmMra3xMnR37qhflU3lcn%2BOjlCvnnF/VrzMYA0nJ6U4DKWBmlS696fSi1CNOSPV%2BvXxgaDPCPr8RfX92t3d8pjataHVfFr951X1fevprWpxWkkFWDNJgIUQQgghShJJgDWTLSiEEEIIIayKVICFEEIIIUoSqQBrJltQCCGEEEJYFakACyGEEEKUJFIB1kwSYCGEEEKIkkQSYM1kCwohhBBCCKsiFWAhhBBCiJJEKsCayRYUQgghhBBWRSrAQgghhBAliVSANZMEWAghhBCiJJEEWDOr3IKXL19m8uTJBAUF4efnR6dOnZg3bx6pqteqv88nn3zCyJGK10R/gvj6%2BhIZGVncwxBCCCFECZCQkMDYsWNp0aIFAQEBzJo1C71en%2B99R44cSaNGjfDz8zP9HDhwAICsrCzmzJlDYGAgfn5%2BvPTSS8TFxRXqWK0uAT569Ch9%2BvShSpUqbNu2jWPHjrF06VJOnDjBiBEjyMrK0txHaGgoy5YtK4TRCiGEEELcx8am8H8Kwcsvv4yTkxMHDx5k06ZN/Pzzz6xcuTLf%2B/7%2B%2B%2B989tlnHDt2zPQTFBQEQEREBD/%2B%2BCObN2/m4MGDODg48O9//7tQxpjL6hLgadOm0bt3b8LCwihfvjwAtWrVYsGCBVSoUIHo6GjAmCi/8MILtG3blkaNGtG3b1%2BOHz8OQGRkJMHBwURERNCuXTv8/f2ZMGECKSkpACxcuJAhQ4YAsGXLFgYOHMjMmTNp1aoVrVu3ZurUqWRmZgJgMBhYtWoVnTt3pkWLFgwaNIjff//9oeP39fVl9erVdO7cGT8/PwYMGMDZs2dN4/L19TW7/xtvvMEbb7xhGtfEiROZPHkyzZo1IygoiF27drF48WICAwPx9/dnyZIlZvE//PADXbt2JSAggLCwMOLj4023nTp1iiFDhtCyZUv%2B%2Bc9/snLlSgwGg6mvESNG8Oyzz%2BLv78%2BRI0cU/ltCCCGEKAmioqI4fPgwr732Go6OjlSrVo2xY8eydu3aB%2B4bHR3NnTt3qF%2B/fr5t/ec//2HUqFFUqlQJFxcXpk6dyoEDB0w5WmGwqjnAV65c4fz587z99tsP3Obu7m5K/tLT03nppZcICwtj4MCBpKenM2XKFObOncu6desAiImJITY2lm%2B//ZbY2FgGDx7MunXrGD169ANtHz16lKCgIA4ePMjp06cZOnQogYGBdO/enXXr1rFixQoiIiKoU6cOX375JcOHD2fXrl24u7vn%2Bzh27NjBmjVrcHBwICwsjLlz5/LZZ58VaBvs3r2bDz/8kNmzZzN//nxeffVVhg4dyv79%2B9m/fz/jxo2jV69eVKlSBYD9%2B/ezbNkyXF1dee2115g0aRKff/45sbGxDB06lPDwcJYvX05UVBRjx47FwcGBAQMGAPDzzz%2BzfPlyGjduTOnSpQs0vri4OLMkGyA72wMPD88CxedlZ2e%2BtJSTk1qcg4P5UoXKmB0dzZdK/PzU4urVM19ayttbLa56dfOlpR7yHPtbtWqZL1WUKWN5TI0a5ksVVauqxVWqZL60VHq6WlzOa5FpqcLWVj1GJTaXypMx97WygK%2BZj2zDUqVKmS8t5eOjFqf1eQzg5mZ5jNZ969IltbjC8BjmAOf3/uvh4YGnZ8Hef8%2BfP4%2BbmxsVK1Y0ratTpw7Xrl0jKSmJsmXLmtafPHkSZ2dnwsPDOXnyJO7u7gwbNox%2B/fqRnJzMjRs38MmzP7m7u%2BPq6srZs2epVq2axkdqZFUJ8K1btwAemljmsre3Z8OGDdSoUYN79%2B4RExODm5sbJ0%2BeNLvfuHHjcHBwoEaNGgQEBHD58uV823NwcCA0NBSdTkfjxo3x9fU13Xft2rWMGTOGejmJQ79%2B/di0aRPbt29nxIgR%2BbY3ZMgQPDw8AOjatSuffvppgbdB3bp16dKlCwBt2rRh6dKlhIaGYm9vT3BwMADXrl0zJcBhYWGm319//XW6dOlCbGws27dvp06dOgwePNjU7osvvsiaNWtMCXC1atVo3bp1gccGsGHDBhYtWmS2bty48YSFTbConbxUcxwvL%2BUuAahTR1u8KtUcFICjR7V1nvMBschNm1Y8/c6bVzz9vvNO8fQLMG5c8fQbHl48/ap8UMmlkpTl0vLhSqvKldXiCliIeai33tIWr%2BqVV9Ti%2BvQp3HFY4jEkwPm9/44fP54JEwr2/puamorjfR/6cv9OS0szS4AzMjJo2rQp4eHheHt7ExkZyYQJE3B2dsYvpxDjdF8VysHBodCO1QIrS4Bzk8b4%2BHhq1qz5wO03b97E3d0dW1tbIiMjGTVqFGlpadStWxc7OzvT1/v3twfGpPn%2B23NVqFABnU6X731jYmKYM2cO8/K8ker1eho2bPjQx5E3gc9vXI/ilucF2SbnCeTq6mr2d3Z2tuk%2BVfNUiyrnvCjGxsYSExPDqVOnaNGihen27OxsbPNUSwr6qTGv/v37mxLxv9r14MYNi5vCzs6Y/N68CQ%2BZg/9ICQmWx4Cx8lunDly8qF70ysiwPMbR0Zj8njkDd%2B%2Bq9ev3YjO1wHr1jMnvoEHGAViqmWK/1asbk9/p0%2BHKFcvjExPV%2Bq1Vy5j8TpoED/ng%2B7dUK8DvvGNMFKKi1PrVUgEeNw4WL4br1y2P11IBDg%2BHBQsgJkatDZW5g7a2xv9RcjKoHhui8sJVurRx/7p8Ge7dU%2BtX9eunUqWMye%2B1a2ovQjNmqPVbvbpxn37nHbXnMahXgF95BT74QH3f%2Bh%2BS3/tv3jzn7zg5OXH3vjef3L%2BdnZ3N1vfu3ZvevXub/m7bti29e/dm165dBAYGmsXmSk9Pf6AdLawqAa5SpQo%2BPj7s3LmTli1bmt2WkJBAhw4deO%2B996hWrRozZsxg/fr1pkR0%2BfLlD63wauHl5UVYWBjdu3c3rbty5YpZolpQuclnRkYGpXK%2BwkpMTKRcuXKm%2B%2BRNxAsiLi7OVJ3OnXtTtWpVvLy8CAgIMJt6kZiYaPbpzNK%2BwJg03584R0dDzpRpJXq9WnxamnqfYHy/V21D9X0PjMmv8ofkY8fUOwZj8qvShpYqGxjfNM%2Bftzzuvq/7LHb5Mpw%2BrRarpToYFQXnzqnFaj3Q9/p1teRba%2BUmJkb9K2ctjzkrSz1e9ZMoGF8EVOMVXnvNZGSovQip7pO5rlxRb0P1qz7Qtm8Vl8dQAc7v/dcS3t7e3L5921RMBLh48SJeXl6Uue81ftOmTTg7O9O1a1fTuoyMDEqXLo2rqysVK1bkwoULpmkQ8fHx3L5922xahFZWdxDcm2%2B%2ByebNm1m0aBGJiYkYDAZOnz5NaGgoDRo0oHPnziQnJ2NjY4NDzqfo48ePs2rVKjJUPhH/jZCQECIiIrh48SIABw8epHv37koHjVWvXh07Ozt27NgBwE8//cShQ4c0jW/hwoXExsZy584dZs%2BezT//%2BU/Kly9Pjx49OH78ONu3b0ev1xMXF0doaCizZ8/W1J8QQggh/sYTeBaImjVr0rx5c959911SUlKIjo5myZIl9OvX74H7pqSkMGPGDP744w%2Bys7P5/vvv%2Be9//0v//v0B6Nu3LxEREURHR5OSksK7776Lv78/1bXME7%2BPVVWAAfz9/VmzZg2ffPIJ3bt35%2B7du7i7u9OlSxfGjBmDvb09bdq0YdCgQQwePJjs7GyqVq3KkCFDmD9/Pjdv3izU8QwbNgyDwcDYsWOJi4ujYsWKTJs2jY4dO1rclqenJ1OmTGHJkiXMmDGDVq1a0bdv3we%2BRrBEu3btCAkJIT09nQ4dOjBlyhTAWE1ftmwZ8%2BbNY%2BbMmdja2tK%2BfXumTp2q3JcQQgghSq6PP/6Y6dOn07FjR2xsbOjduzdjx44FwM/Pj3feeYeePXsydOhQ0tLSGD9%2BPAkJCVSrVo05c%2BaYplWOGzcOvV7P4MGDSU1NJSAggA8//LBQx6ozWDKBVIhioHrWE3t744FsN26oTYFQmb4HxrNHNGgAp04V7RQIZ2fjSRyOHVP/trltO8WvTv38jAfQNWumNgUi59yPFvP2hmXLYOTIop0C8Y9/wObN8OyzRTsFwscHVq6EYcPUvypWPTqzRg2YOdM4n7Yop0DUrv3XfGvVr6mXLrU8xtbW%2BD%2B6fVt9CoTKfNbCmMyvegqb0qWhZk3480%2B1FyHVC0D5%2BBgPoHvxxaKdAlG7NsyfD6%2B%2Bqr5vbd2qFqfVe%2B8Vfpv/%2Blfht/kEs7opEEIIIYQQwrpZ3RQIIYQQQogS7TEcBGdtJAEWQgghhChJJAHWTLagEEIIIYSwKlIBFkIIIYQoSaQCrJlsQSGEEEIIYVWkAiyEEEIIUZJIBVgzSYCFEEIIIUoSSYA1kwRYPPGys9Xici/xYjCotaHXq/Wbe878rCz1NoqNlgtSgPFCGPdd871ADhxQ6zc52bg8elTtAhyVKqn1m5Ly1/L2bbU2vLwsjyld%2Bq9lzqXaLXbhglpc7htudLRaG6pPZBcX4zIuDmJi1NpQuSKNvb1xmZ6udiWdvG1Yws7ur6VKPPy1f1oq98UrLU3tIhyq%2B5ajo3Gpum%2Bp9p17cZaTJ40/wqpIAiyEEEIIUZJIBVgz2YJCCCGEEMKqSAVYCCGEEKIkkQqwZpIACyGEEEKUJJIAayZbUAghhBBCWBWpAAshhBBClCRSAdZMtqAQQgghhLAqUgEWQgghhChJpAKsmSTAQgghhBAliSTAmskWFEIIIYQQVkUqwFbm3r17JCYm4qVyGVYhhBBCFD%2BpAGv2RG/By5cvM3nyZIKCgvDz86NTp07MmzeP1Nzrd2v0ySefMHLkyEJp636%2Bvr5ERkY%2Blra1GDRoED/99FNxD0MIIYQQotg8sQnw0aNH6dOnD1WqVGHbtm0cO3aMpUuXcuLECUaMGEFWVpbmPkJDQ1m2bFkhjLbkSExMLO4hCCGEEEILG5vC/7EyT%2BwjnjZtGr179yYsLIzy5csDUKtWLRYsWECFChWIjo4GjInyCy%2B8QNu2bWnUqBF9%2B/bl%2BPHjAERGRhIcHExERATt2rXD39%2BfCRMmkJKSAsDChQsZMmQIAFu2bGHgwIHMnDmTVq1a0bp1a6ZOnUpmZiYABoOBVatW0blzZ1q0aMGgQYP4/fffC/RYLl68yJgxY2jfvj2NGzemW7du7Nu3D4CrV6/i6%2BvL7NmzadmyJe%2B88w4Aq1atokOHDgQEBBAeHs6ECRNYuHAhABkZGXz00Ud07NgRf39/Ro0aRVRUlKm/devW0alTJ1q0aEGPHj34z3/%2BA8CIESO4du0ab731FtOnT39gnAaDgf/7v/%2BjR48etGjRgpYtW/Lqq6%2BSnp4OQFZWFh9%2B%2BCFt2rQhMDCQt956iwEDBrBlyxYAUlJSmD59Ok899RStW7cmPDycmzdvFmgbCSGEEKKAJAHW7ImcA3zlyhXOnz/P22%2B//cBt7u7uLFmyBID09HReeuklwsLCGDhwIOnp6UyZMoW5c%2Beybt06AGJiYoiNjeXbb78lNjaWwYMHs27dOkaPHv1A20ePHiUoKIiDBw9y%2BvRphg4dSmBgIN27d2fdunWsWLGCiIgI6tSpw5dffsnw4cPZtWsX7u7uj3w8EyZMoGPHjixatAiDwcC8efN4%2B%2B236dChg%2Bk%2Bqamp/Pjjj6Snp7Njxw4WLVrEJ598QqNGjdi4cSPTp0/Hx8cHgAULFnDo0CFWrlyJp6cnS5cuZcSIEezcuZO4uDjee%2B89vvzyS2rXrs3BgwcZN24cTz31FMuXLyc4OJjx48fTt2/fB8a5a9cuVq1axZo1a6hZsyYXL15k0KBBfPXVVzz33HN89tlnbN%2B%2Bnc8//5zq1auzcOFCjh07RkhICABTpkwhNTWVLVu24ODgwOzZsxk/fjxffPEFOp2uQP/7uLg44uPjzdZlZXng4eFZoPi87OzMl5ZydlaLc3Q0X6pQ%2BYKjMPrF21strnp186WlkpPV4urVM19aysNDLa5uXfOlipo1LY%2BpVs18qSLnA63FatQwX1rKYCiefgHs7S2P0foCAuDgYHlM6dLmSxWqyUzueFXGDdCokVpcYTyfiqPfkycLbyyiyD2RCfCtW7cA/jaxtLe3Z8OGDdSoUYN79%2B4RExODm5sbJ%2B/bKceNG4eDgwM1atQgICCAy5cv59ueg4MDoaGh6HQ6GjdujK%2Bvr%2Bm%2Ba9euZcyYMdTLeaPt168fmzZtYvv27YwYMeKR4/z000%2BpWLEiBoOBmJgYypYtS2xsrNl9evfuTalSpShVqhSbNm2if//%2BNGvWDIDBgwezdetWwFilXb9%2BPR9//DHVct4Ex40bx8aNG/n%2B%2B%2B9p1KiR6T6dO3emdevWHD9%2BHJsCvCAGBQXRrFkzvLy8uHXrFomJibi5uZnGumnTJkaPHk3dnBeLl19%2B2TSuhIQEdu/eza5du6hQoQJgTIhbtGjBqVOnaNiw4d/2D7BhwwYWLVpktm7cuPGEhU0oUHx%2BVHOcSpWUuwTUc0mtVHNBALROCZo2TVu8qpwPvEVu8eLi6fff/y6efgHy%2BfaoSMyYUTz9/s370GOj5UOOVrVrq8V98422fnOKW0VOtV%2BtbxJaWGHFtrA9kQmwR07GEh8fT818qiQ3b97E3d0dW1tbIiMjGTVqFGlpadStWxc7OzsM91UaPPJkQPb29g/cnqtChQpmlcq8942JiWHOnDnMmzfPdLtery9QYnfmzBnGjh1LfHw8derUoXz58g%2BMwdPzrwrn9evX6dy5s9ntucnurVu3SEtLY%2BLEiWZJbWZmJjExMXTu3JnVq1ezbNkyQkNDycrKom/fvrz22muU/puKgsFgYMGCBezbt4/y5cvzj3/8g8zMTNNYr1%2B/TpUqVUz3t7W1pXLlyqbtA5iqwXnvc/Xq1QInwP379yc4ONhsXVaWB9evFyjcjJ2dMfmNjwe93vL4%2BwrRBeboaEx%2Bz5%2BHu3fV2lCtANerB2fOqPfrt1jxoNDq1Y3J7/TpcOWK5fFHj6r1W6%2BeMfkdNMj4wC2lpQK8eDGMGwcXLqi1oVoB/ve/YeZMyJkGZjEtFeDp043/5zxTrgpMSwV4xgx48021fgHmzrU8xs7OmPzevKn2AgKQM93OIqVLG//P0dFw755avxkZanEODsbk99Iltf3k5ZfV%2Bq1b15iEjh2r/nwqSf2KJ8ITmQBXqVIFHx8fdu7cScuWLc1uS0hIoEOHDrz33ntUq1aNGTNmsH79elOCtXz58odWeLXw8vIiLCyM7t27m9ZduXIFNze3R8bFxsYyceJEFi1aZErsdu/ezTf3fVLOm3hXqVKFa9eumd1%2B7do1ateuTbly5ShdujTLly%2BnadOmptsvXbpExYoVSUhIICsri8WLF5Odnc3Ro0cJCwujVq1aDB48%2BJFjnTdvHteuXWPv3r24uLgA0KNHD9PtlStXNhuXwWDgek5mWrFiRcA4jSLvB44LFy6YkveC8PT0NPswAMb3PNXXczC%2Bd6nEaz3ZyN276m1oOcZTS7%2BcP6/eMRiTX5U2jh3T1u%2BZM2ptaK3gXLig/jVozvEFSqKj1f9Xqp%2BOckVFwblzlsdlZ2vv9%2BxZtVgt21qvV49X/bABxuRXNV41cc6Vnq62n2idEqDl%2BVQS%2B9VCKsCaPbFb8M0332Tz5s0sWrSIxMREDAYDp0%2BfJjQ0lAYNGtC5c2eSk5OxsbHBIWe%2B0vHjx1m1ahUZWrKlhwgJCSEiIoKLFy8CcPDgQbp3786RI0ceGZeamkpWVhaOORMzL1y4wOKcr00fNs6QkBA2btzIb7/9hl6vZ/PmzaYD%2B2xsbOjXrx/z58/nxo0bZGdns3XrVp555hmioqK4du0aI0aM4Oeff8bGxsaUmIA3Gd8AACAASURBVJYrVw6AUqVKkfyQ%2BZYpKSmULl0aW1tb7t27x/Llyzl37pzpQMD%2B/fubPmBkZGSwePFi4uLiAGMC3L59e2bNmkViYiKZmZlERETQr18/kpKSCrydhRBCCPE35CA4zZ7ICjCAv78/a9as4ZNPPqF79%2B7cvXsXd3d3unTpwpgxY7C3t6dNmzYMGjSIwYMHk52dTdWqVRkyZAjz588v9LMPDBs2DIPBwNixY4mLi6NixYpMmzaNjh07PjKudu3avP7667z22mvcvXsXLy8vQkJCeP/99zl37ly%2BFeTOnTtz5coVxo4dS0ZGBkFBQTRs2BD7nAM5Jk%2BezMKFCxk0aBC3b9%2BmWrVqfPzxx9SvXx8wnkHj7bffJi4ujjJlyjBo0CC6du0KGOcuL1iwgJMnT5pN5wDjnN5//etfBAYG4uTkRPPmzenVqxfncqo9Q4cOJT4%2BngEDBmBra0u3bt3w8vIyjWvu3LnMnz%2Bf3r17k5KSgre3N8uWLTOrCAshhBBCFDed4WETYkWxOXPmDGXKlDGbb9u3b18GDBjwwBzbonTixAmqVKliOjjRYDDQqlUrPvjgA9q0afPY%2BlWd9leqlPEb7uvX1aZA3DcLpcCcnaFxY/jtt6KdAuHsDH5%2BxpkAqv22nfqUWqC3t/EAupEj1b6aP3BArV8/P%2BP84WbNinYKRKNGsHs3dO6s/tWpytGK3t7w6acwZkzRT4Hw8YHPP4ehQ4t2CoSvL6xaBS%2B8oD4FYtMmy2Ps7cHLC27cUJ8CceeO5TEODsa5qRcuFP0UCEdHqF8f/vhDbT955hm1fhs1Mh5A989/Fu1UhMLoV%2BUAlcKQc3rTQvXcc4Xf5hPM%2BmreJcChQ4cIDQ0lPj4eg8HAzp07uXDhAq1bty7WcX311Ve8/vrrJCcno9frWbFiBYDZXGQhhBBCiCfdEzsFwpo9//zzxMTE0KdPH1JTU6lduzYREREWHUz2OLz88stMnz6dp59%2BmoyMDBo0aMBnn32Gs%2BoJc4UQQghhOSucs1vYJAF%2BAtnZ2TF16lSmTp1a3EMx4%2BLiwlyV0wkJIYQQovBIAqyZbEEhhBBCCGFVpAIshBBCCFGSSAVYM9mCQgghhBDCqkgFWAghhBCiJJEKsGaSAAshhBBClCSSAGsmCbB44l24oBZXpozxOgfR0fCQqz8/kouLWr863V9L1deoUqUsjyld%2Bq%2Bl6jUHiI9Xi8u5OAqJiWptqF6QIvcqgx4eam2onsTey8u4jI9Xb6NmTctjci9wcO%2Be%2BkUSVHeO3GsmGQxqbag8CeGvq7qkpqq3YafwVmdr%2B9dS9XpRf/5peUzZssYLYVy7BqqXkc/nCqMFkvuCde%2Be2oUwVLYzmG9r1TZUrh6U98U693dhNSQBFkIIIYQoSaQCrJkkwEIIIYQQJYkkwJrJFhRCCCGEEFZFKsBCCCGEECWJVIA1kwRYCCGEEEJolpCQwJtvvsnhw4extbWlZ8%2BeTJ48Gbt8Dm784osvWLlyJXFxcXh6evLCCy8wePBgALKzs2nevDkGgwFdngMUf/zxR5ycnAplrJIACyGEEEKUJE9oBfjll1%2BmYsWKHDx4kJs3b/LSSy%2BxcuVKRo4caXa/7777jg8%2B%2BIClS5fSpEkTjh8/zujRo3F3d6dz585cuHCBzMxMjh49SimV0yIVwJO5BYUQQgghRP5sbAr/R6OoqCgOHz7Ma6%2B9hqOjI9WqVWPs2LGsXbv2gfvGxsYyatQomjZtik6nw8/Pj4CAAI4cOQLAyZMn8fX1fWzJL0gFWAghhBDC6sXFxRF/33ncPTw88PT0LFD8%2BfPncXNzo2LFiqZ1derU4dq1ayQlJVG2bFnT%2BtypDrkSEhI4cuQI//rXvwBjAnzv/9m787io6v3x4y82WdxQkcQlF1TMSgVB1BJDKjI0cy%2BNMJdAFHBLTc0lKVs0FXC7Lhku5ZKmkF27N7Of3ZtaN81cSFEBRQUSXNhk/f0xMjGCOvM5JPLl/Xw8fByZOe/P5zOH4cz7vOdzzrl1i4EDB5KcnIyzszOTJ0/Gzc1N9eWVIQmwEEIIIURV8jdMgdiyZQtRUVEGj40fP56QkBCj4rOysrC1tTV4rOTn7OxsgwS4tLS0NAIDA3niiSfo06cPADY2NnTo0IGwsDDq1q3Lpk2bGDVqFLt376ZZs2amvrRySQIshBBCCFHNDR06lF69ehk81rDkbptGsLOzI%2BeOOwiW/FyzZs1yY44ePUpYWBju7u4sWLBAf7Lc9OnTDdYbNWoUO3bs4IcffuC1114zekz3IglwNVNYWMilS5cq7AhKCCGEEA/Y31ABdnR0NHq6Q3natGnDtWvX%2BPPPP3FwcADg7NmzNGrUiNq1a5dZf/v27YSHhxMaGsrIkSMNnlu8eDG%2Bvr60b99e/1heXh7W1tbK47uTyVvw/PnzTJs2DS8vL1xdXXn22WdZuHAhWSX3a9do5cqVZc4WrCgXL15k/PjxdO3aFU9PT4KDg7lw4YJye7169WLHjh0mP1eZJk6cyFdffVXZwxBCCCGEqofwJLgWLVrQuXNn3n//fTIzM7lw4QLLly9n0KBBZdbdu3cvc%2BfOJTIyskzyC3D69Gnee%2B890tLSyMvLIyoqiszMTJ577jnN4yxh0iv%2B9ddf6d%2B/P02aNOGrr77iyJEjrF69mt9%2B%2B42RI0dSWFioeUBBQUGsWbNGczvlGTduHHXr1mXfvn3s27cPe3t7goOD/5a%2BHlYZGRmVPQQhhBBC/B8UERFBQUEBPj4%2BDBkyhB49eujzLFdXV3bv3g1AVFQUhYWFhIaG4urqqv83e/ZsABYsWMCjjz5Kv3798PT05PDhw3z66afY29tX2FhNmgIxe/ZsXn75ZUJDQ/WPtWzZksWLFzN79mwuXLhAixYt%2BPXXX1myZAnnzp3j%2BvXrtGnThtmzZ9OpUycOHTrE22%2B/zeDBg9m8eTO3bt3C09OTBQsWUKtWLSIjIzl8%2BDAbNmxgx44dbNu2jccff5zY2FjMzMzo1asXc%2BfOxcrKiuLiYjZs2MCmTZu4evUqbdu2ZcaMGTzxxBNlxn79%2BnUcHBwICwvTX0T59ddfp1%2B/fly/fp24uLh7jqu4uJhVq1axceNGcnNzGTx4sNEJf0pKCgsWLODYsWNcvXoVBwcHxo4dqz8qcnFxwd/fn5iYGFxdXVm5ciVff/01ERERXL16lY4dO9K4cWPy8/P54IMP7vu69%2B7dS0REBFeuXMHR0ZG%2BffsSHBzMzJkz%2BeWXXzhy5AgnTpxg5cqVZca6fft2Nm/eTHJyMnl5eXTp0oUFCxZQv359AKKjo/n000/Jzs6me/fuFBQU0LZtW0JCQsjLy2PFihXs3r2bmzdv0rFjR2bNmkXz5s1NeZsJIYQQ4l4e0usAOzg4EBERUe5zR44c0f8/Jibmnu3Y29uzYMGCCh3bnYxOgJOSkjhz5gxz584t85yDgwPLly8HIDc3l7FjxxIaGsqrr75Kbm4uM2bM4KOPPmLz5s0AJCcnk5KSwr/%2B9S9SUlIYPnw4mzdv5s033yzT9q%2B//oqXlxcHDhzg1KlTBAQE0L17d/z8/Ni8eTOffvopK1aswNnZmV27dvHGG2/wzTff6OeflKhbty5r1641eGzv3r00adKEunXr3ndcX375JZ999hlr1qyhTZs2REVFceXKFaO23axZs7C3t%2Bfrr7%2BmRo0aREdHM3/%2BfHr37q2fGJ6UlMT%2B/fvJz8/nyJEjTJs2jYiICLy8vPj%2B%2B%2B%2BZMGECffv2Bbjn665VqxZvvfUWq1evxtPTk5MnTzJ8%2BHCefvpp3nvvPZKSkujSpUu5Z3UeO3aM8PBwoqOj6dChA1euXCEgIIDo6GgmTJjA119/TVRUFCtXruTJJ59k69atvPvuu7Rt2xbQzdk5ePAg69evx9HRkdWrVzNy5Ej27Nlj9Lyd8i7DkpvbkIYNTZ%2BXVHKzGNWbxtxxMqvRbGwMlyosLCqnXx57TC2uZUvDpakyM9XiWrc2XJqqUSO1uHbtDJcqbv/dmKTkYFLLQWVRkVqc1r5Vp8lpfW8BlHMXKqNjVGJL3OWs93uqVctwqeIuJxzdV8lOT3XnV07xySjOzoZLFSrva637j99/V4sTDwWj/7LT09MByiSWd7KysmLLli00b96cW7dukZycjL29Pb/f8UYZN24cNjY2NG/eHE9PT86fP19uezY2NgQFBWFmZkaHDh1wcXHRr7tp0yYCAwNpd/tDaNCgQWzfvp3du3eXO6ektM8//5x169axYsUKo8a1a9cuhgwZwuOPPw5AWFgYW7duvWcfJcLDw6lZsyZWVlZcunSJmjVrkpuby/Xr1/UJcJ8%2BfbC1tcXW1pYvv/yS559/Xn825nPPPcezzz6rb%2B9er3vYsGHY2Niwfft2ioqKcHNz43//%2Bx/mRhwttm3bltjYWJo2bcr169dJTU2lfv36pKSkALrq8NChQ/XX4Rs%2BfDg7d%2B4EoLi4mC%2B%2B%2BIKIiAj9CXbjxo1j69at7N%2B/H19fX6O2VXmXYRk3bjx%2BfsZdhqU8qvtkrdq0qZx%2BW7XSEPzll9o6X7hQW7yqZcsqp9/bB/UP3Lx5ldMvwPz5ldNvZb236tVTjzXhDPoyKvB6pyZTPbD75htt/d6x739gVPcfjRtX7DhM8ZBWgKsSoxPgkkthpKWl0aJFizLPl5z1Z2FhwaFDhxgzZgzZ2dm0bt0aS0tLiouLy20P0E9nKE%2BDBg0M7gNdet3k5GQ%2B/PBDFpbaMRYUFJQ7BaJEXl4eCxYsYM%2BePaxatYquXbsaNa7U1FScnJz0z1lYWNDYyDf/hQsX%2BOijj0hISKBFixb6KQFFpY5YS595efnyZYMzHwGaNWvGn3/%2Bed/XbWNjw%2Beff87y5cuZPHkymZmZ%2BPr6MmvWLH2l%2B27Mzc2Jjo4mJiYGOzs7XFxcyMzM1G%2BDy5cvl0lkS5Ld9PR0srOzCQsLM0i28/PzSU5ONmo7QfmXYUlIaMjhw0Y3oWdnp0t%2Bjx%2BH7GzT47VUgNu0gTNnIDdXrQ3VCnCrVnDunHq/7d8ZqBbYsqUuQZkyBe5yMHtPWirAy5bBuHEQH296/B3fNhitXTtd8jtsGMTFqbXRoYPpMc2b65LfOXMgMVGtXy0V4Pnz4Z131PrWUgHW8t4CKGe6131ZWuqS34wMKChQ6/fUKdNjatXSJb%2B//qr%2Bd1HOGfdGsbXVvbfj4uCOy1kZZcYMtX6dnXXJ7/jxcPasWhuqFWAt%2B4/KJAmwZkYnwE2aNKFt27bs2bMHDw8Pg%2BeuXr2Kt7c3CxYsoFmzZsyfP58vvvhCn4iuW7furhVeLRo1akRoaCh%2Bfn76x5KSku46STo9PZ2xY8eSl5fH9u3bTboUWKNGjQyuGFFcXExqaup94/Lz8wkMDGTSpEkMGzYMMzMzjh8/rp8IXqJ0kt%2BkSRMuXbpk8PylS5f0twS81%2BvOzMwkNTWVRYsWAXDq1CkmTZrEypUrmTZt2j3Hun79ev7zn/8QExOjr/QHBQXdd1ytWrWiXr16WFtbs27dOjp16qR//ty5cwZ3hbmf8i7DcuUK3LxpdBNlZGerxd/lmMxoublqiTdo%2B9Y1N1ftswtQ%2B8Au7fx5tTauXdPWb3y82teRly9r6zcuDkrNazOJlrkqiYlw%2BrRarNaTlRMT4Y8/TI/T8kcM6u8tUE9gS2JV42/cUO83M1M9XmtylJOjdsBy/Li2fs%2BeVW9Dy/tadf8hqjST/kreeecdvvzyS6KiosjIyKC4uJhTp04RFBTE448/jq%2BvLzdv3sTc3Byb2zv3o0ePEh0dTV5eXoUPfsiQIaxYsYKzt48YDxw4gJ%2Bfn/5e0qXl5%2BczevRoatWqxeeff27ydXAHDx7M1q1bOXLkCPn5%2BaxYsaLMXNXy5Ofnk5ubi42NDWZmZly6dImPP/5Y/9zd%2BvrXv/7FgQMHKCws5IcffuDbb7816nVnZWUxZswYYmJiKC4uxtHREXNzc%2Brd/hqvRo0a3LzLB1FmZiaWlpZYWVlRUFDArl27OHDggH6cQ4YMYevWrRw7doyCggK%2B/PJLjh49Cuiqx4MGDWLRokVcuXKFoqIidu7cSZ8%2BfUhUrVQJIYQQoqyH8DJoVY1JdaYuXbqwceNGVq5ciZ%2BfHzk5OTg4OPDCCy8QGBiIlZUVTz31FMOGDWP48OEUFRXRtGlT/P39WbRokf4r/IoyYsQIiouLCQ4OJjU1lUceeYTZs2fj4%2BNTZt3vv/%2BeEydOYG1tTbdu3Qye%2B/rrr%2B/bV58%2BfcjIyGDixIlcv36dF154ARcXl/vG2dnZ8f7777N06VLCw8Np0KABQ4YMIT4%2BntOnT9OynBM7nnzySebNm8fcuXPJyMjA3d2dbt26YWVlZdTrjoiIYMmSJcyePRsbGxtefPFFRowYAcDLL7/M3LlzOX78uP6kxBIjR47k9OnTeHt7Y21tTfv27Rk2bBgHDx4EwNfXl6SkJIKDg8nLy8PLy4snnnhCP65p06YRGRnJsGHDuHbtGs2aNSMiIqLMdA4hhBBCiMpkVny3ybei0pw/f56ioiKcS50RGxISQqtWrZg4cWKljSsuLo7atWvTpEkT/WMDBgzglVdeYciQIX9bv999pxZXuzZ06QKHD6t9%2B6p6EradHTz5pO4btQc5BcLWFtq3h5Mn1adAdPZXPFh57DHdCXQDBz7YKRBPPgl794Kv74OdAuHqqpuj6eamPgXijgNxo7RtC%2BvXw4gRD34KhIsLREfD668/2CkQWt9boLYTsbTUncSWlqY%2BBeJ//zM9pk4d8PKC//f/1KdAqF4rtWZN3Xv7yBG1KRCvvqrW7xNP6E6g6937wU6B0Lr/ALhjWuADo7rfuRdX14pv8yFW/WreVUB8fDwBAQEkJSUBcOjQIQ4cOEDPnj0rdVwHDx4kKCiItLQ0iouL2bNnD/Hx8WUq6kIIIYT4G8kUCM00nGoj/i7PPfcc8fHxvP7661y/fp0mTZowf/58/eXHKstrr71GcnIy/fv3Jysri1atWrFixQqT51MLIYQQQlQmSYAfUmPHjmXs2LGVPQwDlpaWzJw5k5kzZ1b2UIQQQojqqxpWbCuabEEhhBBCCFGtSAVYCCGEEKIqkQqwZpIACyGEEEJUJZIAayZbUAghhBBCVCtSARZCCCGEqEqkAqyZbEEhhBBCCFGtSAVYPPRat1aLq1FDt2zWDPLyTI8/elSt37p1dcurV%2BH6dbU2iorU%2Bm3fHq5cUe%2B3s%2BodpGrX/mup0kajRmr9tmjx1zI/Xz3eVG3b6pYdOoCNjVobP/1kekxurm557Jj6naAee0wtruTOYFlZand1U7094a1bfy0V28iwcTI5xsIC6gA3rBtSqPhJWa9NG9ODrK11y0cf/eu1myo9XS3OwuKvZcn/TdGunVq/pf%2BOVe%2B6Z2VlekzJ3VbbtlV7vZVJKsCaSQIshBBCCFGVSAKsmWxBIYQQQghRrUgFWAghhBCiKpEKsGayBYUQQgghRLUiFWAhhBBCiKpEKsCaSQIshBBCCFGVSAKsmWxBIYQQQghRrUgFWAghhBCiKpEKsGayBYUQQgghRLUiFWAhhBBCiKpEKsCaVest6O/vT2RkpOZ2Ro8ezcqVKytgRA%2BPyMhI/P39K3sYQgghhLiTuXnF/6tmpAJcAdasWVPZQxBCCCGEEEaqfin/XezYsYNXX32V8PBwunbtSrdu3Zg5cyb5%2BfkAFBQUsHTpUnr27ImbmxvDhw8nLi4OMKwknzlzhuHDh%2BPh4YG3tzfTpk0jMzMTgNzcXD766CN69uyJh4cH/v7%2BHDt2TD8GFxcXwsPD8fT0JCgoqMwYIyMjCQ0NZcqUKbi7u%2BPl5cWiRYv0z99Z0b548SIuLi5cvHhR3/6WLVvw9fWlY8eOBAUFcfz4cV555RVcXV0ZOHAgiYmJ%2Bvjs7GymT5%2BOp6cnvXv35quvvtI/l5eXx9KlS/Hx8aFLly6MGTPGIPZ%2Br0UIIYQQiqQCrJlUgEv59ddf8fLy4sCBA5w6dYqAgAC6d%2B%2BOn58fK1asIDY2lrVr19KyZUuioqIIDAxk3759Bm3MmzePbt26sXHjRjIyMggICGDbtm288cYbzJ07l5MnTxIdHY2TkxOff/45I0aMIDY2lsaNGwOQlJTE/v379Yn3nb799ls%2B%2BOADPvzwQ3788UcCAwPx8fGhU6dORr3GmJgYtmzZQl5eHn5%2BfgQHB/Ppp5/i5OTEqFGjWLlyJQsWLADg%2BPHj9O/fn/nz53P48GECAwNp2rQp7u7uLF68mIMHD7J%2B/XocHR1ZvXo1I0eOZM%2BePVhbWxv1WsqTmppKWlqawWOFhQ1p2NDR6DZKWFoaLk1Vt65aXK1ahksVRUWV0y9t26rFNW9uuDTV7feMyZo1M1ya6tYttTitrxcgN9f0mHbtDJcqWrbUFqcar7qtnZ0NlwosLEyPKckHNOUFKu/rGjUMlypq1lSLs7U1XJqqdWu1OK1/x6C2o2/a1HBpqrNn1eIqQjVMWCuaJMCl2NjYEBQUhJmZGR06dMDFxYXz588DsHPnTgIDA2l9%2Bw987Nix9OzZk%2BLiYoM2rK2tOXDgAM7OznTr1o1du3Zhbm7OrVu3iI2NZdmyZTS//aEZEBBATEwMsbGxvPnmmwD06dMHW1tbbO%2ByA2rRogUvv/wyAD179qRhw4YkJCQYnQC/9tpr2NvbA9CmTRvat2%2BP8%2B0Plq5du/K///1Pv%2B5jjz3Ga6%2B9BsBTTz2Fr68vu3btonPnznzxxRdERETQ7PYOa9y4cWzdupX9%2B/fj6%2Btr1Gspz5YtW4iKijJ4bNy48YSGhhjdxp0aNlSLc3JS7hIAd3dt8aq6dNEQ3Gu9ts7nzdMWr2rWrMrpt7Je7%2BbNldMvwMKFldPv0qXKoXU0dKvpgLJOC/XY20WRStGmjVrcihXa%2Bp0xQ1u8qmnT1OJefLFixyEeKEmAS2nQoAFmZmb6n62srPQJblpamr5KC1CjRo1yk84lS5YQGRnJ4sWLmTRpEm5ubsydO5e6deuSn59P0zuONJs2baqfogDg6HjvSmfDO7I5KysrikwoF5YkvwAWFhbULVXmNDc3N0jo7xyrk5MTp0%2BfJj09nezsbMLCwjAvdRSan59PcnKy0a%2BlPEOHDqVXr14GjxUWNuTyZZObwtJSl/ympUFBgenxf/xhegzoPjDd3eGXX%2BD27BeTqVaAu3SBw4fV%2B%2B0VPUItsHlzXTI4Zw6UmgpjNC0V4FmzIDwcLlwwPV5LBVjL6wUoNf3JaO3a6ZLfYcPg9hQsk2mpAC9cCFOmwO3CgEm0VICXLoWwMOWK243NsSbHmJvr/qYyM9X%2BHgHqpCeYHlSjhi75vXQJ8vLUOr5xQy3O1laX/J45Azk5pserJsDNmumS3/ffV/s7BvUK8LRp8OGHUOpzuEqQCrBmkgAbycnJiculsrD8/Hw%2B/vhjRo8erX%2BsqKiIkydPEhISwowZM7h8%2BTILFixg%2BvTpbNu2DWtray5cuKCvuIJumkDphK90Am4qc3Nzg%2BkGGRkZZdYxpf3U1FSDny9cuECTJk2oV68e1tbWrFu3zuAg4Ny5czzyyCNKfZVwdHQskzgnJqp/DoAu%2BVWJv35dvU/QfXCqtqH6gau1X06fVu8YdL8slTZsbLT1e%2BGC7kPbVCrTEEpTfb0AR46o9xsXpx6v9TWfPw%2BnTpkep5JQlXb2LJw4oRRaWKjebVGRhnjVpB90Oy3V%2BKws9X5B97tSaSM%2BXlu/Fy6ot2Flpd7vxYuVO51BVAo5hDDSgAEDWLt2LefPn6egoIBVq1bx73//m3r16unXMTc3Jzw8nCVLlnDr1i3q16%2BPtbU19erVw9zcnIEDB/LJJ5%2BQmJhIXl4en332GfHx8fj5%2BVXIGJ2dnTlw4AA3btzg5s2brF69WlN7x44d48svvyQ/P5/vv/%2Beffv2MXjwYMzNzRk0aBCLFi3iypUrFBUVsXPnTvr06WNwIpwQQggh/gZyEpxmUgE20ujRoykoKGDUqFFcv36dJ598ktWrV2N1x1HnkiVLmD9/Pk8//TRFRUV4eHgwf/58AKZOnUpkZCQjRozg2rVruLi46E%2BqqwiBgYHMnDkTHx8fateuTWhoKHv37lVur3v37nz33XeEh4fTtGlTli5dSvv27QGYNm0akZGRDBs2jGvXrtGsWTMiIiL0zwshhBDib1INE9aKVq0T4A0bNuj/P2DAAAYMGHDX5y0tLRk/fjzjx4%2B/ZzvOzs6sX7%2B%2B3P5sbW2ZOnUqU6dOLff5P%2B4z6TQkpOyJYKWvQvHII4%2BUuSZxyQlz5bVfetx3tl9eX6VZW1szZcoUpkyZUu7z93stQgghhBCVpVonwEIIIYQQVY5UgDWTLSiEEEIIIaoVqQALIYQQQlQlUgHWTBJgIYQQQoiqRBJgzWQLCiGEEEKIakUqwEIIIYQQVYlUgDWTLSiEEEIIITS7evUqwcHBuLu74%2BnpyXvvvUdBQUG56/7www/07duXTp060bt3b77//nuD51evXo2XlxedOnXC39%2Bfc%2BfOVehYJQEWQgghhKhKHtI7wU2YMAE7OzsOHDjA9u3b%2Bemnn8q9N0JCQgIhISGEhYXxyy%2B/EBISwoQJE0hJSQFg586dbNiwgbVr13Lo0CEef/xxQkNDKS4urpBxgiTAQgghhBBVy0OYACcmJnL48GHeeustbG1tadasGcHBwWzatKnMujt37sTd3Z1nn30WS0tLXnzxRTw8PNiyZQsAW7duZdiwYbRp0wZra2smT57MpUuXOHTokOZxlpA5wOKhZ2enFmd5%2B91tY/PX/01RVKTWb0lcUZF6G3XqmB5Tq9ZfS%2BWDZGdntbimTf9aFhaaHh8fr9Zvbu5fy5wc0%2BMr4pes8noBHnvM9JiS26a3bPnXazfVqVNqcTY2uuX582ptNGqk1m9e3l9Lxddcz171D8KMOrU1VJxS1UM1uXJFLa5uXd3yzz/h%2BnXT45OS1Pot2Xldkou8wQAAIABJREFUuaLeRo0a6v2mpMCFC2r9/h%2BSmppKWlqawWMNGzbE0dHRqPgzZ85gb2/PI488on/M2dmZS5cucePGDeqU%2BmCLj4%2Bnbdu2BvGtW7cmLi5O//yYMWP0z1lZWdGiRQvi4uLo2rWrya%2BtPJIACyGEEEJUIcWYVXibW7ZsISoqyuCx8ePHExISYlR8VlYWtra2Bo%2BV/JydnW2QAJe3ro2NDdnZ2UY9XxEkARZCCCGEqOaGDh1Kr169DB5r2LCh0fF2dnbk3PFNXMnPNWvWNHjc1taW3Du%2B0cnNzdWvd7/nK4IkwEIIIYQQVYjq7K17cXR0NHq6Q3natGnDtWvX%2BPPPP3FwcADg7NmzNGrUiNq1axus27ZtW06cOGHwWHx8PE888YS%2BrTNnzuDt7Q1Afn4%2BCQkJZaZNaCEnwQkhhBBCVCEl55hU5D%2BtWrRoQefOnXn//ffJzMzkwoULLF%2B%2BnEGDBpVZ96WXXuLw4cPs2bOHgoIC9uzZw%2BHDh%2BnXrx8AAwcOZOPGjcTFxXHr1i0WLVqEg4MD7u7u2gd6myTAQgghhBBCs4iICAoKCvDx8WHIkCH06NGD4OBgAFxdXdm9ezegOzlu2bJlrFq1Cg8PD5YvX05kZCQtb5/oO2jQIEaMGMG4cePo2rUrJ0%2BeZNWqVVhZWVXYWGUKhBBCCCFEFfJ3TIGoCA4ODkRERJT73JEjRwx%2B7tGjBz169Ch3XTMzM0aOHMnIkSMrfIwlpAIshBBCCCGqFakACyGEEEJUIQ9rBbgqkQRYCCGEEKIKkQRYO5kCIYQQQgghqhVJgKuhhISEyh6CEEIIIRQ9jJdBq2qqTALs7%2B9PZGSk5nZGjx7NypUrK2BE9xYZGYm/v//f3o%2Bp9u3bx6hRoyp7GEIIIYQQlabazQFes2ZNZQ%2BhUl27do3i4uLKHoYQQgghFFXHim1FqzIV4NJ27NjBq6%2B%2BSnh4OF27dqVbt27MnDmT/Px8AAoKCli6dCk9e/bEzc2N4cOHExcXBxhWks%2BcOcPw4cPx8PDA29ubadOmkZmZCejuOf3RRx/Rs2dPPDw88Pf359ixY/oxuLi4EB4ejqenJ0FBQfccb3FxMf/4xz/o27cv7u7ueHh4MHnyZP19rqdPn05oaCi9e/ema9euJCUlcfHiRUaNGoWbmxsvvPAC69evx8XFRd/miRMn8Pf3x8PDg%2Beff57169frE9uUlBRGjx5Nly5d8PLyYvz48aSmpnLo0CHmzJnDpUuXcHV1JSUlpcxYz549S2BgIM888wwdOnTgxRdf5Pvvv9c/f/LkSV599VVcXV3p168fK1asMLh3%2BH//%2B18GDRqEu7s7fn5%2B%2BoteCyGEEKJiyBQI7apsBfjXX3/Fy8uLAwcOcOrUKQICAujevTt%2Bfn6sWLGC2NhY1q5dS8uWLYmKiiIwMJB9%2B/YZtDFv3jy6devGxo0bycjIICAggG3btvHGG28wd%2B5cTp48SXR0NE5OTnz%2B%2BeeMGDGC2NhYGjduDEBSUhL79%2B/XJ95388033xAdHc3GjRtp0aIFZ8%2BeZdiwYcTExDB48GAADhw4wJYtW2jUqBE1a9bkpZdeokOHDvz4449kZGQwbtw4fXspKSkEBAQwceJE1q1bR2JiIsHBwdjY2PDKK6/wySef0KhRI1asWMGtW7cIDQ3lH//4B7NmzWLevHlERUWV2RYlQkJC8PHxISoqiuLiYhYuXMjcuXPx9vYmMzOT0aNHM3ToUD777DPOnz9PUFAQZmZmAMTFxTF27Fg%2B/vhjfHx8%2BO233wgODqZevXp3vdj1nVJTU0lLSzN4zNKyodL9yS0sDJemqltXLa5WLcOlljZMYWdnuFTSvLlanJOT4dJU5orH4iXjVR236rchWvsFyMoyPeb2XZL0SxU2Nmpx7doZLk3l4KAW17q14bIqsbY2PaZGDcOlisraebVvrxbXqpXhUoXKHcK0/j2dOqUWJx4KVTYBtrGx0SdfHTp0wMXFhfPnzwOwc%2BdOAgMDaX17hzl27Fh69uxZ5qt/a2trDhw4gLOzM926dWPXrl2Ym5tz69YtYmNjWbZsGc1vf8AFBAQQExNDbGwsb775JgB9%2BvTB1tYWW1vbe47Vy8sLNzc3GjVqRHp6OhkZGdjb2xtUYDt16kTbtm0B%2BN///kdCQgLbtm3Dzs4OOzs7Jk6cqO939%2B7dODs7M3z4cABat27NqFGj2LhxI6%2B88grW1tb8/PPPfP3113Tr1o01a9ZgbmSCsWrVKh555BGKi4tJTk6mTp06%2BnHu27cPCwsLQkJCMDc3x8XFhdGjR7N27VoAvvjiC3x8fHj%2B%2BecBcHNzY8iQIWzatMnoBHjLli1ERUUZPDZu3HhCQ0OMii9PnTpqcaUK20q6dNEWr%2BqJJzQEdwnX1nmpA7UH6t13K6ff%2BfMrp9%2BFCyunX4DNmyun3%2BXLK6ff2wf4Slq0UI%2B9XWh54P2C%2Bs5L605z0SJt8ao%2B/FAt7sknK3YcJqiOFduKVmUT4AYNGugrjwBWVlb6BDctLU1fpQWoUaMGnTp1KtPGkiVLiIyMZPHixUyaNAk3Nzfmzp1L3bp1yc/Pp2nTpgbrN23alIsXL%2Bp/NrYqWVxczOLFi/n%2B%2B%2B%2BpX78%2Bjz32GPn5%2BQYJeem2rly5Qr169bArVcorPZbk5GROnDiBu7u7/rGioiIsbpc6Z82axapVq1i7di3Tp0%2BnXbt2zJo1y2D9u4mLiyM4OJi0tDScnZ2pX7%2B%2BfpxXrlyhcePGBsl0s2bNDMZ18OBBg34KCwt59NFHjdpOAEOHDjWYUgG6CnBGhtFN6FlY6JLfGzegsND0%2BDvu2mi0WrV0nx%2BHD8PtGTVKbZjKzk6X/B4/DtnZav122T1LLdDJSZf8LlsGly%2BbHn/hglq/zZvrkt/ZsyEx0fR4LRXg%2BfPhnXfU%2BgX1CvDChTBlCtw%2B4DeZaly7drrkd9gwuD2lzCRaKsDLl0NwMMTHq7Wxd69anJmZ%2BnsE1N4bNWrokt9LlyAvT63fc%2BfU4rTuvFRPVG/VSpf8Tp6sPnbVCvCHH8K0aep/F6LKqrIJ8L04OTlxudSHcH5%2BPh9//DGjR4/WP1ZUVMTJkycJCQlhxowZXL58mQULFjB9%2BnS2bduGtbU1Fy5cwNnZWR%2BTlJRkkJyZGVkZWLhwIZcuXWLfvn3Uup3Z9O3b12Cd0m01btyY9PR0cnJy9NXlS5cu6Z9v1KgRnp6e%2BsorQEZGBlm3P1BPnjzJ0KFDCQkJIT09nWXLljF%2B/HgOHjx4z3GmpKQQFhZGVFSU/nXu3buXb7/9Vj%2BuS5cuUVxcrB/vnePq378/75aqxqWmppp00p2jo2OZA4u0NCgoMLqJMgoL1eKvX1fvE3SfH6ptaPnMzc6GmzcVg1WTuRKXL6u1oZrYlEhMhNOnTY/TWkZJTIQ//lCLVf4lofuwVv36VevXtnFxakeHjRpp6zc%2BHn7/XVsbD9qtW%2BqxeXnq8ZW18zp5Ulu/586pt6FlyoiWv6dKIhVg7arkSXD3M2DAANauXcv58%2BcpKChg1apV/Pvf/6ZevXr6dczNzQkPD2fJkiXcunWL%2BvXrY21tTb169TA3N2fgwIF88sknJCYmkpeXx2effUZ8fDx%2Bfn4mjyczMxNra2ssLCy4desW69at4/Tp03edO9yxY0dat27NBx98QE5ODikpKUREROif79u3L0ePHmX37t0UFBSQmppKUFAQH3zwAQArV65k/vz5ZGZmUqdOHWxtbfWv3drampycHArKyQizsrIoLCzUJ93x8fEsW7YMgLy8PHr16kVxcTErV64kLy%2BPc%2BfOGSThgwYNIjY2lh9//JGioiISEhJ47bXXWLduncnbTAghhBDlk5PgtPs/mQCPHj2avn37MmrUKDw9Pfnll19YvXo1Vnd8RbJkyRLOnj3L008/Tffu3bl58ybzb8/pmzp1Kk8//TQjRozA09OTb775Rn9SnakmTJhAbm4u3bt3p1evXhw9epR%2B/fpx%2Bi4VK3NzcyIiIkhISKBbt24EBATg4eGhH3%2BTJk1Ys2YNW7ZsoXv37vTr149WrVrpE%2BB3332XoqIifHx88PDw4LfffmPp0qUAeHh40KBBAzw8PPjjjspVq1atmDp1Km%2B99RadO3cmLCyMgQMHYmVlxenTp7Gzs2P58uV89913dOnShUmTJvHUU0/px9WxY0c%2B%2BeQTPvnkEzw8PHjttdfo1asXkydPNnmbCSGEEEL8XarMFIgNGzbo/z9gwAAGDBhw1%2BctLS0ZP34848ePv2c7zs7OrF%2B/vtz%2BbG1tmTp1KlOnTi33%2BTuTxzuFhPx10lazZs3YuHHjXdctSVxL5ObmcvnyZdatW6ef17tv3z5iYmL067i6urJp06Zy23N0dNRXbst7LjY29q5jGTVqVJkbZQQEBAC6aRb5%2Bfls375d/9yGDRv0l5gDeOaZZ3jmmWfu2r4QQgghtKmOFduK9n%2ByAlzVWVlZMWHCBLZu3UpRURFXr15l3bp1eHt7V%2Bq4CgsLCQgI4IcffgDg4sWLbN68udLHJYQQQghhCkmAH0IWFhYsW7aMnTt34uHhQd%2B%2BfWnTpg3Tp0%2Bv1HE5ODiwZMkSFi5ciKurK8OHD8fX11durSyEEEI8QDIHWLsqMwWiunF3d2fr1q2VPYwynn32WZ599tnKHoYQQghRbVXHhLWiSQVYCCGEEEJUK1IBFkIIIYSoQqQCrJ1UgIUQQgghRLUiFWAhhBBCiCpEKsDaSQIshBBCCFGFSAKsnSTAQtxFOXeLNkph4V9L1TauXVOLA7h5U0N8VpZaXG7uX0uVNlT35sXFfy1V2rh5U63fkteYlaXeRk6O6TG3bv21VIkHaNRILc7B4a%2BlShtXrqj16%2BSkW/75p3obKnGWltCwoa5f1T/kGzdMj7Gz0y2zsiA7W63fWrXU4kr6trP7a0dmipo11fq1tf1rqdqGjY3pMaVfb%2B3aav2KKksSYCGEEEKIKkQqwNpJAiyEEEIIUYVIAqydXAVCCCGEEEJUK1IBFkIIIYSoQqQCrJ1UgIUQQgghRLUiFWAhhBBCiCpEKsDaSQIshBBCCFGFSAKsnUyBEEIIIYQQ1YpUgIUQQgghqhCpAGsnFWAhhBBCCFGtSAJcDSUkJFT2EIQQQgihqKio4v9VN5oSYH9/fyIjIzUPYvTo0axcuVJzO8b4/PPP8fX1xdXVFV9fXzZt2qTc1o4dO%2BjVq5fJz1WmkydP0qdPn8oehhBCCCEUSQKs3UMxB3jNmjUPpJ9///vffPLJJ6xevZqOHTty9OhR3nzzTRwcHPD19X0gY6hsN2/eJD8/v7KHIYQQQghRaSpsCsSOHTt49dVXCQ8Pp2vXrnTr1o2ZM2fqk62CggKWLl1Kz549cXNzY/jw4cTFxQGGleQzZ84wfPhwPDw88Pb2Ztq0aWRmZgKQm5vLRx99RM%2BePfHw8MDf359jx47px%2BDi4kJ4eDienp4EBQWVGWNKSgpjxoyhU6dOmJmZ4erqiqenJz///LN%2BHIsWLWL48OG4urrSu3dv9uzZo48/e/Ys/v7%2BuLq60rdvX06ePGn09tm%2BfTsDBgzA09MTV1dXAgMDSU9PByAyMpKRI0cycOBAunTpws8//0xGRgYTJ06kc%2BfO%2BPj4sGHDBtq3b8/FixcBSEpKIigoCE9PT7y9vVm8eDF5eXkAZGZmMnHiRDw9PXnqqacYNWoUZ8%2Be5cKFC4wZMwYAV1dXjhw5Uu42mjBhAr169aJjx474%2BPiwfft2/fMXL15k1KhRuLm58cILL7B%2B/XpcXFz0z584cQJ/f388PDx4/vnnWb9%2BPcXFxUZvJyGEEELcm1SAtavQCvCvv/6Kl5cXBw4c4NSpUwQEBNC9e3f8/PxYsWIFsbGxrF27lpYtWxIVFUVgYCD79u0zaGPevHl069aNjRs3kpGRQUBAANu2beONN95g7ty5nDx5kujoaJycnPj8888ZMWIEsbGxNG7cGNAlhvv37y%2B3yjl8%2BHCDn69evcrPP//M22%2B/rX9s69atfPrpp7Ru3Zply5Yxe/ZsfHx8MDc3JzAwEC8vL9asWUNSUhJjxozB3Pz%2BxxDHjh0jPDyc6OhoOnTowJUrVwgICCA6OpoJEyYA8NNPP7Fu3To6dOiAtbU1gYGBmJmZ8d1331FUVMSUKVMoLCwEIDs7mxEjRuDn58fSpUtJT08nNDSUoqIiJk%2BezLp168jMzOSHH37A3Nyc2bNns3DhQlasWMHq1at5/fXXy01%2BAWbNmoW9vT1ff/01NWrUIDo6mvnz59O7d29sbGwIDAykQ4cO/Pjjj2RkZDBu3Dh9bEpKCgEBAUycOJF169aRmJhIcHAwNjY2vPLKK/fdTgCpqamkpaUZPGZp2RBHR0ej4kuzsDBcmsreXi2udm3DpQoj3lZ/S7%2B0aqUW16SJ4dJUtWqpxTVvbrg0VVaWWlzLloZLFbdumR7j7Gy4VHH7QNlkrVsbLk3l5KQW166d4VKFpcJHXUmMSmwJOzvTY2xsDJcqbn9WmKxkvCrjBvXfUYsWhksVNWqYHqN1/3H6tFqceChUaAJsY2NDUFAQZmZmdOjQARcXF86fPw/Azp07CQwMpPXtnefYsWPp2bNnmeqgtbU1Bw4cwNnZmW7durFr1y7Mzc25desWsbGxLFu2jOa336wBAQHExMQQGxvLm2%2B%2BCUCfPn2wtbXF1tb2nmNNS0sjMDCQJ554wmBOrK%2BvL%2B3btwegf//%2BrFy5kqtXr3Lx4kUuX77M1KlTsba2pk2bNrzxxht89tln990ubdu2JTY2lqZNm3L9%2BnVSU1OpX78%2BKSkp%2BnWaNWtGt27dAF0i%2BeOPP/LNN99gfzsLmzFjBn5%2BfgDs37%2BfvLw8Jk2ahJmZGU5OToSFhREaGsrkyZOxsbEhLi6Or776iqeeeor333/fqEQdIDw8nJo1a2JlZcWlS5eoWbMmubm5XL9%2Bnbi4OBISEti2bRt2dnbY2dkxceJE/bbfvXs3zs7O%2BgON1q1bM2rUKDZu3Gh0ArxlyxaioqIMHhs3bjyhoSFGxZenTh21uOeeU%2B4SgK5dtcWr6tJFQ7DPQm2dT5yoLV7V/PmV0%2B9CjdtL1dKlldMvwPLlldPv5s2V02%2B9euqxDRuqx2o5yNGqQwe1OK07vfff1xavat48tbju3St2HCaojhXbilahCXCDBg0wMzPT/2xlZaVPcNPS0vRVWoAaNWrQqVOnMm0sWbKEyMhIFi9ezKRJk3Bzc2Pu3LnUrVuX/Px8mjZtarB%2B06ZN9dMCAKMqhUePHiUsLAx3d3cWLFiAZakj/IaldlgljxcVFZGSkkK9evWwKXVU/uijj963LwBzc3Oio6OJiYnBzs4OFxcXMjMzDZL/0uO%2BfPmy/rWVaNasmf7/ycnJpKen4%2BHhoX%2BsuLiY/Px8rl69ypgxY6hRowbbt2/n3XffpVmzZkyePJnnn3/%2BvmO9cOECH330EQkJCbRo0UJ/sFFUVMSVK1eoV68edqWqA6XHmJyczIkTJ3B3d9c/VlRUhIUJJdihQ4eWOXnQ0rIhGRlGN6FnYaFLfm/cUCuI/PKL6TGgq8B27QoHD8LNm2ptqFaAu3SBw4fV%2B/X5ZopaYJMmuuR38WJITjY9PjVVrd/mzXXJ7zvvQGKi6fFaKsALF8KUKXD7IN9kqhXgpUshLAzOnlXrV0sFePlyCA6G%2BHjT4//8U63fdu10ye%2BwYXB72pzJ9u41PcbSUpf8ZmRAQYFavyrvaxsb3e/57FnIzVXrV3UHYGenS36PHYPsbNPjIyLU%2Bm3RQpf8zpgBqlcpUq0Az5sHc%2Bao7T8qkSTA2j2wk%2BCcnJz0iR1Afn4%2BH3/8MaNHj9Y/VlRUxMmTJwkJCWHGjBlcvnyZBQsWMH36dLZt24a1tTUXLlzAudSRcVJSkkHCVDoBL8/27dsJDw8nNDSUkSNHmjT%2B9PR0srKyqFmzJgBXrlwxKnb9%2BvX85z//ISYmBgcHB4Ayc5RLj7vkQCE5OZmWt79iTS6VVDRq1IhHH32Uf/7zn/rHMjMzuXr1KvXr1%2BePP/6gV69ejBgxgps3b7J582YmTpzIwYMH7znO/Px8AgMDmTRpEsOGDcPMzIzjx4%2Bze/du/bjS09PJycnRV9gvXbpkMC5PT0/Wrl2rfywjI4MsE5IMR0fHMgcxaWnqnz%2BgS35V4q9dU%2B8TdJ9Bqm2oJMAV0S/nzql3DLrkV6UNlaS5tMRE%2BOMP0%2BNUE4US58/DqVNqsTk56v2ePQsnTqjFqiZVJeLj4fffTY8zcn95V3FxcJepW/elZQdSUKAer5JElsjNVY/X%2Br7OzlZrQ/UApURCgnobWqaMJCbKdIZq6IFdB3jAgAGsXbuW8%2BfPU1BQwKpVq/j3v/9NvVJfL5mbmxMeHs6SJUu4desW9evXx9ramnr16mFubs7AgQP55JNPSExMJC8vj88%2B%2B4z4%2BHj91ID72bt3L3PnztWfdGYKV1dXWrZsSXh4ODk5OSQmJrJu3TqjYjMzM7G0tMTKyoqCggJ27drFgQMH7no1BkdHR7y9vfn444%2B5fv06169f56OPPtI/7%2B3tTVZWFmvWrCEvL48bN24wbdo0Jk6ciJmZGdu2bWPq1KlcvXqVWrVqUatWLezs7KhRowbW1taA7moQd8rPzyc3NxcbGxvMzMy4dOkSH3/8sf65jh070rp1az744ANycnJISUkhotQRf9%2B%2BfTl69Ci7d%2B%2BmoKCA1NRUgoKC%2BOCDD4zezkIIIYS4NzkJTrsHlgCPHj2avn37MmrUKDw9Pfnll19YvXo1VlZWBustWbKEs2fP8vTTT9O9e3du3rzJ/Nvz%2B6ZOncrTTz/NiBEj8PT05JtvvtGfVGeMqKgoCgsLCQ0NxdXVVf9v9uzZ9421sLDgH//4B6mpqXTv3p3Ro0fj4%2BNjVL8jR47EyckJb29vevTowe7duxk2bBin73HE%2Bd5772FmZsYzzzxD//799fOSraysqFWrFuvXr%2BfQoUN4eXnx7LPPYm5uzooVKwCYNGkSzZs3x8/PDzc3N3bs2MHy5cuxtrambdu2dO7cmR49evDDDz8Y9GlnZ8f777/PsmXLcHV15fXXX%2Bepp57CwcGB06dPY25uTkREBAkJCXTr1o2AgAA8PDz0v8MmTZqwZs0atmzZQvfu3enXrx%2BtWrWSBFgIIYSo5rKzs3n77bfx9PSkc%2BfOTJ069Z7fEO/du5d%2B/frh5uZGr169iIqKoqhUpt67d286duxokM%2BdNWFqmFmxXKPqofSf//yHzp076%2Bcc//HHH7z88sscPXpUX8V90HJzczly5AhdunTRz%2Bvdt28fc%2BbM4cCBA39bv3dcFMJoWqfw7d%2Bv1q%2B9ve4Eun/968FOgbC3Bx8f%2BO479X4HbhqgFtiq1V9zYh/kFAgXF4iOhtdff7BTIB57DL78EgYOfLBTIB5/HGJjoU%2BfBz8F4skn4dtv4fnnH%2BwUCFdX%2BPVXcHNTnwJRaqqW0SwtdSexaZmDVer8FKPZ2el%2BzydOqE%2BBUN0BaD2BYdo0tX4rYp63yhSItm1h/XoYMUJ9CsR//6sWp9Hf8ZHbo0fFt1na22%2B/zeXLl1myZAmFhYVMmDCB1q1bM2fOnDLrHj9%2BnOHDh7NkyRJ69uzJ%2BfPnGTNmDK%2B99hojR44kMzMTd3d3vvvuO5ooXn1IboX8kPrwww9ZsWIFBQUFZGZmsmLFCrp3715pyS/oqs8TJkxg69atFBUVcfXqVdatW4e3t3eljUkIIYSobv6OKRCpqamcOHHC4F%2Bq6gnKd8jJySEmJobQ0FDs7e1p0KABU6ZMYceOHeSUUwxITk7mlVdewdvbG3Nzc5ydnXnuuef09204fvw49vb2yskvPCR3ghNlLVq0SH9TEXNzc3r06GEwD7gyWFhYsGzZMj766CMWLlyItbU1vr6%2BvPXWW5U6LiGEEEJoU95lSMePH09IiHGXIc3NzTW4vGtpOTk55Ofn07ZtW/1jzs7O5ObmkpCQwGOPPWawvq%2Bvr8EdenNzc9m/fz99%2B/YF4Pfff8fW1pbXXnuNM2fO0KRJE0JCQkwqyEkC/JBq06aNUdcYftDc3d3ZunVrZQ9DCCGEqLb%2BjpPWyrsMaUMTrmX922%2B/8frrr5f7XFhYGIDBZVRLriZ1vytFZWZmEhYWho2NDSNGjAB0V8568sknmTRpEo0bN%2Baf//wnISEhbNy4sdxL7JZHEmAhhBBCiGquvMuQmsLT05M/7nIuxsmTJ1m6dCk5OTn6S8mWTH2odY87gZ47d47Q0FAaNGhAdHS0ft3Sl9AFeOmll4iNjWXv3r1GJ8AyB1gIIYQQogqpapdBa9myJVZWVsSXuonO2bNnsbKyosVdboH9ww8/MHjwYHr06MHatWupW7eu/rm1a9fy008/Gayfl5dn0nlSkgALIYQQQlQhVS0BtrW1pXfv3ixcuJD09HTS09NZuHAhffr0MbjDbomjR48ybtw43n77baZNm2Zwx17Q3TF33rx5XLhwgYKCArZv386RI0fo37%2B/0WOSBFgIIYQQQvyt5syZQ4sWLejbty8vvPACTZs2NbgPg5%2BfHytXrgRg5cqVFBQU8N577xlc57dk6sPUqVPx8vJi2LBhuLu788UXX/CPf/yD5s2bGz0emQMshBBCCFGFVMU7t9WqVYv58%2Bfrb252p6%2B//lr//5JE%2BG5q1KjBjBkzmDFjhvJ4pAIshBBCCCGqFbkTnHjoqd6sy9wcataErCy1o%2BXaBRlqHVtYQJ06cOMGFBaqtWFvrxZnZgZa/qTT09XiLCx0Y752Te01q97xysoKGjXS3WUsP9/0eEvFL8Eq4C5hGTZOJsdUxFurnr2G94eW95fqneAq4o5sjRubHlMRd6A7dsz0GBsbaNMGzpxRv2vfnj1qcY88orsr2vr1cJfrud5TcLBav1p31qC2rbTeLhR0781K8M03Fd9m794V3%2BbDTKZACCGEEEJUIVVxCsTDRqZACCGEEEKfVYiWAAAgAElEQVSIakUqwEIIIYQQVYhUgLWTCrAQQgghhKhWpAIshBBCCFGFSAVYO0mAhRBCCCGqEEmAtZMpEEIIIYQQolqRCrAQQgghRBUiFWDtpAIshBBCCCGqFUmAHxIJCQmVPQQhhBBCVAFFRRX/r7qplglwr1692LFjR5nHd%2BzYQa9evYxqY/fu3fj5%2BRm1bmRkJP7%2B/nd9ftOmTbzzzjt3fd7V1ZVffvnFqL6EEEII8X%2BbJMDayRxgRS%2B99BIvvfRShbSVnp5%2Bz%2BePqN6HXgghhBBClFEtK8DGSkpKIigoCE9PT7y9vVm8eDF5eXlA2Wrxf//7X15%2B%2BWXc3Nx45ZVX%2BPjjjw2qvllZWcyaNYunn34aT09PFi9eDMDOnTtZtWoVv/zyC%2B7u7uWOw8XFhUOHDgG66vWqVat4%2BeWXcXV15eWXX%2BbgwYN3fQ33GldkZCQjR45k4MCBdOnShZ9//pmzZ88SGBjIM888Q4cOHXjxxRf5/vvvAZg6dSqTJ082aH/ChAnMmzfvvttLCCGEEBVDKsDaVdsK8Lx583j//fcNHsvPz6dBgwYAZGdnM2LECPz8/Fi6dCnp6emEhoZSVFRUJgm8ePEiQUFBzJw5k4EDB3L06FGCgoJ47LHH9OucPHmSgIAA5s%2Bfz6FDhxgxYgTPPPMM/fv35%2BLFixw%2BfJgNGzYYNfYvv/yS1atX4%2BjoyLx585g7dy7//Oc/y6xnzLh%2B%2Bukn1q1bR4cOHbC2tuall17Cx8eHqKgoiouLWbhwIXPnzsXb25shQ4YwatQoMjMzqVWrFjdu3GDfvn18/vnnJm2ve0lNTSUtLc3gMTu7hjg6OhrdRglzc8OlySws1OI0d1yJVF9zSZxqvJWVWpylpeHSVKrj1dqvYtdV%2Ba2lvK0qYFvj6mp6TLt2hksVNjamx1hbGy5VPPKIWlz9%2BoZLU6m%2BMSvija3y/tC63yooUIurANUxYa1o1TYBnjNnDgMGDDB4bMeOHURFRQGwf/9%2B8vLymDRpEmZmZjg5OREWFkZoaGiZhC4mJobHHnuMoUOHAuDu7s6QIUP4/fff9eu0adOGfv36AdC1a1ccHBxISkrCVWHHPGjQIJo3bw5A3759%2Beqrr8pdz5hxNWvWjG7duul/XrVqFY888gjFxcUkJydTp04dUlJS9PFOTk588803DB48mNjYWFq1asXjjz/Onj17jN5e97Jlyxb976DEuHHjCQ0NMbqNO9naqkbWUe4TgFq1tMWrMjNTj7W319Z37dra4lU5OFROv/XqKYdqeXdpe2tpeH%2BA%2BvurYUNt/WrY1vz6q3rs5s3qsVo8%2Bqh6bJs22vquoOl9JlPfWUPNmuqxdRT/Gu8o1oiqpdomwPeTnJxMeno6Hh4e%2BseKi4vJz8/n6tWrButevnyZJk2aGDzWrFkzg0TT/o7EokaNGhQWFiqNzaHUh72lpSXFxcXlrmfMuO6srMbFxREcHExaWhrOzs7Ur1/foP3Bgweza9cuBg8ezM6dOxk8eDBw/%2B1VUlm/n6FDh5Y5EdHOriFZWUaFGzA31%2B1Pc3LUjpZrFt4wPaik41q1IDNT/TBdNZE0M4O7vB%2BMcv26WpyFhW7MN2%2BCyvs6N1etX0tLXfL7559q1RgtFeB69SAjQ7kKdMPa9ISwIt5adWpreH9oeX/9%2BadaXAVsa3x9TY9p106X/A4bBnFxav1u2WJ6jLW1LvlNSoJbt9T6/c9/1OLq19clv7t3w33OTSnX7c8Dk2ndWQOoTLezsNAlvzduqO23KpFUgLWTBPguGjVqxKOPPmowtSAzM5OrV69S/46vh5o0aaKfJ1vi0qVLD2Sc92LMuMxKVXNSUlIICwsjKipKn4Tu3buXb7/9Vr9O//79WbJkCf/973/5448/6NOnD2Da9roXR0fHMkn5zZva/tiV5zdp3SEWFVW5narm8RYWqrWRn6%2Bt34ICtTa0HCyU9KuYlBVq2PtWxbeW5q%2BLNWxrtJxIHBenHq96YAe65Fc1/va3dsrS09Xa0JqVaZmMquX9VVhYqdMZROWoijPJHghvb2%2BysrJYs2YNeXl53Lhxg2nTpjFx4kSDpBGgX79%2BnDp1iq%2B%2B%2BorCwkJ%2B%2B%2B03tm7danRf1tbWZGZm3rWSq8rUcWVlZVFYWIjt7a%2Bh4uPjWbZsGYD%2BZLb69evj7e3NrFmzeP7556lbty5g2vYSQgghhDo5CU47SYDvolatWqxfv55Dhw7h5eXFs88%2Bi7m5OStWrCizbqNGjYiIiGD16tW4u7vz4Ycf8vTTT2Nl5Ik93t7eXLt2jc6dO3PjhuLX7uUwdVytWrVi6tSpvPXWW3Tu3JmwsDAGDhyIlZUVp0%2Bf1q83ZMgQkpOTGTRokP4xU7aXEEIIIdRJAqydWXFFlx2rocuXL5ORkUH79u31j33wwQekpaWxaNEiGZdGN2%2BqxZmb686LyMpS%2B%2BOuXZCh1nFFzCtTPRlN6xxglXl/oHvN9vZw7Zraa87OVuvXygoaNYIrV9SmQGi5MkHDhrqTYBS/Os2wcTI5piLeWvXsK2kO8JUranEVsK1p3Nj0GFdX3clzbm7qUyCOHTM9xsZGdxLbmTPqUyD27FGLe%2BQRGDEC1q9XmwIRHKzWr9adNahtq4qYX6715E5Fn35a8W2%2B8UbFt/kwkwpwBcjIyGDYsGEcP34c0J1Itnv3bry9vWVcQgghhKhQUgHWTk6CqwDt27dn5syZTJo0ibS0NBwcHHjzzTf1J4jJuIQQQgghHh6SAFeQwYMH6y8J9jB5WMclhBBCCDXVsWJb0SQBFkIIIYSoQiQB1k7mAAshhBBCiGpFKsBCCCGEEFWIVIC1kwqwEEIIIYSoVqQCLIQQQghRhUgFWDtJgIUQQgghqhBJgLWTBFg89K5dU4uzstLdXOjmTbWbhNWuX0OtY3PzvwZgYaHWhurezcJC254xKUktztZWdye4K1cgJ8f0eCNvG16GjY1umZmpdieohAS1fuvU0d0B6tQp3W3ZFNRr08b0IGtrqNOCOukJcOuWUr%2BkqoVhbQ0tWkBiolrfqrd5t7PTbevUVPU7BqrekQ1gyxb1O7J16GB6TMkd6IYOVb8D3ZtvqsWV7CgvXoQLF0yPV72TZI0aup31jRuQl6fWhsr7y8ZGdye4P/9U/x1X0p3ghHaSAAshhBBCVCFSAdZOToITQgghhBDVilSAhRBCCCGqEKkAaycJsBBCCCFEFSIJsHYyBUIIIYQQQlQrUgEWQgghhKhCpAKsnVSAhRBCCCFEtSIVYCGEEEKIKkQqwNpJAiyEEEIIUYVIAqydTIEQf6vU1FSyVe/eJIQQQgjxN5AEuBL06tWLJ598EldXV4N/I0eOrOyhlXHo0CFcXFyMXv/DDz/E1dUVT09P/vzzT3x9fUlXvT2mEEIIIcooKqr4f3%2B37Oxs3n77bTw9PencuTNTp04lKyvrruvPmTOHJ554wiBP2rJli/751atX4%2BXlRadOnfD39%2BfcuXMmjUemQFSSefPmMWDAgMoeRoWLjo5m8eLFPP/881y8eFGqv0IIIYRg/vz5XL58mb1791JYWMiECRNYuHAhc%2BbMKXf933//nfnz59O/f/8yz%2B3cuZMNGzawdu1aHn30URYvXkxoaCgxMTGYmZkZNR6pAD%2BE8vLyWLp0KT4%2BPnTp0oUxY8aQmJiof97FxYUtW7bg6%2BtLx44dCQoK4vjx47zyyiu4uroycOBA/fqRkZH4%2B/sbtN%2BrVy927NgBgL%2B/P9OnT8fb25tnnnmGzMzMe44tKSmJoKAgPD098fb2ZvHixeTl5ZGRkYGrqysFBQVMmTKFt956iz59%2BgDQp08f9uzZU5GbSAghhKi2qloFOCcnh5iYGEJDQ7G3t%2Bf/s3fnYVGV7QPHv%2BzgDgYiipmIobnhAmqGgluKZCJKuZtm7huWa66plZoLmFamvmoarilJvlaalntqLvnDNVPZBUvZt/n9MTIvI2jMGQ5I3J/r8ho5Z57nPgyz3POc%2B3lO1apVmTx5Mrt27SI1NTXf/TMyMrh69SoNGzYssL9t27bRt29fXF1dsbKyIigoiKioKE6ePFnoY5IR4GfQsmXLOHHiBBs2bMDBwYEvvviCt956i/DwcKysrAAICwsjNDSUjIwMfH19GTVqFOvXr6d69eoMHTqUNWvWsGjRokLFO3bsGNu3b8fGxoYKFSo88X4pKSkMHjwYX19fVqxYQWJiIuPGjSMnJ4egoCDOnTvHiy%2B%2ByBdffIGnpyd3796lQ4cOfPvtt9SsWbNQxxIXF0d8fLzetpwce%2BztHQrVPi9zc/1bg5kq/H6Y%2B%2B3TxER5HyXFxkZZu0fPS92toZT%2BkYyNW6mSsna5r5OnvF7%2BkZJjtrTUvy1OxsYuV05ZO2tr/VsllByzsc8tAHd3w9u4uenfKuHsrKydo6P%2BraGUPjeMfrNG2fPD2L9xWpqydkVAjYS1oM9fe3t7HBwK9/mblpZGbGxsgftSU1PJzMykXr16um0uLi6kpaVx69Yt6tevr3f/iIgIsrKyWLlyJWfOnKFixYr06tWLYcOGYWpqyvXr13n77bd197ewsKB27dpERETQqlWrQh2vJMAlZO7cuSxcuFBv25EjR7CxseHrr79m5cqVOD96Exs9ejTbtm3jp59%2BokuXLgD079%2BfKlWqAODq6kqDBg1wcXEBoFWrVpw5c6bQx%2BLl5UW1atX%2B8X4//fQTGRkZTJo0CRMTE6pXr8748eMZN24cQUFBhY73NKGhoYSEhOhtGz16DOPGjVXc53PPKW2pMBnMZcwHtjHMzJS3NeZDF%2BCFF4xrr5TSD/y6dY2L26yZce2VcnIqmbglGfvR%2B1uxq1VLeduzZ5W33bJFeVtjDRtWMnHt7UsmrtK/8cWLRXscJaygz98xY8YwdmzhPn/Pnz/PwIEDC9w3fvx4AMrl%2BSJs82jApaA64IcPH%2BLh4cGAAQP45JNP%2BL//%2Bz9Gjx6Nqakpw4YNIzk5Wdc%2Bl7W1tUFll5IAl5DZs2cXWAOckJBASkoK48ePxzTP6GFmZiaRkZG6n3OTXwAzMzMqV66s%2B9nU1BSNRlPoYynst7vIyEgSExNp2bKlbptGoyEzM5OEhASqVq1a6JhPEhgYiI%2BPj962nBx7YmIM78vcXJv83rsHWVmGt3esnP%2B0TKGYmGiT37Q0MODvoEfpSIqZGWRnK2sLcO2asnZWVtrk948/ID3d8PbGjAA7O8OdO8riRkUpi1uhgjb5PXsW/qFs6ImUfOhaWmoT0KgoyMhQFlcpY2M/ZbLLU1lba5PfGzeUj7gpHQGuVQtu31b23AIIDDS8jZubNvnt2xciIpTFVTq/xNFRm/yuXYuiN92hQ5XFNTfXJr/x8crerEHZ67Ao/sYlRI0R4II%2Bf%2B0N%2BFLi6enJlStXCtx3%2BfJlVqxYQWpqKuXLlwfQlT4UdOb55Zdf5uWXX9b93LhxYwYNGkR4eDjDhg3DxsaGtMfeD9LS0nR9F4YkwM8YW1tbrKysWLduHU2bNtVtv3nzpt4obaGLvE1NyczM1P2ck5PDX3/9pXefwvbl6OhIrVq12L9/v25bUlISCQkJ2NnZFaqPf%2BLg4JAvIb9zB/L8CgbLylLYXuk7TO4XF42m9C3WWEAtlkHS05X1YWFhfFwlydGDB8bFTUpS3ocxH7gZGSX3ga00trETYtPSlPdhzOtQ6XML4Nw55XEjIpS3zzNIoUhMjPaN11DGfinLylLehzHlCMb8jf9FCvr8LSovvPACFhYWXL9%2BnSZNmgBw48YNXenC43744Qfu3bvHG2%2B8oduWkZGB9aMzq66urly7dg1vb29AO0h469YtvRKLf1LKChT//UxNTQkICGDp0qXExMSQk5PD7t276d69u95EuMJycXHhypUrXLt2jaysLNauXat4ZQZvb2%2BSk5NZu3YtGRkZPHjwgClTpjBx4sQCk%2BjceuV/mlgnhBBCiMIrbZPgbGxs6Nq1K0uWLCExMZHExESWLFlC9%2B7ddUltXhqNhkWLFnH8%2BHE0Gg3nzp1j48aNBD46q9KrVy82b95MREQE6enpLF26lOeee44WLVoU%2BpgkAX4GTZkyhSZNmtC3b19atGjBhg0bWLlyJQ0aNDC4r44dO%2BLn58fgwYN55ZVXuH//Ps2bN1d0XBUqVGDDhg2cPHkSLy8vOnbsiKmpKatXry7w/s899xydOnUiMDCQrVu3KoophBBCCH2lLQEGbeln7dq18fPz49VXX6VmzZrMmjVLt9/X15c1a9YA0KlTJ6ZNm8acOXNwd3fn3XffZezYsfTo0QOAgIAABg8ezOjRo2nVqhWXL1/ms88%2Bw8KAs4kmGkOKRYUoAUrOxIH2rLqjo/ZsnpISCGc7hTWLpqba1RRSU5W/qyidQGdsDfCFC8ra2dhoaxcjIoq3BMLaWjuR7fp1Zacwb91SFrdSJfDygiNHlJdAuLoa3sbKCmrX1h53cZdAGBtb6eNUrhy89BL8/rvyEgglrydra%2B3f6No15afHGzc2vI27u7a2vFkz5SUQw4cra%2BfsDDNnwgcfKHvjnT5dWVxLS6heHaKjlZdAKHl%2BFcXfuFEjZe2MNGNG0fe5YEHR9/kskxpgIYQQQohSpLRNL3kWSQmEEEIIIYQoU2QEWAghhBCiFJERYONJAiyEEEIIUYpIAmw8KYEQQgghhBBliowACyGEEEKUIjICbDxJgIUQQgghShFJgI0nJRBCCCGEEKJMkRFgIYQQQohSREaAjScJsHjmXb2qrF3Fitorwd2%2BDQ8fGt7evEF5RXHNzcHeBuKTbMjKUtQFGfcMb6O7mFKcmeKLKT1frpyyhlZW2ltrazAxMbx9UpKyuKaPTmJlZCi7OlmVKsriln/03KhY8X/HYKjEROVxHzyAZIVXKoyJUdaucmXtleBu3oS//za8fYUKyuLmXtnw4UNlL2TQXlnNUNWqaa8SdvQoxMYqi6vkimzOztpbf39o2VJZ3M8/V9bO3V17Jbhdu5RdhW7MGGVxc6/Ul5Sk/IpsSq8SCNqYxrQXpZIkwEIIIYQQpYiMABtPEmAhhBBCiFJEEmDjySQ4IYQQQghRpsgIsBBCCCFEKSIjwMaTEWAhhBBCCFGmyAiwEEIIIUQpIiPAxpMEWAghhBCiFJEE2HhSAiGEEEIIIcoUGQEWQgghhChFZATYeDICLIQQQgghyhQZARaqyc7OJioqCufcS3sKIYQQwmgyAmw8SYCLmI%2BPD/Hx8Zib6z%2B07u7urFu3roSOqvBmzZoFwLx58/7xvgMGDMDDw4OxY8cWuH/ixIm4uro%2Bcb8QQgghDCcJsPEkAVbB3Llz8ff3L%2BnDUKQwiW9h3b9/v8j6EkIIIYQoKlIDXMwyMjJYsWIFHTp0wMPDg7fffps///xTt//FF18kNDSULl260KRJE0aMGMGlS5d44403cHd3p1evXrr7BwcHM2DAAL3%2BfXx82LVrF6AdoZ06dSre3t60b9%2BepKQkvfuePHmSdu3aERQURIsWLfj888%2BZOnUqU6dO1d1n48aNeHt74%2BnpycSJExk7dizBwcG6/X/%2B%2BSdvvfUWLVu2pEOHDuzfvx%2BAGTNm8Ouvv/LZZ58xYsSIon0QhRBCiDIsJ6fo/5U1MgJczJYtW8aJEyfYsGEDDg4OfPHFF7z11luEh4djZWUFQFhYGKGhoWRkZODr68uoUaNYv3491atXZ%2BjQoaxZs4ZFixYVKt6xY8fYvn07NjY2VKhQId/%2BmJgY6tSpw4cffkh6ejoffPCBbt%2B%2BffsICQlhzZo1NGrUiG3btjFv3jzq1aunu8/Ro0dZu3Yt9evXZ/Xq1UybNo0OHTqwYMECbt%2B%2B/dQSiYLExcURHx%2Bvty0tzR57e4dC95GrXDn9W0OZK3x15LZT2l6pIomrsVLWztJS/9ZQ2dnK2llb698aylThGICNjf6tEmZmJRO3cmVl7XLfPwp4HykUpS9EY1/IANWqGd7Gzk7/VonMTMPbODrq3yrh7q6snZub/q2hlL4OH3326W6Li7HvHykpRXcsothJAqyCuXPnsnDhQr1tR44cwcbGhq%2B//pqVK1fqJoaNHj2abdu28dNPP9GlSxcA%2BvfvT5UqVQBwdXWlQYMGuLi4ANCqVSvOnDlT6GPx8vKi2j%2B8%2BQcEBGBhYYGFhYXe9h07dhAYGEizZs0A6NevH7t379a7T7du3XjppZd0/1%2B5ciUJCQk4KnzzDg0NJSQkRG/b6NFj8PVVXkfcsKHipkaxtS2ZuPb2xrSubVxwJyfj2itVp07JxFWaKBjL1bVk4gJ4eJRM3MaNlbdt1Up529deU97WGMOGKW87c6ZxsbdsMa69UrVqlUxcpa%2BnkyeL9jgMUBZHbIuaJMAqmD17doE1wAkJCaSkpDB%2B/HhM84w8ZWZmEhkZqfs5N/kFMDMzo3KeERtTU1M0Gk2hj8XB4Z9HTp90n%2BjoaF1SnuvxFR3yHmtuAp2VlVXo43tcYGAgPj4%2Bettu3bLn1CnD%2BypXTpv8Xrqk7Iv6Cy8Y3ga0I7C2tnD/Pih9KJS0MzfXJr/x8crjVk%2B/payhpaU2%2BY2KgowMw9srHUmxttYmvzdvQlqa4e3T05XFtbHRJr8REZCaqqwPpSPArq5w7ZryuPfuKWtXoYI2%2BT11Ch4rpyoUY0aAGzeGCxeUP08iIgxvY2enTX737oXERGVx7941vI2jozb5XbsWYmKUxX1UBmcwNzdt8tu3r7LHLDRUWVwrK23ye/u28tekkte/tfX/Xk9K2pcgSYCNJwlwMbK1tcXKyop169bRtGlT3fabN2/qjdKamJgUqj9TU1My85xiy8nJ4a%2B//tK7T2H6etJ9atSoQVRUlN62qKgo6qg42ubg4JAvIY%2BJgYcPlfeZkqKsvRF5vK690j6U5JB54ypur/TDJ1dGhrI%2BlCZzudLSlPVhbNzUVEhOVtZWSQJcFHH//lt5XNAmv0r6UFrmkkvpCxkgNlZ53MRE5e3v3FEeNyZGeftz55THBW3yq6QPY5PI9HTlfRhTjpCWJuUMZZBMgitGpqamBAQEsHTpUmJiYsjJyWH37t10795dbyJcYbm4uHDlyhWuXbtGVlYWa9euJaUIX8R9%2BvRh27ZtXLhwgaysLHbu3Mlvv/1W6PaWlpY8NCZzFUIIIUQ%2BMgnOeDICXMymTJlCcHAwffv25a%2B//sLZ2ZmVK1fSoEEDg/vq2LEjx44dY/DgweTk5PD666/TvHnzIjvWLl26cPv2bUaNGkVGRgZeXl40bNgwX63wk7z%2B%2BuvMmTOHS5cusaWkasqEEEIIIR4jCXARO3jw4FP3W1lZMXnyZCZPnlzg/itXruj9vGnTJr2f866oYG5uzrx58564du/jbR/n6emZL96HH36o%2B39ERATdunXj7bff1m3z9/fH7tGs6Mf7r1mzpl5/fn5%2B%2BPn5PfUYhBBCCGGYsjhiW9SkBEI80YkTJxgxYgTx8fFoNBrCw8O5fv06rVu3LulDE0IIIcosKYEwnowAiyfq378/kZGR9OzZk%2BTkZOrUqcPq1avzrQQhhBBCCFGaSAIsnsjc3JwZM2YwY8aMkj4UIYQQQjxSFkdsi5qUQAghhBBCiDJFRoCFEEIIIUoRGQE2niTAQgghhBCliCTAxpMSCCGEEEIIUabICLAQQgghRCkiI8DGM9FoNJqSPgghnkrBZaIBsLSE6tUhOhoyMgxu/u3F5xWFrVQJvLzgyBF48EBRFyUWt/tHryhrWK8efPklDB0KV68a3v76dWVxGzWCAwegc2e4eNHw9uYKxwAaNoTvvoOuXeHSJWV9uLkZ3qZuXVi9GkaOVP6Y3b6trF2DBrB7N/TsCZcvG96%2BfHllcd3cYMsW6NsXIiKU9XH4sOFtTE21x5ycrDzbSEw0vI2R71uA8jcAa2twdYVr1yAtzfD2jRsri%2BvuDmfPQrNmcO6csj5q1jS8TVG8ju/cUdbOSL6%2BRd/nvn1F3%2BezTEaAhRBCCCFKERkBNp4kwEIIIYQQpYgkwMaTSXBCCCGEEKJMkRFgIYQQQohSpDSOAKekpDB//nwOHjxIVlYWHTp0YPbs2ZQvYH7ArFmzCAsL09uWlpZGmzZt%2BPLLL8nJyaF58%2BZoNBpMTEx09zl69CjlypUr1PHICLAQQgghhFDV/PnziY6O5r///S8HDhwgOjqaJUuWFHjfefPmce7cOd2/4OBgKlWqxNSpUwG4fv06mZmZnDp1Su9%2BhU1%2BQRJgIYQQQohSJSen6P%2BpKTU1lbCwMMaNG0eVKlWoWrUqkydPZteuXaSmpj61bWJiIpMnT2bGjBm4uroCcPHiRV588UUsLS0VH5OUQAghhBBClCJqJKxxcXHEx8frbbO3t8fBwaFQ7dPS0oiNjS1wX2pqKpmZmdSrV0%2B3zcXFhbS0NG7dukX9%2BvWf2O%2BSJUto2LAhr732mm7bxYsXSU9Pp1evXkRGRuLi4kJQUBDNmjUr1LGCJMBCCCGEEGVeaGgoISEhetvGjBnD2LFjC9X%2B/PnzDBw4sMB948ePB9ArUbCxsQEgOTn5iX3euXOHvXv3sn37dr3t1tbWNG7cmPHjx1O5cmW%2B%2Buorhg4dyt69e3F2di7U8UoCLIQQQghRiqgxAhwYGIiPj4/eNnt7%2B0K39/T05MqVKwXuu3z5MitWrCA1NVU36S239KFChQpP7HPnzp24u7vnGyHOrQXONXToUHbt2sXhw4fp379/oY5XEmAhhBBCiDLOwcGh0OUOhnrhhRewsLDg%2BvXrNGnSBIAbN25gYWFB7dq1n9juwIEDvPXWW/m2L1u2jC5dutCgQQPdtoyMDKysrAp9TDIJTgghhBCiFCltk%2BBsbGzo2rUrS5YsITExkcTERJYsWUL37t2xtrYusM39%2B/e5ceMGLVu2zLfv6tWrLFiwgPj4eDIyMggJCSEpKYlOnToV%2BpgkAS4iPj4%2B7Nq1K9/2Xbt25TulUNKCg4MZMGCA0f0MGzaMNWvWFMERCSGEEKKwSlsCDNltSQYAACAASURBVDB79mxq166Nn58fr776KjVr1mTWrFm6/b6%2Bvno5xd27dwGoVq1avr4WLVpErVq16NGjB56enpw6dYr169dTpUqVQh%2BPlEAIxdauXVvShyCEEEKIUqBChQrMnz%2Bf%2BfPnF7h/3759ej83atToiTXFVapUYdGiRUYdj4wAF5NZs2blq2OZN28e7733Hnfv3uXFF19k06ZNvPzyyzRv3px3332XpKQk3X337duHn58fzZs3x9/fn19%2B%2BUW3b8CAAUydOhVvb2/at2/PlStX/rG/XBqNhs8//xw/Pz9atGhBy5YtCQoKIi0tDYBr167Rr18/WrZsibe3N1OmTNH1M2DAAIKDgwFISkpi5syZdO7cmaZNm/LKK6/I6LAQQgihgtI4AvyskRHgIjR37lwWLlyoty0zM5OqVasSEBBAYGAgsbGxVKtWjYyMDPbt28eKFSt09z1w4ABhYWFkZ2czevRo5s6dy%2BLFizl8%2BDCzZ89m9erVNGvWjCNHjjB27Fi2bdumWxT62LFjbN%2B%2BHRsbGx48ePDU/vL67rvv2LhxI5s3b6Z27drcuHGDvn37EhYWRu/evZk7dy6tW7dm8%2BbN3L9/n0GDBrF9%2B3aGDBmi18%2BSJUu4e/cuO3bsoGLFihw4cIBx48bRtWtXnn/%2B%2BUI/hgWuQ5idjYMBM1F1zM31bw1UqZKiZuROaH3KxFZVFEncPGs0GqRWLf1bQz1aDsdgdevq3xrKzExZOxcX/VslnjLx44lyl/cp5DI/BVL6BKlTR//WUEr/xrmPk5LHK5epgrGe3DZK2uZSski/ke9bADyhpvIf5U4gMmAikR53d2Xt3Nz0b5Uo4DT5PzL2dXzpkrJ2RaAsJqxFTRLgIjR79mz8/f31tu3atYuQkBAaN26Mi4sL3377LUOHDuWnn36iQoUKeHp6EhkZCcC0adOws7MDYNy4cYwcOZIFCxawefNm3nzzTV0huLe3Nz4%2BPnz99de8//77AHh5eenqZHIT4Cf1l5eXlxfNmjXD0dGRxMRE7t%2B/T5UqVXSLWVtZWfHzzz/j4uJC69at2bNnD6YFfCCMHTsWMzMzKlSoQExMjG4mZlxcnEEJcIHrEI4ezdhx4wrdRz5KkmfAq7rykAAGrMddpIyK6/WlccFnzzauvVKffloycR97rhab6dNLJi7A0qUlE/exwYViozRxB3i03JMiCt%2B3ioTSL7JnzxoXd8sW49orpfR1bMwXUVHiJAEuRv7%2B/nzzzTe69ep69uyJiYmJbn/eRLF69epkZGTw119/ERkZyalTp9i6datuf3Z2Nq1atdL9XNDSJU/qLy%2BNRsOyZcs4dOgQdnZ21K9fn8zMTDQaDQDLly8nODiYZcuWMWnSJJo1a8acOXN0I8%2B5EhISWLBgAZcvX6ZmzZo0bNgQgBwDv6YWuA5hdjZERxvUD6AdQbG3h/h4yMoyuPmRa8oy4AoVtEno2bNQQNWJaooirtd/hiprWKuWNvmdOxdu3za8/Z07yuLWratNfkeNguvXDW9vzAhwSAiMGQM3bijrQ%2BkI8PTp2mRQ6WMWE6OsXZ062uQ3KAhu3jS8vTEjwAsXan/vW7eU9fHFF4a3MTXVHnNqqvLhtkeDEQYx8n0LUP4GYGWlfS3fvg3p6Ya3DwxUFtfNTZv89u0LERHK%2BlA6Amzs67iEyAiw8SQBLkY9evTgk08%2B4dy5cxw9elRv9iNAbGwsdR6dXrx79y42NjbY2tri6OjI66%2B/zvDhw3X3jYqK0ls6JG8i/U/95bVkyRKioqI4ePCgbjFqPz8/QJu8Xr58mbFjxzJ9%2BnSio6NZtGgRU6dOZefOnXr9jB8/Hh8fH7788kvMzc25f/8%2B27ZtM/gxKnAdwj//hIwMg/vSycpS1F7JZ1deSUnG91Hsca9eNS747dvK%2BlCSvD7e/uJFw9sZc5oZtB%2BaSk%2BDKk1uQJv8Kn3MlHxByevmTbh82fB2xoyGgjb5VZocGZMtGFMgWQLvWwA8msOhWHq6sj7OnTMubkSE8j5q1lQe15jXsSi1ZBJcMapatSrt2rVj3rx5tGjRAicnJ739S5cuJSkpidjYWFauXEmPHj2wsLCgT58%2BbNy4kQsXLgDaa2D7%2B/vz7bffPjXek/rLKykpCSsrK8zMzEhPT2fdunVcvXqVzMxMTE1N%2BeCDD1i%2BfDnp6enY2dlhZWWVL4kGePjwIdbW1piZmZGYmMgHH3wAaGughRBCCFF0ZBKc8SQBLmb%2B/v5cvnyZXr165dtXq1YtunfvzmuvvYa7uzvTH9X5vfrqq0yaNInp06fTrFkzxo8fz%2BDBg/9xLd8n9ZfXhAkTSEtLo02bNvj4%2BPDbb7/Ro0cPrj4axVu%2BfDk3btygbdu2tGnThocPHxa4hMmiRYsIDw%2BnWbNm%2BPv7U61aNRo0aKDrRwghhBBFQxJg40kJRBE5ePBggdv9/f31JsbVqFGDSpUqFXi1kn79%2BjFlypRC9ZPXpk2bCtz%2BpP7Gjh2r%2B7%2BzszObN28usD2Ai4sLGzZs%2BMe4r7zyCt99990T%2BxFCCCGEeFZIAlxMkpKSiIqKYvny5fj7%2Bxt0vWohhBBCiFxlccS2qEkJRDGJiYkhMDCQv//%2Bm1GjRpX04QghhBBClFkyAlxM6taty7knzG6tWbPmEy/3p0RR9yeEEEKIZ4eMABtPEmAhhBBCiFJEEmDjSQmEEEIIIYQoU2QEWAghhBCiFJERYOPJCLAQQgghhChTZARYCCGEEKIUkRFg40kCLIQQQghRikgCbDxJgMWzr0IFZe3MzLS3NjZgaam4udJ2ZmbK%2BzBX8MrM/RUtLUHxdVaee05ZuypV/nerpI/r15XFNVZ2trJ2uZ8%2BOTnK%2B7CwMLxN7hPD3FxZe1D0WgD%2BF8/CQlkf1tbK4uZ9YivtIy3N8Dbm5lC%2BPGRkQFaWsrgPHhjeJvd3TEpSdtwAKSnK2uVKS1PWR82ayuJVq/a/W6V93L1reBt7e%2B1tbKyy9qJUkwRYCCGEEKIUkRFg48kkOCGEEEIIUabICLAQQgghRCkiI8DGkwRYCCGEEKIUkQTYeFICIYQQQgghyhQZARZCCCGEKEVkBNh4MgIshBBCCCHKFBkBFkIIIYQoRWQE2HiSAAshhBBClCKSABtPSiBUkJaWRkxMTEkfhhBCCCGEKECZS4BnzZqFu7s77u7uNGrUCDc3N93P7u7u/Prrr0bHeOONNzh58iQAx44do0GDBga1/%2BGHH3jjjTdwd3enWbNm9OrViz179hh9XEps376dTp06PXH/5MmTmTFjRjEekRBCCFG25eQU/b%2BypsyVQMybN4958%2BYBsGvXLkJCQjh48GCRxkhMTFTc9tSpU7z33nusWLGCNm3aoNFo%2BPnnn5k0aRLm5ub4%2BvoW4ZEKIYQQQpQ9ZW4EuDBycnLYsGEDXbp0oUWLFvTv35/ff/9dtz8pKYk5c%2Bbg5eVF69atCQoKIiEhAYCBAwcSFxfHzJkzWbBgga7NF198QceOHWnatCnjx48nKSmpwNhnz57FycmJtm3bYmZmhrm5Od7e3gQFBZGenq673549e/D19cXd3Z1u3brx3//%2BV7cvNDSUrl270qxZM/z8/Ni3b59u35tvvsm0adNo3749Pj4%2BpKSk8MMPPxAYGEjr1q1p0qQJAwYM4Pbt27o22dnZLFy4kNatW9OxY0fWr1%2BPRqMp8Pj37t2Ln58fzZs3x9/fn2PHjhn46AshhBDiaWQE2HhlbgS4MDZt2sTGjRtZvXo1derUYffu3QwZMoT9%2B/djZ2fHlClTyMjI4JtvvsHS0pKFCxcybtw4vvrqKzZu3IiXlxdBQUH06NGDY8eOkZ2dTVxcHPv27SMxMZHevXvz9ddfM2zYsHyxfXx8%2BOyzz3jzzTfp0qULTZo04aWXXqJ///66%2Bxw7doyZM2eyatUq2rZty5EjRxgzZgyurq6cOXOGxYsXs2rVKlq0aMGJEycYM2YMNjY2%2BPj4AHD8%2BHFCQ0OxsbHh/v37TJgwgVWrVtGuXTsSExMZNWoUq1evZtGiRQBERkZiY2PD4cOHiYiIYNiwYdjb29O9e3e9Y//xxx%2BZP38%2Ba9asoWnTpvz000%2BMHj2aHTt24OLiUqjHPi4ujvj4eL1t9paWONjbG/Q3BMDMTP/WQJUqKWpG%2BfL6t0ooOeRy5fRvFalTR1m7GjX0bw2VnKysXd26%2BreGMjEpmbgAhXxN6KlZU/9WiQoVlLV74QX9W0MpfWI%2B/7z%2BrRLmCj7qjHz/AMDa2vA2Vlb6t8Up93iVHDdAw4bK2uW%2BFpS8JnIp%2BYxwc9O/NdS5c8raFYGymLAWNUmAC7BlyxZGjhzJiy%2B%2BCECfPn3Ytm0bYWFhvPrqq/zwww98//332NnZATB9%2BnQ8PDyIiIjA7QkvpHHjxmFlZUX16tVp3ry53ghrXvXq1WPPnj1s3ryZHTt28NFHH2FpaUmnTp2YPn06VatWZffu3XTt2hUvLy8A2rdvz5YtW3BwcGDnzp307dsXT09PAF5%2B%2BWX69OlDaGioLgFu164d1apVA8Da2prw8HBq1apFUlISMTEx2NnZERsbqzum5557jnHjxmFmZkbjxo0JCAhgz549%2BRLgr776in79%2BtG8eXMAOnTogJeXF6GhoUyfPr1Qj31oaCghISF628aMHs3YceMK1b5AFSsqavbyy8pDAjRtalx7pRo3NqJxq6XGBZ80ybj2Sn36acnEXbWqZOJOmVIycQE%2B%2Bqhk4s6dWzJxlX4TBrC1Vd62Vi3lbY3l6qqs3XffGRf3sff%2BYrNli7J2Sr9Ai2eCJMAFiIyMZOHChXyU540%2BKyuLqKgoIiMjAfD399drY25uzt27dwtMgM3MzKiYJwmzsLAgOzv7ifFr1aqlSxgfPHjAqVOn%2BOSTT5gwYQKbNm0iPj6epo9lV40fZT337t3D2dlZb1/NmjU5evSo7mcHBwe9Y9m7dy%2BhoaGYmZlRr149Hjx4gHWeEYDq1atjlmcUxMnJiV9%2B%2BSXfcUdGRnLmzBk2b96s25adnU3btm2f%2BLs%2BLjAwUJeo57K3tIS//ip0HzpmZtrk9%2BFDeMrj/SRHf69ieEy0I79Nm8Jvvykf2FQ6Aty4MVy4ACkpyuK22h6krGGNGtrk95NP4NFrxCAXLyqLW7euNvkdNQquXze8vTEjwKtWwejRyuIC1KtneJuaNbXJ70cfwd27yuLm%2BXJrkBde0MadMgX%2B%2BMPw9saMAM%2BdC7Nnw59/Kutj2TLD25iZaZPfBw8UvX8AcO%2Be4W2srLTJ7%2B3bkKfszSBpacraWVtrk99r15T18d57yuK6uGiT3zFj4MYNZX0oeV67uWmT3759ISJCWdwSIiPAxpMEuADVqlXj3Xff5dVXX9Vtu337Nra2tjx8%2BBCAAwcO6EaANRoNN27coFYRfGN/4403aNmyJUFB2kSkUqVKdOzYEY1Gw7Rp0wBtQhodHa3Xbu3atbRo0YIaNWrkG12%2Bffs29nlOD5nk%2BdAPCwvj66%2B/ZuvWrbrEefbs2fyZ54Pm8ZKEO3fuUKOAU93VqlWjT58%2BDB06VLctt3yisBwcHPQSdAASEpR/AIG2rYL2Dx4oDwna5FdpH0rO2OZKSdHm/IrcvKk8MGiTXyV9KE2Ac12/rqwPY0dwlMYF406t372rPFG4c0d5XNAmv//3f4a3U3gmRufPP%2BHqVWVts7KUx83OVt5eaSIK2uRXaXul34BzpaUp6%2BPSJePi3rihvA%2BlXwhBm/yWYDmDKBkyCa4AgYGBfPrpp/zxaJTj8OHDdOvWTW%2BC2oIFC/jrr7/IzMxk1apV9O7dW5ccW1lZPXGS2z/x8/Njy5YthIWFkZiYSE5ODjdv3mTTpk107twZgJ49e7J//36OHTtGTk4Ohw8fZtWqVVSsWJHevXuzdetWTp48SXZ2NsePH2fHjh306tWrwHgPHz7E1NQUKysrXV9hYWFkZmbq7hMTE8Pnn39ORkYGZ86cYceOHbzxxhsFPm7/%2Bc9/uPToDezChQv4%2B/vznbGnxYQQQgihI5PgjCcjwAXIHcF85513iI%2BPx9HRkblz59KuXTsAli5dypIlS3jttddITk6mXr16fPnll1StWhWA3r17s3jxYi5evMhrr71mUOx%2B/fpRsWJFvvrqK%2BbMmUNWVhaOjo74%2BfkxfPhwADw8PFi0aBGLFi0iMjKSmjVrsnz5clxcXHBxcSE5OZm5c%2BcSHR2No6Mj06dPz1evmysgIIBz587RrVs3zMzMcHFxYeDAgYSGhuqS4AYNGnDz5k08PT2xt7dn%2BvTpusciL19fX1JSUpgyZQpRUVHY2toydOhQ%2BvXrZ9BjIIQQQognK4sJa1Ez0TxpPSshnhWPlpgzmJkZVKmirR9WUALx3amqisJWqqSdQHf0aPGWQFSsCK1awYkTyksgOn3aU1nDOnVg6VIIClJWAnHihLK4jRrBgQPQuXPxlkA0agT//S906aK8BELJLEkXFwgOhrFji78Eon592LYN%2BvQp3hKIevVgwwYYPFh5CYSSCwmZm2snsd2/r7wEQskVQY2twwXlJRDlymmf2xcvKusjIEBZ3IYNtRPounYt3hIId3c4exaaNVNeAlFCKZSxFUUFUVw6V0rJCLAQQgghRCkiI8DGkxpgIYQQQghRpsgIsBBCCCFEKSIjwMaTBFgIIYQQohSRBNh4UgIhhBBCCCHKFEmAhRBCCCFKkdK8DnBqaiqBgYHs2rXrqfc7f/48vXv3xt3dHR8fH7Zv3663f/fu3XTq1ImmTZvi7%2B/POQNX8pAEWAghhBCiFCmtCfC1a9fo168fv/3221Pv9/fffzN8%2BHBef/11Tp8%2BzYIFC1i0aBEXLlwA4OTJk8yfP58PP/yQ06dP89prrzFy5EhSU1MLfSySAAshhBBCCFUdP36cQYMG0bNnT5ycnJ563wMHDlClShX69euHubk5rVu3xs/Pj6%2B%2B%2BgqA7du34%2BvrS/PmzbGwsGDw4MHY2toSHh5e6OORSXBCCCGEEKWIGiO2cXFxxMfH622zt7fHwcGhUO3T0tKIjY0tcJ%2B9vT1ubm4cOnQIKysr1q9f/9S%2Brl27Rr169fS21a1blx07dgBw/fp1evXqlW9/REREoY4VJAEWpUFVZVdki4uLIzQ4mMDAwEK/gPPq2lVRWOLi4ggODlUcV6kiidtpt%2BLYocHBBE6ZUuy/c2hwMIGbN5dM3E2bSibu%2B%2B8Xa1y92CEhJfM7f/xxycQ15vVkb29cXFdXZXEVMvp3VniVQV3c//ynZP7G%2B/cX%2B%2BvJWGpcgC44OJSQkBC9bWPGjGHs2LGFan/%2B/HkGDhxY4L5Vq1bRsWPHQh9LcnIyNjY2etusra1JeXSFwn/aXxhSAiH%2BteLj4wkJCcn3jVbi/ntiS9x/f2yJ%2B%2B%2BPXdbiPqtyJ6bl/RcYGFjo9p6enly5cqXAf4YkvwA2NjakPXYp8LS0NMqXL1%2Bo/YUhI8BCCCGEEGWcg4PDMzMSXq9ePY4ePaq37fr167g%2BOivi6urKtWvX8u338vIqdAwZARZCCCGEEM%2BMTp06ce/ePTZs2EBmZiYnTpwgLCxMV/cbEBBAWFgYJ06cIDMzkw0bNpCQkECnTp0KHUMSYCGEEEIIUaJ8fX1Zs2YNALa2tqxbt479%2B/fj6enJzJkzmTlzJq1atQKgdevWzJ49mzlz5uDh4cG%2Bffv44osvqFKlSqHjmc2ZM2eOGr%2BIEM%2BC8uXL4%2BHhYVBdkMQtXbEl7r8/tsT998cua3HLukGDBlG/fn29bf369aNFixa6n6tVq0ZAQADvvPMOAwcOzHd/Nzc3%2Bvfvz4gRI%2BjTpw%2BOjo4GHYOJRqPGXEIhhBBCCCGeTVICIYQQQgghyhRJgIUQQgghRJkiCbAQQgghhChTJAEWQgghhBBliiTAQgghhBCiTJEEWAghhBBClCmSAAshhBBCiDJFEmAhhBBCCFGmSAIshBBCCCHKFEmAhRAG%2B/XXX8nJySmx%2BJcvX%2BbAgQNkZGSQkJBQYsehtpEjR5KUlFTSh1HmJCYmFlus7777rsDtoaGhqsceOXJkgdv79%2B%2BvemwhSpp5SR%2BAEKVdZmYm4eHhREZG5ksKx4wZo1rcO3fusGbNmgLjbty4UbW4AKNHj%2Bann37CxsZG1TiPS0hIYPTo0Vy6dAkLCwt27NhBQEAA69atw93dXdXYGRkZHD58mMjISAIDA/nzzz9xc3NTNea5c%2BewtLRUNcbTHD16lE2bNhEXF8dnn33GunXrCAoKwtxc3Y%2BObdu26eLu3r2bDz/8kEWLFlG%2BfHnVYmZlZREcHMzmzZvJzs4mLCyMCRMmsHr1ahwcHIo0VmpqKvfv3wdg%2BvTpNG3aFI1Go9v/8OFDPvzwQwIDA4s0LsDdu3f55ptvAPjll18ICQnR25%2BUlMSVK1eKPO6z4o8//iA0NJTo6Gjmzp3Ld999x5tvvlnShyVKgCTA4l8nPT2dv//%2BmypVqhRL8hAUFMTJkydxdXXFxMREtz3v/9UwadIkLCwsaNWqFaamxXsyx9nZmYsXL%2BLh4VGscRcuXEi9evVYv349Xl5euLi4MHz4cD7%2B%2BGO2bt2qWtzbt2/z1ltvkZmZyYMHD2jXrh29evUiJCQEb29v1eJ2796dcePG4efnh729vd5zqmXLlqrFBQgLC2PRokX07t2b06dPA3Dw4EFMTEx47733VIu7YcMGtm7dytChQ/n4448pX748cXFxLFq0iA8%2B%2BEC1uMHBwZw4cYIVK1YwceJEqlatiqOjIwsWLGDFihVFGispKQlfX1/S0tLQaDT4%2BPjo9mk0GkxMTOjYsWORxszl5OTEtWvXSExMJDs7m5MnT%2Brtt7KyYvbs2arELmnHjx9n9OjReHl58fPPP5OSksLy5ctJSUlh6NChJX14orhphPiX%2BPXXXzVvvPGGpn79%2Bho3NzfNSy%2B9pOnfv7/m/PnzqsZ1d3fX3LlzR9UYBWnatKkmNTW12ONqNBrNW2%2B9pWnQoIGmc%2BfOmv79%2B2sGDBig%2B6emNm3aaFJSUjQajUbTsmVLjUaj0WRkZGhatGihatzhw4drVq1apcnJydHF2rVrl%2Bb1119XNe6LL75Y4D83NzdV42o0Gk337t01586d02g0Gt3v/Mcff2heeeUVVeN27txZc/36dY1G87%2B/cWxsrKZNmzaqxvX29tbExMToxf377781Hh4eqsS7d%2B%2Be5s6dO5qmTZtq7t69q/cvPj5elZiPmzFjRrHEeZLbt29rTp8%2BrTl16pTm1KlTmqNHj2rWr1%2BvWrxevXppDh48qNFo/vecPn/%2BvKZDhw6qxRTPLhkBFv8KZ86cYciQIXTu3Jn%2B/ftja2tLQkICBw8eZNCgQWzZsoX69eurEtve3p4qVaqo0vfTuLm5ERMTQ%2B3atYs9tru7u%2BolBwWxsLAgLS0NGxsb3Snj5ORkVU%2BNA/z2228EBwdjYmKiG4Xt0aMHCxYsUDVuRESEqv0/TUxMDE2aNAH%2Bdzbj%2BeefJyUlRdW49%2B/f54UXXgDQ/Y2rVq1KVlaWqnFTUlKws7PTi2ttba3a2RVfX19OnDhBp06dqFGjhiox/skHH3xAdnY29%2B7dIzs7W2%2Bfk5OTqrE/%2B%2Bwzli1bpntuaR6NfNevX5/BgwerEvPWrVu0b98e%2BN9zunHjxrpyFFG2SAIs/hWCg4MZOXJkvkkdfn5%2BhISEsHr1alauXKlK7ClTpjB%2B/Hj69u1LpUqV9PapeZp65syZDB48mM6dO%2BeLq2btcXH0/yQ%2BPj68%2B%2B67zJw5ExMTExISEvjggw9o166dqnErVqzIvXv39JKC%2BPh4KleurGpcKJnaY4DatWvz448/6p2KP3bsGM8//7yqcd3c3AgNDeXNN9/UJSnh4eG4urqqGrdp06aEhIQwceJEXdxNmzbRqFEjVeJlZGTwww8/cODAAfr06aNXA5xL7TKXAwcOMHXqVFJTU3XbchPR//u//1M19pYtW1i5ciWWlpYcPHiQSZMmMX/%2BfKpXr65azOrVq3P%2B/HmaNm2q2/b777%2BrGlM8u0w0Bb3qhChlPDw8OHjwIBUqVMi378GDB3Tv3p0jR46oEnvZsmV89tln%2Bbar/SEyYsQIzp49i6urq94olYmJieqT4KBkJiolJyczbdo0Dhw4AGh/13bt2rF48WIqVqyoWtwVK1Zw%2BPBhgoKCGD9%2BPOvWrWPx4sW4u7szadIk1eI%2BXnu8a9cuunfvrnrtMWiT3VGjRtGhQwd%2B%2BOEHevbsybfffsvSpUtV/cLx%2B%2B%2B/M3jwYFxcXLh06RKtW7fmt99%2BY%2B3atboRaTXcvn2bwYMHk5WVRUJCAs8//zzJycmsX7%2BeOnXqFHm8jz76iE2bNpGdnV1g8lscSWjHjh3x9/enW7duWFhY6O1Te1Ta3d2dc%2BfOERMTw6hRo9i1axeJiYkEBARw8OBBVWLu3buXBQsW0K9fP9avX8%2B4ceP4z3/%2Bw9ixY%2BnVq5cqMcWzSxJg8a%2BQ%2B2b6JM2bN%2BfMmTOqxG7ZsiVLly6lbdu2xToZzd3dne%2B//57nnnuu2GLmenyi0o8//sjw4cNxdXVVdaJSrsTERO7evYujo2ORz9AvSGZmJp988glff/01qampWFlZERAQwJQpU1SdaPnOO%2B/QpEkTRo4ciYeHB6dPn2b37t1s3LiR3bt3qxY3V0REBKGhoURGRuLo6EhAQACNGzdWPW5sbCx79%2B4lKioKR0dH/Pz8VD8lD9rVGQ4dOqSL2759%2BwK/VBelf3rvUpOHhwenTp0qkdhdunRh586dlC9fHk9PT06ePImJiYmq79UAP/74I1u2bNG9fwQGBtKtWzfV4olnlyTA4l%2BhWbNmnD17VvF%2BY7Rt25bDhw9jZmamSv9P0qVLF3bs2KHqyOfTYn/66ae4uLjoPkTj4uLo2bMnR48eVS1u7vJNj7OwsMDOzo6mTZuqvjRbYmIitra2qq/yAeDp6cnPP/%2BMpaWl7nHOycnBw8ODX3/9VfX4Zcn8%2BfPpzESzTwAAIABJREFU3bt3sZSX5PXXX3/p5hAkJibq6pCLw9ChQ3n33XeL/XcGbQlXVFQUy5cvZ9y4cTRq1AgrKyvCw8MJDw8v9uMRZY/UAIt/BY1GQ3R0dIGnEnP3q2XIkCEsWbKEESNGFEtNaK6hQ4cyatQoBg4cSOXKlYt1iaySmqgUGhrKb7/9RtWqValRowbR0dHEx8fj6OhIamoqJiYmrFu3rsgnPD6%2BVmouS0tLbG1tadOmjSqnjEuy9tjHx6fAJD/3y4a3tzdDhw4t8rMebm5uBcY1NzfXxZ06dSrW1tZFGjchIYHAwEBcXFzo3bs33bt3L5YvlxUqVGDZsmW69Yf37t3LxIkTVVl/OFfu89nOzo6hQ4fStWvXfBN51a7znzp1KkuXLiUrK4vp06czYcIEHj58yKJFi4o81vvvv/%2BP95k/f36RxxXPNkmAxb9CamoqPj4%2BT0x01Ryt%2B%2Bqrr4iKimLDhg359qlZwzdr1iwA3RqtuYqjdrCkJiq9%2BOKLtGzZkgkTJugSr5CQEP7%2B%2B29mzJjBunXrWLRoUZHXQF%2B9epUDBw7QqFEjnJ2diYqK4rfffqNRo0ZkZ2ezYMECVq9eTevWrYs0rp%2BfH2PGjCEoKIicnBwuXLjA4sWL8fX1LdI4BenTpw/btm1j2LBhODs7ExkZybp162jTpg116tRhy5YtpKWlMXbs2CKNO3XqVPbs2cOECRN0cYODg/Hw8KB58%2BasW7eOJUuWMHPmzCKNu3z5ch4%2BfEhYWBi7d%2B/mo48%2BokuXLgQEBKj6hfLx9Yefe%2B451dYfzpV37d86derku/BFcZzdqFChgm69YTs7O1VHfdPS0lTrW5ReUgIh/hUiIyP/8T5qTep4Wg1dcV8ooriU1ESltm3bcujQIb0JO5mZmXh7e/PLL7%2BQlZVFq1atirw8YNKkSbRo0YK%2Bffvqtu3cuZOTJ0/y8ccfEx4ezvr169m%2BfXuRxi2p2mOAnj178vHHH%2Bt9qbl58yaTJ09m165d3L17lwEDBnDo0KEijdutWze%2B/PJLvZn5sbGxDBkyhPDwcBISEujRowe//PJLkcZ93PHjx5kxYwbR0dGqfqH08fFh69atVKtWTVfm8uDBAzp16pTvIhX/Bp9//jnDhw9/4lkVKLlVZkTZIiPA4l%2BhMBOC1HpTLakkNyoq6on71J4w9NJLL/Htt98SFhZG/fr1cXR0ZO7cucUyUenOnTt6s/IjIyN1pRdpaWn5ZrMXhWPHjrF48WK9ba%2B//joff/wxAF27di3UaVZDWVhYMGXKFKZMmVKstccAf/75Z741pp2dnfnjjz8AqFmzJg8ePCjyuLGxsfnqYCtXrkx0dDSgHS1Ua0QvOTmZ/fv3880333DhwgXat2%2Bv%2Bqnx4l5/OK%2BSKO05ffo0w4cPf2Jyr/bzOzw8nD179hAbG0uNGjV48803adu2raoxxbNJEmDxr/BPIyVqvqk%2BqVYStDOO1Y6b%2B6GZ9xjULoEAqFatGj169NB9kNja2qoeMyAggOHDh/POO%2B/g5OREVFQUX375Jf7%2B/iQkJPDee%2B%2BpskRXuXLluHTpkt7o9uXLl3WjsAkJCapNvjtz5gx79uwhLi6OGjVqFNtELTc3Nz777DO9L47r1q2jbt26ABw5ckSVsyru7u7Mnz%2Bf999/HysrK9LT0/noo49o2rQpGo2G0NBQXFxcijxuUFAQBw8exNHRkd69e7NixYpimZBW3OsP51USpT1ffPEFoP0di9uGDRtYvXo1AQEBeHl5cefOHSZOnMiMGTN4/fXXi/14RMmSEgghjPT46HNiYiI7d%2B6kd%2B/eDBkyRLW4j5d9JCYmsnbtWjp06MBrr72mWlyAe/fuMXnyZE6ePKlbOL9z584sWLBA1WWjcnJyWLt2LTt37iQ6OhonJycCAwMZNGgQly5dIiwsjAkTJhT5WsQbN25k1apVvPHGG9SoUYPIyEi2b9%2Bum0A0YsQIWrduzbRp04o07jfffMP7779P586dcXJy4s6dOxw6dIiVK1eqfvGPy5cv8/bbb2Nubk716tWJjo4mJyeH1atXk5GRwaBBg1ixYgU%2BPj5FGjcyMpJ33nmHW7duYWtry/3796lbty4rVqwgOjqa8ePHs3r1apo1a1akcadMmULv3r1p0aJFkfb7T%2B7cucOgQYOKbf3hvEqqtAe0I9/btm1j8ODB3Lhxg6lTp2JnZ8e8efOoVq1akccD6Ny5M0uXLtX7cnHmzBmmT5/Of//7X1ViimeXJMBCqOD27dtMmjSJHTt2FGvchw8f0rNnT3744QdV40yYMIGMjAzee%2B89nJycuH37Nh999BH29vYsXLhQ1dglZd%2B%2BffkS786dOxMREcGJEycYMGBAkS%2BF5%2Bvry4wZM2jTpo1u26FDh1i2bBl79%2B4t0lgFSUpK4uDBg8TExFCjRg18fHywsbHhr7/%2BIjs7m6pVq6oSNycnh3PnzhEbG4uTkxNNmjTBxMSE9PR0LCwsim297aysLK5evUqDBg1UjVMS6w8DtGrViqNHj%2Bo9b7Ozs2nTpo3uy22LFi1UWZd36tSp/N///R979uyhf//%2BVK1aFSsrKx4%2BfMjq1auLPB5AmzZtOHz4sF6ZVEZGBq1atVJtmUzx7JISCCFUUKNGDW7dulUisdWoy3zcyZMn%2BeGHH3QjrXXr1mXJkiW8%2BuqrqsbNyMggLCyM2NhYcnJyAO1EsatXr6r2oZnL19e3wNUX3NzcVCtJSEhIwNPTU2/bK6%2B8ourV5/KqUKGC3tmErKwsLl%2B%2BrHpCmJ6eTo0aNXQT4W7fvs3Vq1fp1KmTajEPHz7MnDlziI2N1VtNxtzcnIsXL6oWF8DGxqZELsZQkqU9p06dYteuXfz999%2BcPXuWQ4cOUaVKFVXrcf38/Pj0008ZN26crtzkP//5D126dFEtpnh2SQIshJEeX4YsMzOT/fv355tAVNQen8CSmZnJzz//rHede7XY2try8OFDvVKD9PR0rKysVI07ffp0fv75Z2xtbcnMzKRcuXJcu3ZN9fq9%2B/fvs2nTpgITbzVHYr29vQkNDdU7RR0WFsbLL7%2BsWsxcP/30E3Pnzi32hHDnzp3Mnz%2Bf9PR0ve1Vq1ZVNQFevHgxnTt3plKlSly5coXu3buzatUqAgICVIn3pPWO81K7ln/w4MEMHz68wNKeqKgoRowYodqSe8nJyVSpUoX9%2B/fj7OxMtWrVyMjIUGW%2BRufOnTExMSEzM5OoqCi2b9%2BOk5MT8fHxREdHq/6FTjybJAEWwkgDBgzQ%2B9nU1BQXFxfdGpdqeXzin5mZGe7u7rzzzjuqxcxN9jt27MiIESMYP348NWrUIC4ujuDgYNWShVw///wzW7duJTExka1bt7J06VLWrVvHhQsXVI07bdo0bt26hZ2dHUlJSTg5OfHLL7/Qr18/VeINGDAAExMTUlJS%2BOabb9ixYwc1a9YkLi6OCxcuFPl6wwVZsmRJsSaEudasWaOr4z59%2BjSDBg1i8eLFqif9d%2B7c4d133%2BXu3bucOHGCzp07U6dOHSZOnJjvNV4UinqtaiUGDhxI1apV2blzJwcOHMDJyYk5c%2BboSnv8/f1V%2Bd0BXF1d%2BfTTTzly5Aje3t4kJSWxfPlyXnrppSKP9fbbbxd5n6L0kxpgIUSh/dOpfrUvwtGyZUtOnz5NYmIi/fv3Jzw8nPT0dDp06KDqurDNmzcnPDyc2NhYPv/8c0JCQtizZw/ffvutblZ7UXraGqm51F4rtUmTJpw5c4a7d%2B/y/vvvs2nTJq5fv87EiRMJCwtTLW7Tpk05d%2B4ckZGRTJ48ma%2B//pqoqCgGDx7MgQMHVIvr7e3Njz/%2BSFZWFu3bt%2BfYsWPA/55zomhdv36duXPnYmVlxfLly7l8%2BTLz589n5cqVuqtMFpfs7Oxiv5S9KHkyAixEEYiOjiYyMjLflejUvIJUZmYm4eHhREZG6k7L51IrOYqIiFCl38JydHTkzp07ODs7k5CQQEpKCqampiQnJ6sa19zcnGrVqmFjY6O7apavr69uHeCi9ixcCMDOzg5TU1OcnJy4ceMGoK31jomJUTVu1apVyczMpHr16ro1h52cnEhISFA17osvvsiKFSsYPXo0VatW5fDhw1hbW6te1lMS5syZw5w5c566aokalyTOq27dunpLoXl4eKj6xQq0o/yrV6/WK%2BvJzMzk5s2bHD16VNXY4tkjCbAQRlq9enWBlyxVezQ0KCiIkydP4urqqlc3V1wXSigo6TcxMVF1GSk/Pz/69u3Ljh07aN%2B%2BPSNHjsTKyoqGDRuqFhO0kxovXbpEw4YNSU5OJjExEXNzc9UvsXrnzh3WrFlT4JcctU%2Bhl1RC2LhxY2bNmsX7779P7dq12bp1K9bW1lSpUkXVuO%2B%2B%2By7jxo2jT58%2BjBs3jlGjRpGTk8N7772natyS8Cyc%2BC2JCa0zZswgMzMTOzs7EhIScHNzIywsjEGDBqkSTzzbJAEWwkgbNmxg1apVT70ghhp%2B%2BeUX9u7dS82aNYstZq6SSvqHDx%2BOs7MzFStW5P3332fx4sUkJSWpchW2vPr27cuAAQPYt28f3bt3Z9CgQZibm6s6wg8wceJELC0tadWqVbEt/ZWrpBLCadOmMXPmTJKTk3n33XcZMWIEaWlpqo9I3r9/n71792JmZkaNGjU4dOgQycnJqp%2BOT0lJoVy5cqrGeNzcuXMBcHFx4c033yzydbMLoyQmtF68eFG33NzKlSuZM2cO3t7eqpQxiWef1AALYaSXX36ZI0eOFHsNWZcuXdi5c2exrBf6OE9PTxYuXFjsSX9JunDhgm7m/vr160lOTuatt96icuXKqsV0d3fn%2BPHjWFtbqxajsOLi4oolIXxcVlYWmZmZqi3HlcvT05OffvpJ9TiP8/HxYe/evSXyOvbw8OD48eMlUv/q6en5xAmty5cvVyVmmzZtOHbsGMnJyfj5%2BXHw4EE0Gg1t2rTh%2BPHjqsQUz67iHVIQ4l%2BoX79%2BLFu2jKSkpGKNO2XKFMaPH8%2BPP/7I6dOn9f6pzdzcnPbt25eZ5Be0p%2BYtLS2xsLBg%2BPDhTJw4UdXkF7STDtWuuS0sBweHYk9%2BQftcK46k1NnZWfX1fp8kNTW1ROK%2B8sorfPHFF8TFxRV77JycHOrUqUOdOnV0Z4369evHr7/%2BqlrMWrVq8fPPP1O%2BfHmysrKIjIzk3r17ZGVlqRZTPLukBEIII9WpU4egoCC%2B/PLLfPvULAc4f/48R48ezTd5Q%2B0yBPhf0j9ixIgSGbkqK2bOnMngwYN1y5Hl9SxMlPs3qVy5MkOGDKFmzZo4ODjofblTs97a09OT3r174%2BXlhYODg94%2Btf/GZ86cYd%2B%2BfQWWM6n9HlISE1qHDRvG6NGj2bdvH3369CEwMBBzc3O8vb1ViymeXZIAC2GkDz/8kLfeeos2bdoU66nELVu28Pnnn9O2bdtirw8tqaS/rAkODiYlJYXff/9d729clkbei4u7uzvu7u7FHvfu3bs4Ozvzxx9/6Fa9gOL5G6u1iklhlMSE1rZt27J//34cHBwYM2YMtWrVIikpSfW1rcWzSWqAhTBS8%2BbNOXPmTLHHbdu2LYcPHy6R%2Br327dvj5%2BdXYNLv4eGhWtwPPviAmTNn5tv%2B3nvvleiHuVrc3d35/vvvee6554o9dnx8PPb29vm2X7t2DVdXV9Xinj9/Xu/SvLmOHDmCl5eXanFF8fvuu%2B9o164dOTk5ugmtEyZMwNnZWZV4HTp0YM%2BePXLWSgAyAiyE0Tp16sT333%2Bv6mVaCzJkyBCWLFnCiBEjVK9FfdzDhw8JCgoqllixsbG6CSrbt2/PN0L08OFDvv/%2Be1WPYeTIkQUuzdS/f382b96sWlwHB4cSW4e2S5cunD17Vm9bdnY2gYGB%2BbYXpSFDhuTrPykpifHjx3Pu3DnV4uZefe9xFhYW2NnZ4e3tTbdu3Yo87jfffPPEfWpf4rukde3aVff/3JUp1JSTk0NaWpokwAKQBFgIo6WlpTF%2B/HhcXFyoUqVKsdUOfvXVV0RFRbFhw4Z8%2B9QuQyjOpN/W1pbNmzeTmJhIRkYGK1eu1NtvZWWlSq3k3bt3dcnJL7/8ku/qbElJSbqLYqhl6NChjBo1ioEDB1K5cmW955YaS7D9%2BeefDB06FI1GQ2pqKh06dNDbn5aWRo0aNVSJ6%2BvrS3Z2NhqNhvr16%2Be7T7NmzYo8bl5NmjQhNDSUPn364OzsTFRUFKGhoXh5efHcc8%2BxYMECEhISivzSwI8/n//%2B%2B29SU1Np3rz5vzIBftIXjbzUet9s1aoVffr0oV27dvnqvEeMGKFKTPHskgRYCCPVrVuXunXrFnvcDz/8sNhj5irOpN/S0pIdO3YA2oSwoLpjNTg5OXHt2jUSExPJzs7m5MmTevutrKyYPXu2qscwa9YsgHwre6g10fH5559nxowZ3L9/nzlz5uT7YmFlZaVK4v3888%2Bzfft2Hjx4wPDhw/Oty2plZUW9evWKPG5eZ8%2BeZfXq1XoXcunQ4f/bu/OoKMv2D%2BDfYUdJWQVRMjELCxVFQEBFcCsEjUUJBIRUFheQSHMNZQmDN1cEQ9FShEpRASUwC4vXXFA0FREtNxjWGFzAhe35/eFhXhHQfjL3PAjX55zOiWc4fG%2BQ5Zp77ue6JiA6OhrR0dGYPn06AgMDJV4A//rrry3e5jgO27dvx927dyWa0xY%2BehCbmZlJNe9Zt2/fho6ODgoLC1s8eRUIBFQAd0N0BpgQ8v/2/G7os1jfuf7PP/9AU1MTdXV12L9/P9TV1fHBBx8wzVy1ahXCw8OZZnQ2Z86cYXqeuz3NnQGa1dTUQEFBAQoKCkxzR40ahTNnzrS42bCpqQmjRo0SH8kYOXIk0%2BMfzRobGzFu3Djm43n57EHcrL6%2BHvfu3YOamhov9zOQ7ot2gAnpoOXLl7f7GIvpVfb29khPT3/hEIpffvlF4rnP4qsF1759%2BxAREYELFy4gOjoaGRkZEAgEuHHjBubPn88sNzw8XDwK%2BcGDB9i2bRvU1dXFE%2BFYKSkpafcxXV1dZrkAMHDgQHz55ZdYsWIFzp49i4CAAKipqWHTpk1MX/Goq6vDggULsHXrVvz8888ICgpCz549ERsbC2NjY2a5enp6SElJwYwZM8TX0tPTxV/n/Pz8Nm8KZOHmzZtS6/Tx6NEjXgrg2tpahIaGIjMzE3V1dVBSUoKDgwOWLVvG9MnO8ePH8f3330MoFKJPnz5wcnJicrabdH5UABMiYdXV1Th16hScnJyYfHwfHx8AT4tQvtphSbvob5aYmIitW7eisbERBw4cwPbt26GlpQUPDw%2BmBXBcXBx27NiBc%2BfOISwsDJcvX4aMjAzKysqwcuVKZrnNT3KaX6h79t%2Bb9Tnv0NBQPHz4EBzH4csvv4StrS2UlZURFhaG7777jlnul19%2BiT59%2BoDjOKxfvx4BAQHo2bMn1q1bh3379jHLXbJkCfz9/ZGSkoJ%2B/fqhpKQEV69exebNm1FQUAB3d3cm/9bPn4mtr69HYWEhpk2bJvGs5/HZg3jt2rW4ffs2YmNj0bdvXxQVFWHLli34z3/%2BgxUrVjDJzMjIwKpVqzBz5kyMGTMGt2/fxsqVK/Ho0SNmv69J50UFMCEd1FbB98cffyApKYlJnr29PYCnbdCe/6MFgOkkpfawLvqblZaWwtLSEnl5eZCTkxPfGHX//n2muYcPH8bevXtRV1eHrKws/PDDD9DS0sK0adOYFsDP7%2BSLRCLs2LGj1c1pLFy6dAkZGRmorKxEQUEBEhIS8MYbbzA/w1lYWIht27ZBKBTizp07cHNzQ8%2BePfH1118zzbWwsEBGRgbS09NRWloKa2trbNy4Edra2igrK0NSUlKbN%2Bd11PNfTxkZGXh5eWHixIkSz3oenz2Is7OzkZmZCQ0NDQBPe4sbGBhg%2BvTpzArgb775BjExMbCwsBBfs7GxQXh4OBXA3RAVwIQwYGFhgYCAAKYZH330EaKjo2FpaQng6c0zW7ZsQXx8PC5fvsw0W9pFf7PevXvj9u3byMrKEp9PPXXqFPOXpisqKmBgYICTJ0/ijTfegIGBAQD2I2yf77jQr18/hIeHw8HBgfkO4aNHj6CkpISff/4Z77zzDtTU1FBTU8P0yAcANDQ0gOM4nDhxAu%2B//z5UVFQgEomk0g6uX79%2Bbd4MpaOjAx0dHSaZfE7027NnD2/ZioqKrc789uzZk%2BnYa6FQCHNz8xbXzMzMUFpayiyTdF5UABMiYQ0NDTh8%2BDDU1dWZ5ixYsAALFiyAt7c3HB0dsXTpUpSXl0utS8LzpFH0e3t7i3fA9%2BzZg3PnzsHX15d5NwZtbW3k5ubi0KFD4j%2Bghw8fZtaw/2VY73gDwLBhw7BmzRqcO3cOH374If755x%2BEhoYyvzHOwsICixYtwtWrVzFnzhwUFRVh6dKlGD9%2BPNNcvhQVFYl3vJuamlo8xrKNIsBvD2I/Pz8EBARgxYoVGDBgAMrLy/H111/D1ta2xdl3SZ5179OnD86dO9ei08fZs2fRt29fiWWQ1wd1gSCkgwwMDFq9ZCgrK4uVK1fC1dWVaXZhYSH8/PxQUVGBiRMnIiIigpcbWpqL/tjYWBw9epRpVlFREeTk5NC3b1%2BIRCKUlJQwHZ8KAFlZWVi6dCmUlJSQnJyM8vJy%2BPj4YMuWLUwLs%2Be7bdTX1yMnJweampqIj49nlgs83fVev349FBUVsWrVKly5cgVxcXEIDw9nOpmutrYWO3fuhKKiInx8fHD16lXs378fwcHBTHcH%2BeLs7AwFBQWMHj261Uhz1rvDNjY2Ld5%2Btgcx693h5ldRALQ45/7s25Ju9/fDDz/g66%2B/hqurK/T09FBUVITk5GQsWbKkxc2PpHugApiQDjpz5kyLt2VkZDBgwADmL8s/evQI0dHROHjwIExMTPDnn38iJCREKnc081n019XV4bfffoNQKISLiwtu377d4o8pK0%2BePAHw9KXbmpoaPHz4sM0z2JL0fM9ZWVlZDBo0CL6%2BvsyzOwORSMT8lRS%2BjRgxAidPnoSSkhLfS2nRg3jp0qVMs65du4aePXu%2B9P0kPXhl3759SElJQVVVFfr164cZM2Zg6tSpEs0grwcqgAl5TU2aNAlKSkpYv349Bg8ejIyMDKxZswaWlpbYsGED02y%2Biv47d%2B7gk08%2BQX19Pe7fv48DBw7Azs4OMTExsLa2ZpotEomQlpYGoVCIwMBA5ObmMs/k248//og9e/agoqICBw8exLp16xAZGfmvCpdXVV9fj5iYGCQmJqKxsRHp6elYvHgx4uLiumTR7%2BrqisjISLz11lt8LwVA1%2B9BfOvWLWhqakJFRQV//vknevXqhYEDB0p1DaRzoDPAhHTQ9evXERUVhVu3brU6w8eyH6%2BFhQVWrFghvjnI1tYWw4cPx5IlS5hlNuNjQAIAREREwNHREf7%2B/jA1NcXAgQMRHh6OzZs3My1G8/Pz4e3tDX19fRQWFsLT0xOBgYEICQlhfvf4xYsXcfPmTTy/V8H6jOa3336L5ORkzJkzB1FRUejZsyfKy8sRGRnJdChITEwMTp06hU2bNiEoKAgaGhrQ0dFBREQENm3axCz39OnTWLt2LW7dutXqa82y5dyqVavg5eWFyZMno1evXi0e4%2BMGua7cg/jo0aMIDg5GcnIyDA0NcfbsWWzduhWbNm3C2LFjpbYO0jnQDjAhHeTq6gplZWV8%2BOGHre6Qd3BwkPp66uvrIS8vzzSDr6LfzMwMOTk5UFBQgKmpKc6cOYOmpiaYmpoybf/m7u4OR0dHODo6wsTEBLm5ucjJyUFkZCQyMjKY5a5fv17c6/jZ7y2BQMB82MmUKVMQGxuLQYMGib/WFRUVcHBwYLo7aGNjg%2BTkZGhra4tz79%2B/j0mTJrUaRy1JDg4OMDAwgL29faufY5ZP%2BPz8/JCXl4fBgwe3OAMsEAiY3wT3oh7Ea9asYZq9fPlynDx5Uqo9iO3s7LBkyRJYWVmJr/3222/YsGHDC28IJF0T7QAT0kGFhYX4/fffpf5S3p07d7B161aUl5eLi9D6%2BnrcvHkTp06dYpr9xRdfQFlZGT4%2BPszbYj3rjTfewD///NPizvDKykr07t2bae61a9cwffp0AP/rkTp27FgsXryYaW5aWhq2bdvW4g%2B2tFRXV4tfGm7eJ9HQ0EBDQwPT3IcPH4rP/TbnKikptbpBTNJu3bqF77//Xirt1p51%2BvRp/Pzzz0xvLGxPd%2BtBXFJS0upnady4cQgODmaWSTovKoAJ6aA%2Bffqgrq5O6rkrV64Ex3FQU1NDVVUV3nvvPRw6dAheXl7Ms/kq%2Bu3t7bFw4UIEBwejqakJFy9eRHR0NPObWNTV1XHjxg0MHjxYfO3GjRvMi5ba2lqMGzeOaUZ7DAwM8MMPP8DV1VVclGRkZLT4GrBgZGSEmJgYBAUFiXP37NmDoUOHMs196623UFFRIfXWdn369JF60d2su/Ug1tXVxYkTJ8S904GnfcSpDVr3RAUwIR3k7u6OBQsWwNPTs1VBZGJiwiz38uXLOH78OEpKSrBx40asWrUK48aNwzfffMP8DxtfRf/8%2BfPx%2BPFjLFy4EI8ePYKHhwecnZ2Zf75ubm7w9fWFn58fGhoakJGRgbi4OLi4uDDNHT9%2BPNLT06UyFvd5n3/%2BOby8vJCamoqHDx9i3rx5uHDhAnbs2ME0d%2BXKlZg9ezYOHjyI2tpa2Nraora2Frt27WKa%2B%2BGHH2Lu3LlwdnZudTMny/PWc%2BbMwfz58%2BHp6YnevXu32AFl%2BfsD4LcHMQD8/fffSE5ORllZGcLCwnDkyBG4u7szy5s3bx78/f1ha2sLXV1dlJaWIjMzE19%2B%2BSWzTNJ50RlgQjqovRZcku5h%2BTwLCwv88ccfqK2thZ2dHbKzswEA5ubmOHnyJLNcAEhMTMSRI0ekXvQ/SyQSQU1NTWo37OzduxdJSUkQCoXQ1taGi4sLvLy8mL40HxAQgGMjHlf8AAAgAElEQVTHjuGtt95q9XWWRoFSXl6OtLQ0lJSUQEdHB/b29hIdTNCeR48eITs7W5w7fvx45q82PN8Ttxnr89Z8/f4A%2BO1BfOLECSxatAjW1tbIzs7GkSNH4OjoCG9vb/j4%2BDDL/eOPP3Do0CFUVlZCR0dHfK6fdD9UABPymvr444/h7%2B8PKysrWFlZITExEQoKCrCzs0Nubi7TbL7%2BaD8/GKKZgoIC1NTUYGFhIfG%2BoXxq7/MF%2BH35mqVnp4A9S15eHr1794aCgoKUV9R18dmD2MnJCQEBAbCyshLfWHrp0iUsXryY%2BQ2ehAB0BIKQV1ZWVgYdHZ12/2ADkh3j%2BTwfHx8EBATg8OHDcHFxwccffwxZWVlMmDCBWWazq1evMs9oy7Vr13D06FEMHToUenp6KCkpwYULFzB06FA0NjYiIiICcXFx4nHFkrJ8%2BfI2r8vLy0NdXR3jx4%2BHkZGRRDMBfovctoadAICcnBzU1dVhbW2NZcuWSbx4mjRpUquX45vJyMjAwsICX331lcQGZJw7dw7GxsbtPmkUCAQtRueywNdwFwMDA5SVlfHSg/j27dvi8%2B3N32dDhw7FvXv3mORlZGSgrq4OH330EUQiEYKCglBQUIApU6YgJCREqjfzks6BdoAJeUUjR45EXl6euFBo/lFiNcazLeXl5VBXV4e8vDwyMjJQU1ODjz76qMvukn366acYNWoU3NzcxNdSUlJw%2BvRpREVFISMjA7t27cK%2BffskmvvFF1/gwIEDmDhxorjwPnr0KCwsLKCoqIicnBxERERIZQqftHz77bdITU3F4sWLoaenB6FQiC1btsDU1BTGxsbYuXMn3n33XaxatUqiuYmJicjOzsaKFSugp6eH4uJiREVFwdDQEJMnT0ZcXBzk5OQQHR0tkbxnf47bwvrnmM/hLvn5%2BViwYAEvPYinTZuGkJAQGBsbi9vdXbp0CStWrEB6erpEs9LS0hASEoLPPvsMs2bNwmeffYaCggIEBQVhz549MDMzw/z58yWaSV4DHCHklZSUlHAcx3HFxcXt/kcky8zMjGtoaGhxraGhgTM1NeU4juOampq4kSNHSjx37ty53M8//9zi2vHjxzlfX1%2BO4zju1KlTnJ2dncRz%2BfThhx%2BKv8eblZWVcR9%2B%2BCHHcRz3zz//cJaWlhLPnThxIlddXd3i2t27d7kJEyZwHMdxDx48EP97dwU%2BPj7c1q1buaamJm7UqFEcx3HcgQMHuI8%2B%2Boh5tq%2BvL2diYsK5ublx7u7u4v88PDyYZx8%2BfJgzMTHh1q9fzxkZGXHx8fHc2LFjuYMHD0o8y9nZmcvOzuY4juOePHnCDRs2jPvll184juO4v//%2Bm5syZYrEM0nnR3v%2BhLyi5tY50j5zOmTIkJe%2BD%2BudZ7706NEDly9fxvDhw8XXrly5It7xrqqqgrKyssRz//zzT3zzzTctro0dO1bcP9TMzAxCoVDiuXxqfnXhWb1790ZpaSmAp63hHj9%2BLPHc6upqyMrKtrgmEAhQVVUFAFBWVm73iMTr6MKFC9iyZQsEAoH4KMD06dMRERHBPJvPHsRTp06FiooK9u7dC11dXZw6dQorV67ElClTJJ518%2BZN8XGL/Px81NfXi4ebDBw4EOXl5RLPJJ0fFcCEvCK%2BCtHevXujoaEBU6dOxaRJk7rscYe2eHl5wcfHBx9//DH69esHoVCIffv2Yc6cOSgpKYGfnx%2BTnsDq6urIyclp0UT/5MmTUFVVBfC0nRSLYRz%2B/v6Ijo6Wer9l4OkNUmFhYVi9ejUUFRXx5MkTfPXVVzAyMgLHcfjhhx8waNAgiec2P7FYuXIldHV1UVJSgujoaIwZMwZ1dXXYunUr3n//fYnn8oWv4S4Avz2IAcDKygoWFha4d%2B8e1NTUWj3xkZSmpibxk4s///wTgwYNEv9M3b17l87/dlP0r07IK%2BKrEM3JycGvv/6KlJQULF26FLa2tnB2dpbKTTPPEolESEtLg1AoRGBgIHJzc5mfWfT09ISGhgZSUlJw9OhR6OrqYs2aNZg8eTKuXr0KR0dHeHh4SDx30aJFWLhwISZPnoz%2B/ftDKBTi2LFjWLNmDW7cuIHZs2cz6V96/vx53p7grF27Fr6%2BvjA2Noaamhqqq6vx9ttvY/PmzTh9%2BjQ2bNiAuLg4ieeGhIQgODgYU6ZMERct48ePR0REBM6ePYvjx49j/fr1Es/lC1/DXQB%2BexDX1NQgLCwMmZmZqKurg5KSEhwcHLBs2TKJf8%2B/8847OHnyJCwsLJCVldViEMaJEyfw9ttvSzSPvB7oJjhCXlF9fb24EL18%2BTIvhWh5eTkOHjyIgwcPomfPnnB2doa9vT3eeOMNprn5%2Bfnw9vaGvr4%2BCgsLkZaWhqlTpyIkJAROTk7McsPCwhAUFMTLjuj58%2Bdx4MABlJaWQldXFzNnzoShoSFu3bqFv/76i8n42PDwcBQXF8Pe3h5aWlpSH5LQr18/nD9/HuXl5dDV1cXw4cMhEAjw5MkTyMvLM%2BmBfPbsWYwYMQL//PMPysrKoKur22owhTTV1NQw/X6rr6/H%2BvXr8f333%2BPRo0dQUlKCs7Mzli5dyvzJD589iJcuXYrbt28jICAAffv2RVFREbZs2YKRI0dixYoVEs06duwYPv/8c%2Bjq6kIoFCI1NRV6enpYv349kpOTERISAjs7O4lmks6PCmBCJICvQvRZZ86cQVhYGIqKinDhwgWmWe7u7nB0dBQ3kc/NzUVOTg4iIyORkZHBLNfU1BR//PGH1F%2By5OsoAp8FioWFBY4ePSr1z9nMzAzHjx9ncpb7RZo7ETxv1KhROHv2rFTWIO3hLnwyMTFBZmYmNDQ0xNfKy8sxffp0nDp1SuJ5Z86cwfnz52FjYyMe5/3xxx9j2rRpLbrKkO6DjkAQIgHa2trw8/ODn5%2BfuBCNiopiXogCT/tpHjx4EKmpqaivr4erqyvzzGvXrmH69OkA/tfDc%2BzYsVi8eDHTXCcnJ4SGhsLBwQF9%2BvRpUSiw7LnM11EEvvotA4CqqirKy8ulXgDr6enh0qVL4puUWLp9%2Bza%2B%2BOILcByHmpoaeHp6tni8pqamVXswSZkzZw4SEhLEbz9%2B/FhivY3/P/jqQayoqNjqzG/Pnj2ZPfExNTVt9T31/fffM8kirwcqgAmREGkWojU1Nfjpp5%2BQkpKC/Px8jB8/HqtXr4aVlRWzG0mepa6ujhs3boh3UgDgxo0bzO8m37VrFwDgxx9/FBe/nBR6LtvZ2SEgIICXowhlZWVIT0%2BHUChEnz59YGdnhzfffJNpJgAMHjwYM2fOhJGREfr06dPiscjISGa5vXv3hre3N/r379/qSY6kxz8PGDAAkydPRnV1NfLy8loVSAoKCu2OSO6o8%2BfPt3h73Lhxbe5As/R8D2IrKys4OTlJpQexn58fAgICsGLFCgwYMADl5eX4%2BuuvYWtr22K4kDRGb5PuiQpgQjqAj0I0ODgYv/zyCwYOHAgHBwfExsZKfefIzc0Nvr6%2B8PPzQ0NDAzIyMhAXFwcXFxemuXyNSE1MTAQAHD9%2BvMV11oX3pUuX4OXlBX19ffTv3x%2BXLl1CfHw8EhISYGxszCwXeNpybvLkyUwz2jJixAiMGDFCanmzZs0CAPTv3x8fffSR1HKfx8dpxIiICDg6OsLf3x%2BmpqYYOHAgwsPDsXnzZuYFcHh4OADgo48%2BajFICAB27twptWFCpPuiM8CEvKLnC1E7OzupFKIGBgZQU1PD22%2B/3e5ZQUnvlLVl7969SEpKglAohLa2NlxcXODl5cXkxqgXaWhowLVr1/Dee%2B9JNVcaPD09MXHixBYvzX/33XfIzMxEcnIyjyvrmv7880/cuXMHjY2NLa6zKIybJ9A1a%2B8MMktmZmbIycmBgoKCOL%2BpqQmmpqbMzz3/277Z0u6zTroP2gEm5BUdOXIEampqUFFRwbFjx3Ds2LFW78OiEGU9ovTfmjVrlnj3TFqOHz%2BOtWvXory8vMWOkZycHC5dusQ0%2B9GjR7h37554CEN9fT2uXbuGSZMmMcssLCzEzp07W1xzc3PD5s2bmWU2q6urQ3p6OsrLy1t9zizanzWrrq7Gnj172sxNS0tjlrthwwbEx8dDU1MT8vLy4usCgYDXnWGW%2BOxBvGXLFjg5OTE/QkRIe6gAJuQV8VWI/n9y16xZgzVr1kh8DY2NjcjKysKtW7daTeVi%2BXX5z3/%2Bg8mTJ6NXr14oLCyEnZ0dtm7dCmdnZ2aZAJCSkoKwsDA8efKkxXUNDQ2mBbCysjJKS0uhp6cnvlZaWiqVAmXFihXIycmBmpoa6uvr0aNHD1y/fp15Mbh8%2BXLcunUL6urqqKmpga6uLv773/8yf7L1448/4ttvv4WZmRnTnGYNDQ04dOiQ%2BO36%2BvoWbwNsdp6fxWcP4h49emDRokV444034ODgAEdHR%2Bjo6DDNfPjwIb7//vs2d/nDwsKYZpPOh45AECIFrArRl3n%2BZVZJWbVqFY4cOQIDA4MWLckEAgHT4xfDhw/HuXPnUFxcjNWrV2PPnj3466%2B/EBQUhPT0dGa5kyZNwqxZs9CzZ0/k5uZi9uzZiI6OhqWlJebNm8csNyoqCn/88QeCg4PRv39/3LlzBxs2bMCYMWPw2WefMcsFnr48npycDJFIhOTkZHz99dfYuXMnLl68iI0bNzLLNTY2RkZGBsrLyxEfH4%2BYmBikpqbi8OHD2L59O7PcMWPG4L///S%2Bzj/%2B8l91cJxAImJ9557MHcXN%2BdnY2Dh48iBMnTsDExAROTk6YOHEik/zFixfj3LlzMDY2brHLDwDR0dESzyOdHEcIYW7EiBG85BoZGTH5uBYWFtzFixeZfOwXGT9%2BPNfY2Mg9efKEMzc3F18fNWoU09zhw4dzTU1NXFFREefi4sJxHMcJhUJu0qRJTHMfP37Mff7555yhoSH37rvvcsOGDePWrl3LPX78mGkux/3va1pVVcV9%2BOGH4vVYWloyzTU1NeU4juPu3bvHTZw4keM4jquvr%2BcsLCyY5q5atYpLT09nmtGZVVVVcU1NTbzlnz9/nnNwcODeffddztTUlFu3bh13//59iWYYGRlxZWVlEv2Y5PVFRyAIkQKOpxdaWDXUb2pq4uWms3fffRebNm3CggULoKGhgd9%2B%2Bw1KSkpQVFRkmquhoYH6%2Bnr07dsXN2/eBPC0PVNVVRXTXEVFRaxbtw6hoaG4d%2B8eNDU1pTYkQUdHB0VFRdDT00NVVRUePnwIGRkZ1NbWMs3t168fLl%2B%2BDENDQ9TW1kIkEkFOTg6PHz9mkufh4QGBQIDa2lqkpKQgPj4eqqqqLd5HGjeVSlNn6UFcWVmJw4cPIzU1FX///TesrKywcOFC6OrqYuPGjfD39xd3YJEETU1NXj5P0jlRAUyIFHS1yU52dnZISEiAj4%2BPVHOXLFmCgIAAzJw5EwEBAZg/fz6ampqwdOlSprnDhg3DF198gdWrV%2BOtt95CcnIylJSUWhVKklZbW4t9%2B/bBy8sL9%2B/fx/z586Guro7Q0FBoa2szzba3t4ebmxv279%2BP8ePHw9/fH4qKijA0NGSa6%2BbmBg8PDxw5cgR2dnaYPXs25OTkmN0s9eyZX9atvzqLztCDeM6cOTh58iQGDRoER0dHTJ8%2BvUVx%2Bumnn0q8raKLiwv%2B85//ICAgAD179pToxyavHzoDTIgUsDqLy1eum5sb8vLyoKys3GpHRZq9eisqKlBbW4uBAwcyz1m1ahXCw8Nx584d%2BPn54fHjx4iMjIS9vT2z3GXLlqGgoACpqalwd3eHhoYGFBUV8eDBA6adGJr99NNPsLKyQlNTE6Kjo1FTU4PFixe3uCmPhYsXL8LAwAACgQC7du1CbW0tPvnkE6nc/NcdPP97oXmcuTSFhITA2dkZQ4cObfPx2tpalJWVYdCgQR3Oev/998W9hhsbGyEQCFr1ab98%2BXKHc8jrhQpgQqSgqxXABw8ebPcxBwcHief9mz/O0myn1NDQgPr6emZjW5vZ2NjgwIEDEAgEMDc3R3Z2NlRVVTFmzBipFyxdXfNRiOfJy8tDXV0d1tbWsLW15WFlksdnD2IbGxtxMdreK2OSfhJ98uTJl76Pubm5RDNJ50dHIAjpwlg9v2VR5L6Ih4fHCx9nNTHq%2BbZUbWHZqqq2thaqqqrIzMyEnp4etLW1UVdXx/RITXuF4LNYnIltLoxehOWrC8OHD8cPP/yAmTNnQk9PDyUlJfjhhx8wbtw4aGpqIiIiAlVVVS/9XiQvtmjRIgBPfzeFhoYiJCSEeWZzcRsZGYnly5e3enz58uVUAHdDVAATIgWsX2gRiURt3twRGBgo0RwfHx/Ex8e/sEhiURxdvXpV4h/z33jZwAnWQxIGDx6M2NhY/P7777C2tkZNTQ02btyI999/n1mmtPrgPq%2B5MOJLXl4e4uLiMGrUKPG1CRMmIDo6GtHR0Zg%2BfToCAwO7RAHMZw/iZ588r1u3jvmT6fLycvHu9vfff4%2BhQ4e2%2BH384MEDZGZmIjIykuk6SOdDBTAhEiStQhR4%2Bkdsy5YtSExMRGNjI9LT07F48WLExcWhT58%2BAAAvLy%2BJZhobGwPgr0iStl9//ZXX/DVr1mDt2rVQUVHBwoULceXKFZw%2BfZrpJDi%2BBrxI%2B1WF5127dg0jR45scW3o0KG4cuUKgKcjyCsrK/lYmsRpamq2%2BB5SU1Nr8XZXmn6nqqqKnTt3QiQSoa6urlW/X0VFRfj7%2B/O0OsInOgNMSAf9m0KUhQ0bNuDUqVNYtGgRgoKC8Ntvv2HJkiWQk5PDpk2bmOUS0hU5ODjAzc0NM2bMEF9LTU3F9u3bcfjwYeTn5%2BPTTz9FVlYWj6vsWqR59hgAZs%2Beje%2B%2B%2B05qeaRzox1gQjpoy5YtOHXqFDZt2oSgoCBoaGhAR0cHERERTAvR9PR0JCcnQ1tbGwKBAD169EBkZCTT0bzNamtrkZSU1OYoZHopsePi4%2BPh4%2BODmJiYdt%2BHr53armrJkiXw9/dHSkoK%2BvXrh5KSEly9ehWbN29GQUEB3N3dsXLlSr6XSTqgufgtLCxEcXExxo0bhwcPHlBv4G6KCmBCOoivQvThw4fiX9zNL%2BQoKSlBRkaGaS7w9KaR8%2BfPw8zMrNVIUdJxubm58PHxwenTp9t8vKv1le4MLCwscOTIEaSnp6OsrAzW1tbYuHEjtLW1UVZWhqSkJAwZMoTvZb72Xnb2GGB3/lgkEiEgIADnz5%2BHgoIC9u/fj5kzZ2Lnzp0YPnw4k0zSedERCEI6aPTo0cjJyYG8vLy4n2ZdXR2srKz%2BVfudV%2BXn54d3330XQUFB4pcSExIScPr0acTHxzPLBZ6eAd6/fz/zfrBtEYlESEtLg1AoRGBgIHJzc7v0AAOO49DU1ARZWVlUVlZCXV29VQ9TFvz9/REdHQ0VFRXmWaT7sLGxeeHjAoGAWbeP4OBgKCsrY9myZbC2tkZubi5iYmJw8uRJ7N27l0km6bxoB5iQDjIyMkJMTAyCgoLEO3N79uxpt8G7pKxcuRKzZ8/GwYMHUVtbC1tbW9TW1mLXrl1Mc4GnN46wnkTWlvz8fHh7e0NfXx%2BFhYXw9PREYGAgQkJC4OTkJPE8vtugXb16Ff7%2B/ti0aROGDRuGHTt24NixY9ixYwfz4R/Nu2TSwlcbNHt7e6Snp78wX5rDXbo6Pm8sPXXqFH7%2B%2BWf06NFD/G/t6%2BtL54K7KdoBJqSDioqKMHv2bDQ0NKCqqgoDBgwQF6L6%2BvpMsx89eoTs7GyUlJRAR0cH48ePl8qO3bZt21BRUYGFCxdK9fycu7s7HB0d4ejoKN5tz8nJQWRkJDIyMiSe17xb1dTUhPLycqiqqkJXVxcVFRX4559/8O677/6rIvlVeXh4wMTEBPPnz4ecnBwaGhqwbds25OXlYefOncxyASA8PBzFxcWwt7eHlpZWi%2BKQxdCR5uEq%2Bfn5%2BOWXX%2BDt7Y0333wTpaWl2LVrFyZMmIDPP/9c4rnp6emwt7eX%2BnAXIn1WVlZITU2Fqqqq%2BPfHvXv3MG3aNPz22298L49IGRXAhEgAH4VoSUlJm9fl5eXRu3dvprt3NjY2KCkpaXPHjMVAimampqY4efIkZGVlW9xBbmxsjHPnzjHL/eqrr6CgoIDAwEDxGevY2FgUFxfjyy%2B/ZJY7atQo5Obmtvg6NzY2YvTo0cwnwRkYGLR5ndXQkWbTpk3Dhg0bWozAvX37Nnx8fKTWgaG9dobk9bZmzRqUlpZi1apVcHJywtGjRxEWFoYePXogLCyM7%2BURKaMjEIR0UHMhamRkBCMjIwDA/fv38ejRI6aF6KRJk1p1YGgmIyMDCwsLfPXVV0z%2BkK9bt07iH/PfUFdXx40bNzB48GDxtRs3bkBTU5NpbkpKCk6cONHiBkMfHx%2BYmZkxLYBVVFRw8%2BbNFq8kFBUVoVevXswym/E1fKSoqAhvvvlmi2va2tqoqKhgmtteO8Nt27ZBS0uLaTaRjs8%2B%2Bwyff/65%2BAZlc3NzjBkzRirT6EjnQwUwIR3EVyG6fPlyZGdnY8WKFdDT00NxcTGioqJgaGiIyZMnIy4uDpGRka0av0uCqalpm9dFIpHEs57l5uYGX19f%2BPn5oaGhARkZGYiLi4OLiwvTXEVFRfz9998tdkUvX77MvBB1cHCAv78/5s6dC11dXZSUlCAhIQGOjo5Mc5uVlZUhPT0dQqEQffr0gZ2dXaviVNIMDQ3x1VdfYenSpVBQUMCjR48QHh4uHsLCSnvtDMPDw6mvdhehoqKCrVu3oqKiAkKhEDo6Oujbty/fyyI8oSMQhHRQYmLiSwtROTk5iReikyZNwr59%2B6Cqqiq%2Bdu/ePTg5OeHYsWOoqanBhAkT2m2l1REXL15EVFQUysvLxcV/fX09RCIRLl%2B%2BLPG8Z%2B3duxdJSUkQCoXQ1taGi4sLvLy8mLZ/27ZtG/bs2YMZM2ZAV1cXRUVF%2BPHHHxEQEIBZs2Yxy21sbERsbCwOHTqEyspK9O3bF46Ojpg7dy7zThCXLl2Cl5cX9PX10b9/f9y5cwd///03EhISmBajN27cgK%2BvL0pLS6Gmpobq6moMHDgQ8fHxTIsVGxsbcTvD5uM19%2B/fx6RJk5j8DBF%2B3L17F%2Bnp6SgpKcGCBQuQl5eHcePG8b0swgMqgAnpIL4K0VGjRiE7OxtvvPGG%2BNr9%2B/dhZWWF8%2BfPMz0r6uzsDD09PaiqqqKoqAiWlpbYvXs3PD094e3tLfG8zmD//v1IS0tDeXk5%2BvbtixkzZmDq1Kl8L4sZT09PTJw4EZ6enuJr3333HTIzM5GcnMw0u6GhAXl5eaioqICOjg5GjhzJvL81X%2B0MifRcuXJFfHPlX3/9hbS0NNja2iIsLKzLjH4m/x4dgSCkg6qrq1vtxgkEAlRVVQEAlJWV2z0i0RFjx45FcHAwVq5cKX55PCoqCpaWlqirq8PWrVvx/vvvSzwXAK5fv47ExEQUFxcjIiIC3t7eGDFiBEJDQ5kWwI2NjcjKympzAh3ryWjOzs5wdnZmmvE8Pj/fwsLCVp0m3NzcsHnzZqa5wNOuG2%2B%2B%2BSb69%2B8P4OlRDADQ1dVllslXO0MiPZGRkfjss88wY8YMmJiYQE9PDzExMYiKiqICuBuiApiQDuKrEA0JCUFwcDCmTJki/oM9fvx4RERE4OzZszh%2B/DjWr18v8VwA6NWrF5SUlKCnp4fr168DeFpACIVCJnnNQkJCcOTIERgYGEBO7n%2B/vlhPRuNr9DNfny/w9IlbaWlpi2EnpaWl6N27N9Pcn376CV988QVqamrE1ziOY959gs%2B%2B2kQ6CgsLxefnm3%2BGrKys8Omnn/K5LMITKoAJ6SC%2BClFVVVUkJCSgvLwcZWVl4DgOBw4cgI2NDS5cuIDU1FSJZzbT19dHcnIyXF1d0aNHDxQUFEBBQYF5YZadnY3du3dLfVeOr9HPfH2%2BAGBra4tFixYhODhYfAZ4w4YNsLW1ZZq7ZcsWzJo1Cw4ODi2Kftb09PRw5MgRXvpqE%2BlQU1PDzZs38fbbb4uv3bp1i3kXGdI5UQFMSAfxWYgCT9tGJSQk4LfffsPgwYOxZMkSpnkAEBgYCH9/f1haWmLOnDmYOXMmZGVl4erqyjS3qakJ7733HtOMtpw%2BfZqX0c98fb7A039jkUiE%2BfPno76%2BHoqKinBycsKiRYuY5paWlmLhwoVSLX6bKSsrMy/wCX9cXV3h5%2BcHf39/NDQ04OjRo4iNjcWMGTP4XhrhAd0ER4iEnD17tkUhOnPmTGYdApqampCZmYldu3bh%2BvXraGhoQFxcHMaOHcskry1PnjyBvLw8ZGRkcPHiRTx48ACWlpZMMyMiIqClpQUfHx%2BmOc8bN24cjh07JtXRwAB/n%2B%2Bz6urqcO/ePWhqakrl6IW7uztWrVrV7iAOSeNrBDORPo7jsHv3biQlJYl3%2BWfMmIF58%2BZJ5XubdC5UABPSAXwUot999x12796NpqYmuLq6YubMmfjggw%2BQmpoKbW1tZrnNOI5rNawgIyMDU6ZMYd6ay83NDXl5eVBWVm7VV5llkcLX6Ge%2BPl/g6bnnffv2wcvLC3///TeWLVsGdXV1hIaGMv0%2BW79%2BPX788Ud88MEHrV6aZnHjX/MIZI7jEBoa2uZQBBqF/Ho7d%2B4c8z7S5PVDBTAhr4ivQtTAwABubm5YtmyZeEdy9OjRUimAHz58iE8%2B%2BQSampqIiYkBAFRVVcHa2hqGhobYsWMHevTowSy/uVhpC8siha/Rz3x9vgCwbNkyFBQUIDU1Fe7u7tDQ0ICioiIePHiAuLg4ZrkeHh5tXhcIBNi9ezezXAAtxmuTrmPkyJHIy8vjexmkk6EzwIS8osjIyFaFqDSsXr0aSUlJsLKywsyZM%2BHm5ia1l%2B/i4uIgLy%2BPtWvXiq9paGggOzsb/v7%2B%2BOabbxAUFMQsn6%2BdOL5GP/O583jmzBkcOHAA9%2B7dQ15eHrKzs6GqqooxY8Ywzd2zZxOTL7QAAA9OSURBVA/Tj0%2B6H9rnI22hApiQV8RXITpr1izMmjULJ0%2BeRGJiIiZNmoTGxkacPHkS9vb2TI8hZGVlYfv27dDQ0GhxXUNDA2vXrsXixYuZFMA%2BPj6Ij4%2BHh4dHu19jlruDQ4YMaTFwpFlzf1pJa95dfxHWfYBra2uhqqqKzMxM6OnpQVtbG3V1dUy/x3/%2B%2BWfk5ubC0NAQdnZ2LYZfrFmzBmvWrGGWTbouOt9L2kIFMCGviM9CFADMzc1hbm4OoVCIpKQkrFu3DlFRUZg2bRqWLVvGJLOqqgoDBgxo87EhQ4agsrKSSW7z%2BT0zMzMmH789xcXF8Pf3x19//YV%2B/fohPDwco0ePFj9ua2vL5KXVl00NlMYf9MGDByM2Nha///47rK2tUVNTg40bNzIbrpKUlISNGzfCzMwMaWlpSE9PR2xsrLjtXFpaGhXA5JU8evQIEyZMeOH70I2O3Q%2BdASZEQpoL0ZSUFMjIyDAtRNtSV1eHtLQ0JCUl4cCBA0wyrKyscOjQIaipqbV67O7du5g6dSpOnDjBJJsPCxcuRO/eveHp6YnMzEx8%2B%2B23iI%2BPh4mJCQBgxIgROH/%2BPM%2BrZOOvv/7C2rVroaioiI0bN%2BLKlSsICwvD5s2bMXDgQInnffDBB1i3bh2MjIxQVVWFefPmYdCgQYiOjgbA7mt96NAh8f%2BvXbu2zZvgaErY623YsGEtjm21hW507H6oACZEwqRRiPJl2bJl6N%2B/f5svv8fGxiI/Px9bt25lli/tiWzm5ub49ddfoaysDABITk7Gpk2bcODAAejq6tLNNRJkbGyMc%2BfOid%2BurKzEjBkz4OXlBS8vL2YFsI2NzQsfFwgEtDv4mqOfU9IWOgJBiIQpKCjA2dkZzs7OfC9F4nx9feHo6Ijq6mrY2tpCS0sLFRUV%2BOmnn5CSkoLExESm%2BXxMZHv2HKqrqyuuXbuGRYsWITk5uUveXBMfHw8fH58XnkNmcf5YS0sLFy9exLBhw8Rvb9y4Ed7e3hg8eDCzYx%2B//vork49LOo%2Bu%2BHNKOo4KYELIvzZw4EAkJCQgJCQEe/fuhUAgAMdxeOedd7B9%2B3YYGhoyzZf2RLZRo0YhIiICAQEB4p60K1asgIeHBxYsWNAl/7Dm5ubCx8en3XPIrArR2bNnY968eZg3bx7mzp0LADAyMsLKlSvh5%2BfXasefkH9r2rRpfC%2BBdEJ0BIIQ8kqKioogEomgpaUFXV1dqWRKeyKbUCjEggULoKWlhe3bt4uvV1dXY86cOSgoKGDaB5hvHMehqakJsrKyqKyshLq6OtObO48dO4aKigq4ubm1uJ6VlYWtW7ciLS2NWTYhpHuhApgQ8trgayLb/fv30atXrxbXGhoakJ2djUmTJkltHdJ09epV%2BPv7Y9OmTRg2bBgiIyNx7Ngx7Nixg8lNcIQQIk1UABNCXht8TWTrjjw8PGBiYoL58%2BdDTk4ODQ0N2LZtG/Ly8rBz506%2Bl0cIIR1CBTAh5LXxojG1pqamUlxJ1zdq1Cjk5ua2eLLR2NiI0aNHIzc3l8eVEUJIx9FNcISQ10Z7Ra5IJJLySro%2BFRUV3Lx5E/r6%2BuJrRUVFrY6CEELI64gKYELIa%2BPixYuIiopCeXm5uCtAfX09RCIRLl%2B%2BzPPquhYHBwf4%2B/tj7ty50NXVRUlJCRISEuDo6Mg019/fH9HR0VBRUWGaQwjp3qgAJoS8NkJDQ6Gnp4fBgwejqKgIlpaW2L17N4KDg5lni0QipKWlQSgUIjAwELm5ubC2tmaey5eFCxdCRkYG27ZtQ2VlJfr27QtHR0dxizJWzp8/L7UuH4SQ7ovOABNCXhvDhw/H6dOnUVxcjIiICOzatQsXLlxAaGgo06l7%2Bfn58Pb2hr6%2BPgoLC5GWloapU6ciJCQETk5OzHK7o/DwcBQXF8Pe3h5aWlotziA3j6AmhJCOogKYEPLaGDt2LHJycvDkyRNMmDAB//3vfwEAZmZm7Q5ukAR3d3c4OjrC0dERJiYmyM3NRU5ODiIjI5GRkcEsl0%2BNjY3Iyspqc%2Bw0i0lwzQwMDNq8LhAIqNMHIURi6AgEIeS1oa%2Bvj%2BTkZLi6uqJHjx4oKCiAgoICs%2Blkza5du4bp06cD%2BN8ktLFjx2Lx4sVMc/kUEhKCI0eOwMDAAHJy//tTwfprffXqVaYfnxBCACqACSGvkcDAQPj7%2B8PS0hJz5szBzJkzISsrC1dXV6a56urquHHjBgYPHiy%2BduPGDfF45K4oOzsbu3fvxtChQ6WeXVZWhvT0dAiFQvTp0wd2dnZ48803pb4OQkjXRQUwIeS1MXLkSPz%2B%2B%2B%2BQl5eHi4sLhgwZggcPHsDS0pJprpubG3x9feHn54eGhgZkZGQgLi4OLi4uTHP51NTUhPfee0/quZcuXYKXlxf09fXRv39/XLp0CfHx8UhISICxsbHU10MI6ZroDDAh5LXAcRyKiopa7ARmZGRgypQpkJWVZZ6/d%2B9eJCUlQSgUQltbGy4uLvDy8oKMjAzzbD5ERERAS0sLPj4%2BUs319PTExIkT4enpKb723XffITMzE8nJyVJdCyGk66ICmBDS6T18%2BBCffPIJNDU1ERMTAwCoqqqCtbU1DA0NsWPHDvTo0YPnVXYtbm5uyMvLg7KyMtTV1Vs89ssvvzDLNTMzw4kTJ1qcO66vr8fo0aNx7tw5ZrmEkO6FjkAQQjq9uLg4yMvLY%2B3ateJrGhoayM7Ohr%2B/P7755hsEBQUxy%2BerIwKfZsyYgRkzZkg9V1lZGaWlpdDT0xNfKy0tRe/evaW%2BFkJI10U7wISQTm/y5MnYvn07BgwY0OqxgoICLF68GFlZWczyV61a1W5HhN27dzPL7Y6ioqLwxx9/IDg4GP3798edO3ewYcMGjBkzBp999hnfyyOEdBG0A0wI6fSqqqraLH4BYMiQIaisrGSaz2dHBGlrPmLyIix3vQMDAyESiTB//nzU19dDUVERTk5OWLRoEbNMQkj3QwUwIaTTU1FRQXV1NdTU1Fo9dvfuXSgrKzPN56sjAh9eNlCEdR9gRUVFrFu3DqGhobh37x40NTWZZxJCuh8qgAkhnZ65uTn27t3b5s5jUlISjIyMmObb2dkhISFB6h0R%2BLBnzx5e82tra7Fv3z54eXnh/v37mD9/PtTV1REaGgptbW1e10YI6TroDDAhpNO7efOmeBSxra0ttLS0UFFRgZ9%2B%2BgkpKSlITEyEoaEhs3y%2BOiJ0R8uWLUNBQQFSU1Ph7u4ODQ0NKCoq4sGDB4iLi%2BN7eYSQLoIKYELIayEvLw8hISG4fv06BAIBOI7DO%2B%2B8g9WrV8PExIRp9sGDB9t9zMHBgWl2d2NjY4MDBw5AIBDA3Nwc2dnZUFVVxZgxY5Cbm8v38gghXQQdgSCEvBZGjhyJ9PR0FBUVQSQSQUtLC7q6ulLJpiJXempra6GqqorMzEzo6elBW1sbdXV1dA6YECJRVAATQl4renp6LXrEsuTj44P4%2BHh4eHi0W4BRGzTJGjx4MGJjY/H777/D2toaNTU12LhxI95//32%2Bl0YI6UKoACaEkHYYGxsDeDqdjEjHmjVrsHbtWqioqGDhwoW4cuUKTp8%2Bjc2bN/O9NEJIF0JngAkhhBBCSLdCO8CEEPIStbW1SEpKanMUcmRkJE%2Br6lri4%2BPh4%2BPzwkEcXXXsNCFE%2BqgAJoSQl1i%2BfDnOnz8PMzMzyMvL872cLik3Nxc%2BPj7tDuKgm%2BAIIZJERyAIIeQlzMzMsH//fqndfNfdcRyHpqYmyMrKorKyEurq6pCVleV7WYSQLkSG7wUQQkhnp6ioSFPIpOTq1auwsbFBfn4%2BAGDHjh2YPHkybt68yfPKCCFdCe0AE0LIS2zbtg0VFRVYuHBhq0lwRLI8PDxgYmKC%2BfPnQ05ODg0NDdi2bRvy8vKwc%2BdOvpdHCOkiqAAmhJCXsLGxQUlJSZvnUAsKCnhYUdc1atQo5ObmtvhaNzY2YvTo0TQJjhAiMXQTHCGEvMS6dev4XkK3oaKigps3b0JfX198raioCL169eJxVYSQroYKYEIIeQlTU9M2r4tEIimvpOtzcHCAv78/5s6dC11dXZSUlCAhIQGOjo58L40Q0oXQEQhCCHmJixcvIioqCuXl5eI%2BwPX19RCJRLh8%2BTLPq%2BtaGhsbERsbi0OHDqGyshJ9%2B/aFo6Mj5s6dS50gCCESQwUwIYS8hLOzM/T09KCqqoqioiJYWlpi9%2B7d8PT0hLe3N9/LI4QQ8v9EBTAhhLzE8OHDcfr0aRQXFyMiIgK7du3ChQsXEBoaigMHDvC9vC6lsbERWVlZbU7do0lwhBBJoTPAhBDyEr169YKSkhL09PRw/fp1AICRkRGEQiHPK%2Bt6QkJCcOTIERgYGEBO7n9/omgSHCFEkqgAJoSQl9DX10dycjJcXV3Ro0cPFBQUQEFBgYoyBrKzs7F7924MHTqU76UQQrowKoAJIeQlAgMD4e/vD0tLS8yZMwczZ86ErKwsXF1d%2BV5al9PU1IT33nuP72UQQro4OgNMCCH/wpMnTyAvLw8ZGRlcvHgRDx48gKWlJd/L6nIiIiKgpaUFHx8fvpdCCOnCaAeYEEJegOM4FBUV4c033xRfKy4uxpQpU3hcVdeVn5%2BPvLw8xMXFtRo7/csvv/C0KkJIV0M7wIQQ0o6HDx/ik08%2BgaamJmJiYgAAVVVVsLa2hqGhIXbs2IEePXrwvMqu5eDBg%2B0%2B5uDgIMWVEEK6MiqACSGkHV9//TUuXLiAjRs3QkNDQ3y9qqoK/v7%2BMDc3R1BQEI8rJIQQ8iqoACaEkHZMnjwZ27dvx4ABA1o9VlBQgMWLFyMrK4uHlXU9zTvsL0J9gAkhkkJngAkhpB1VVVVtFr8AMGTIEFRWVkp5RV3X6dOnX/g4tZwjhEgSFcCEENIOFRUVVFdXQ01NrdVjd%2B/ehbKyMg%2Br6pr27NnD9xIIId2IDN8LIISQzsrc3Bx79%2B5t87GkpCQYGRlJeUWEEEIkgXaACSGkHb6%2BvnB0dER1dTVsbW2hpaWFiooK/PTTT0hJSUFiYiLfSySEEPIK6CY4Qgh5gby8PISEhOD69esQCATgOA7vvPMOVq9eDRMTE76XRwgh5BVQAUwIIf9CUVERRCIRtLS0oKury/dyCCGEdAAVwIQQQgghpFuhm%2BAIIYQQQki3QgUwIYQQQgjpVqgAJoQQQggh3QoVwIQQQgghpFuhApgQQgghhHQrVAATQgghhJBuhQpgQgghhBDSrfwfXlX1bruvswoAAAAASUVORK5CYII%3D" class="center-img">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAsAAAAJ0CAYAAAAcdJWMAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMi4zLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvIxREBQAAIABJREFUeJzs3XlcVOX%2BwPHPsAmCAi6IG4oIKC6Ju6gkarkrLldcMpebSyqYZWl2s3JLDbPc0KuZuXDTRLNSr7fFrSysEDNz30UDRJRN9vn9MTAxisY8B0V%2BfN%2BvF68DZ%2Bb7PM8cZs585zvPOUen1%2Bv1CCGEEEIIUUZYlPQAhBBCCCGEeJwkARZCCCGEEGWKJMBCCCGEEKJMkQRYCCGEEEKUKZIACyGEEEKIMkUSYCGEEEIIUaZIAiyEEEIIIcoUSYCFEEIIIUSZIgmwEEIIIYQoU6xKegBCCPEkOnXqFNu2bePHH38kNjaWjIwMKlWqhKenJ506dWLQoEHY2tqW9DCFEEIo0MmlkIUQwtTSpUsJCwsjNzcXBwcH3NzcsLa2Jj4%2BnuvXrwNQvXp1VqxYQaNGjUp4tEIIIcwlCbAQQhQQERHBzJkzKV%2B%2BPO%2B%2B%2By7PPPMMlpaWxtvPnz/PzJkziY6OxtnZmd27d1OpUqUSHLEQQghzyRxgIYQoYNWqVQC89tprdO/e3ST5BfDw8CAsLIzKlSuTmJjIhg0bSmKYQgghNJAEWAgh8iQlJXHlyhUAnnrqqQfer1KlSnTt2hWA33777bGMTQghRPGRg%2BCEECKPldVfu8R9%2B/bh4%2BPzwPsGBwfz/PPPU7lyZeO6GTNmsGPHDl5//XU6duzIkiVL%2BPnnn8nMzKROnTr079%2BfIUOGUK5cuULb/Pnnn9m4cSNRUVHcvn2bihUr0qxZM0aMGEG7du0KjUlKSuLTTz/lwIEDnDt3jpSUFOzs7HBzcyMgIIDnn38eR0dHkxhvb28AfvjhBxYsWMC3336LhYUFjRo1Yt26dfzrX/9ix44dzJ07l1atWrFs2TJ%2B%2BuknkpOTqVWrFv/4xz8YNWoUOp2O//3vf3zyySecPHmS3NxcGjRowIsvvsjTTz9931jT09OJiIjgm2%2B%2B4fTp0yQlJWFjY0ONGjXo0KEDo0ePplq1aiYxnTt3JiYmht27d5OQkMDatWs5duwYaWlp1KpVix49evDPf/4Te3v7B/6vhBDiXjIHWAghChg6dChRUVHodDr69evHoEGDaN68%2BX1TIQqTnwAPGDCAvXv3kpaWhqenJ9nZ2Vy4cAGAFi1asHr1aipUqGASGxoaypo1awBwdHSkVq1axMXFER8fD8ALL7zAq6%2B%2BahJz6dIlRo0axY0bN7CyssLNzQ07OztiYmK4ffs2AO7u7kRERJgkiPkJcPPmzTl69CheXl7cunWLNm3asHjxYpPHsWfPHrKzs/Hw8CAhIcE4nnHjxqHT6Vi9ejUVK1akdu3aXLx4kbS0NHQ6Hf/%2B97/x9/c39nnr1i1GjhzJmTNn0Ol0uLm5UaFCBWJjY41tVq5cme3bt%2BPq6mqMy0%2BAR48ezfr167GxsaFu3brcuXOHP//8EwBfX182b95cpP%2BREEIAoBdCCGF04sQJfbNmzfReXl7Gn%2BbNm%2BvHjh2rX716tT46Olqfk5NTaOz06dONMQEBAfo//vjDeFtUVJTez89P7%2BXlpX/zzTdN4v7zn//ovby89C1bttTv3LnTuD43N1e/a9cu43i2bt1qEvfcc8/pvby89IMHD9bHxsaaxO3YsUPfoEEDvZeXl37Tpk0mcfljbNy4sf7IkSN6vV6vz8nJ0ScmJt73OIYOHaqPi4sz3mfGjBl6Ly8vfYMGDfTe3t76jz76yLg9bt26pQ8MDNR7eXnpn3vuuUK3zTPPPKO/ePGiyW0HDx7UP/XUU3ovLy/9ggULTG4LCAgwjmXGjBn6pKQk42PctGmT8bavv/660P%2BJEEIURuYACyFEAT4%2BPnz22We0aNHCuC4lJYUDBw6wePFiBg8eTIcOHViyZAl3794ttA0LCwtWrlxJw4YNjet8fX1ZuHAhAJ999hmxsbEAZGZmsmzZMgDmz59P3759jTE6nY6ePXsaK7/Lli0jOzsbgISEBM6ePQvAnDlzcHFxMYkLDAykdevWAJw%2BfbrQcfbo0YNWrVoZx%2Bzk5GRyu5WVFe%2B//z5Vq1Y13mfcuHEA5Obm0q9fP8aMGYOFheGtxNnZmeeffx6AP/74w9hOdnY2v/zyCzqdjtdff526deua9NOxY0d69uwJwJkzZwoda4MGDZg/f76xcq7T6Rg%2BfLixmv3rr78WGieEEIWRBFgIIe5Rv359wsPD%2Bfzzz5k8eTK%2Bvr5YW1sbb09ISGDVqlX07dvX%2BDV8QW3btqVBgwb3re/QoQO1atUiNzeXffv2AXD06FFu3ryJvb09Xbp0KXQ8ffv2xcLCgtjYWGNiWblyZX766SeOHTuGl5fXfTE5OTk4ODgAhrm3hSmY5BfG29vbZDoCQM2aNY2/FzbPNz8RT0lJMa6zsrLim2%2B%2B4dixY3Tq1Om%2BGL1eT/ny5R861k6dOqHT6e5bX69ePQCSk5Mf%2BliEEKIgOQhOCCEeoGHDhjRs2JDg4GDu3r1LVFQU33//PTt37iQhIYErV64wZcoUtmzZYhLXtGnTB7bp7e3NtWvXuHTpEoCxipuVlcXw4cMfGGdpaUlubi4XLlwwad/W1pYbN25w7Ngxrly5wtWrVzl//jwnT54kLS0NMFRrC5Nf2X2Q6tWr37fOxsbG%2BLuzs/N9txc8kPBe5cqVIyEhgejoaC5dusS1a9e4cOECJ0%2Be5M6dOw8da8EKd0H5V%2BPLycl58AMRQoh7SAIshBBFYGdnR/v27Wnfvj1Tpkxh5syZ7Nq1i%2BjoaE6cOGFyRbh7z7pQUH6lMykpCfircpmZmUlUVNTfjiM/DuDChQssWrSIAwcOmCSODg4OtGzZkri4OE6dOvXAtv7uUs52dnYPvT1/6kNRxMfHs3DhQv773/%2BSlZVl0keTJk3Iycl56DSGgol3YfRyPLcQwgySAAshRJ5Zs2bx008/0b9/f1588cUH3s/W1pbZs2fzv//9j6ysLC5evGiSAOdXXguTPzUg//Rp%2BUlmo0aN2L59e5HHmpCQwHPPPUdCQgI1atRg8ODB%2BPj4UK9ePWrVqoVOp%2BOVV155aAL8uGRkZDBy5EjOnz%2BPk5MTQ4cOpXHjxnh4eODm5oalpSVLliyRebxCiMdGEmAhhMiTkZHB5cuX%2Beabbx6aAIOhympvb8/t27fvuxRy/rSGwuQnpPXr1wcMpykDwynNsrOzC51CoNfriYyMxNXVlRo1amBjY0NERAQJCQk4OTkRERFR6OWY8w%2B0K2nffPMN58%2Bfx8rKii1bttx3EBxQ6FxqIYR4VOQgOCGEyJN/Bobff//9b6ux33//Pbdv38bJyem%2Bq8YdPHjQeG7bgvbt28eNGzewsbGhc%2BfOALRq1YoKFSqQmpr6wD6//PJLRo4cSY8ePYyJ4rVr1wCoUaNGocnvuXPniI6OBkp%2Bfmz%2BWO3t7QtNfm/evMn%2B/fuBkh%2BrEKJskARYCCHytG/fnm7dugHwr3/9i3nz5hmTt3wZGRlERETw0ksvATBlypT7rkKWlpbGxIkTuXHjhnFdZGQkr7/%2BOmC4iET%2B6bzKly9vPLXYvHnziIiIMJnP%2B8033/DWW28BhtOWubm5AX%2Bd/eDUqVPs3bvXeH%2B9Xs/Bgwd54YUXjHNtH3S6tsclf6x37tzhk08%2BMZmvGx0dzejRo40X7ijpsQohygaZAiGEEAWEhoZSvnx5Pv/8czZs2MCGDRuoUaMGlStXJiMjg0uXLpGZmYm1tTWvvPIKw4YNu6%2BNunXrcvLkSbp27YqXlxdpaWnGsz707t2b8ePHm9x/7NixXL16la1btzJz5kzee%2B89atWqRWxsLHFxcYDhqm1z5841xgwaNIjw8HAuX75MSEgINWvWxNnZmRs3bpCQkIC1tTWtW7fmyJEjJT4VonPnzvj6%2BnL06FHmz5/PmjVrqFatGvHx8cTGxqLT6fDz8%2BPw4cPExcWh1%2BsLPeWZEEIUF0mAhRCiABsbGxYsWMDw4cPZvXs3kZGRxMbGcurUKezs7HB3d6dDhw4MGjTIWNm8V5MmTQgNDWXp0qX8%2BuuvWFlZ0bp1a4YOHWq84ENBOp2OOXPm0K1bNz799FOio6M5efIk5cqVo1mzZvTu3ZugoCCTMyE4ODiwbds21qxZw759%2B7h27Ro3b97E1dWVTp06MXLkSMqXL0/Xrl05deoU169fp0aNGo9suz2MpaUl69evZ%2BPGjezatYurV69y5swZqlatSs%2BePRk%2BfDiNGjWiTZs23L59m6ioqL89R7EQQmih08u5Y4QQoljMmDGDHTt20KdPH0JDQ0t6OEIIIR5A5gALIYQQQogyRRJgIYQQQghRpkgCLIQQQgghyhRJgIUQQgghRLG5desWzzzzDJGRkQ%2B8z4EDB%2BjTpw/NmjWjR48e7Nu3z%2BT2NWvW4O/vT7NmzRgxYgQXLlwo1jFKAiyEEMVkwYIFnD59Wg6AE0KUWb/%2B%2BitBQUFcuXLlgfe5dOkSwcHBTJkyhV9%2B%2BYXg4GBeeukl4ykbd%2BzYwcaNG/noo4%2BIjIykUaNGhISEUJznbZAEWAghhBBCaLZjxw6mTZvG1KlT//Z%2BLVu2pGvXrlhZWdGzZ09atWrFli1bANi6dSvDhg3D09OTcuXK8corr3D9%2BvWHVpTNJQmwEEIIIUQZFxcXx4kTJ0x%2B8i/EU1QdOnTg66%2B/LvR85wWdO3cOLy8vk3X169fn1KlThd5ubW1N3bp1jbcXB7kQhnjyqV4Ryt0dzp4FT0%2B4eNHs8Pg4ta9aLCygUiW4dQsKXNHWLBkZ5sdYWoKrK/z5J%2BTkqPWrehVaKyuoWxcuXYLsbPPjnZzU%2BtW6rf/4Q61fW1to3RqOHIH0dLU26tY1P8bKCmrUgOvX1bYzQMWKanEWFobYpCS1bW2l%2BG6j04G9PaSmguq3nyr/o%2BJ4HavEWVpC5cqQkKD%2BOv75Z7W48uUhIAD27YO0NPPjeze5rNZxcTyxHRzMj7GwMOx8bt9W/ydXrqwWp9UjuFLilqVLWb58ucm6yZMnExwcXOQ2qlatWqT7paamYmdnZ7LO1taWtLwn3t/dXhwkARb/fzk5Gd5NVLMrRRYWhn2ThYX6PlVrv6pvnKosLQ19W1qqv3%2BpKKltbWVl6Fc1qVNV8PE%2BbjrdXz8l1e/jvGxTST23Smo7Q8k9r0vsiV2SG/sJFBQUROfOnU3WFTWhNZednR3p93wyTU9Px97evki3FwdJgIUQQgghSpNH8GHBxcUFFxeXYm%2B3MF5eXpw4ccJk3blz52jcuDEAnp6enD17loCAAACysrK4dOnSfdMmtJA5wEIIIYQQ4rHp27cvR44cYffu3WRnZ7N7926OHDlCv379ABg4cCCbNm3i1KlTZGRksHjxYqpUqULLli2LbQxSARZCCCGEKE1KYh6URr6%2Bvrzzzjv07dsXDw8PVqxYQWhoKG%2B88QY1a9Zk2bJluLu7AzBo0CCSk5OZNGkSt27dokmTJqxevRpra%2BtiG48kwEIIIYQQpUkpSIBPnz5t8vfRo0dN/u7YsSMdO3YsNFan0zFmzBjGjBnzyMb35G9BIYQQQgghipFUgIUQQgghSpNSUAF%2B0skWFEIIIYQQZYpUgIUQQgghShOpAGsmCbAQQgghRGkiCbBmkgALJSNGjKB169b079%2BfLl26YGdnh06nQ6/XY2VlhY%2BPDyEhIcV6zj4hhBBCiOIgCbAoFl999RW1atUCIDk5mY0bNzJ69Gg%2B/vhjSYKFEEKI4iQVYM1kC4piV6FCBSZOnMizzz5LaGhoSQ9HCCGEEMKEVIDFIxMQEMC0adO4e/cudnZ2RYqJi4sjPj7eZF1Vd3dcnJzMH0CDBqZLM1kpvjosLU2XKnJzzY/JH6/quFX7Bci/OI/qRXpKals7OKjF5T%2Bdi/i0LpSNjfkxxfE/Vt1W%2BQUn1cKT1jgtBS%2BV7VVSr%2BPi6LdiRbW4/NeD6utC6UkNJffE1rqxc3LU4oqDVIA1kwRYPDLOzs7o9XqSkpKKnABv2bKF5cuXm6ybPGUKwVOmqA8kPFwpzFm9R0D9TUirypVLpl%2BA6tVLpl/Vbd2ihbZ%2BfXy0xauqWrVk%2BgUNyZFGWj5s2Nurx5bU61jlM38%2Bf39tfTdvrhqpcQdQUk/sChXU4hISincc5pAEWDNJgMUjk5CQgKWlJY6OjkWOCQoKonPnzibrqvbpA598Yv4AGjQwJL/DhsGpU2aHJ34bZX6fGIoJFStCUpJ6gSAz0/wYKytD8puQANnZav1mZKjFWVsbkt8bNyAry/x41fcfrdv6wgW1fu3sDMnvH3/A3btqbdSoYX6MlZUhR4iPV/8fqyaDFhaG5DclRVtlU6VfOzvDdlb9hkLl9VQcr2PV7eTkBLdvq/d74oRanIODIfmNijL8n83l73lDrePieGKrfEKytDTsfJKTS7aaK0qEJMDikdm3bx/NmzfH1ta2yDEuLi64uLiYrrx4UdtATp2Ce65BXhSq%2B%2BF8OTnqbagkkfmys9XjVRPgfFlZam1oqe6B%2BrZWeZMv6O5d9TZUkrJ82dnq8Wa8HAuVm6uWK%2Bh02vtVTYC1vJa1vI5Vx6u136Qk9X7B8JxWakPLkxq0PbFVp1%2BAYWOXtgRYKsCaSQIsit2dO3fYuHEj%2B/btY/369SU9HCGEEOL/F0mANZMEWBSL3r17o8sr8djb29OsWTM2bdpE48aNS3hkQgghhBCmJAEWSjZu3Gj8/fTp0yU4EiGEEKKMkQqwZrIFhRBCCCFEmSIVYCGEEEKI0kQqwJpJAiyEEEIIUZpIAqyZbEEhhBBCCFGmSAVYCCGEEKI0kQqwZrIFhRBCCCFEmSIVYCGEEEKI0kQqwJpJAiyEEEIIUZpIAqyZJMDiiRcfp1eKs7ICZyDx2yiys82Pr%2BqiU%2BoXX1%2BIisK5S3M4elStDWdn82OaNoX9%2B6kW1Al%2B%2B02p298P3lKKy7sIIJmZkJFhfvylS0rdUr68YVNdvw5paebHV6qk1q%2BtrWFZsSLY2Ki1ofIvcnSE6tXh7Fm4c0etX1WOjuDvD9HRan2r/H/A8P999ln44QdITFRrI6h3qvlBFhaAHc62dyE3V63juDjzY2xsgJpUTo8xvKAU9OjhrhSXr317tbiv9tRRiqtYEfyrw8Gz1UlKUuvb0lKt3/bt4YcTTsr99uihFidKniTAQgghhBCliVSANZMtKIQQQgghyhSpAAshhBBClCZSAdZMEmAhhBBCiNJEEmDNZAsKIYQQQogyRSrAQgghhBCliVSANZMtKIQQQgghyhSpAAshhBBClCZSAdZMEmAhhBBCiNJEEmDNZAsKIYQQQogyRSrAQgghhBCliVSANZMtCGzevBlvb2/Wr1//2PpctmwZ3t7evPzyy/fdlpmZSdu2bfH29gbg%2BvXr%2BPr6cv369cc2PiGEEEKI/68kAcaQAA8dOpQNGzaQnZ392Pp1dnbmm2%2B%2BITk52WT9d999R1ZWlvHvGjVqcPToUWrUqPHYxiaEEEKIJ5SFRfH/lDFl7xHf48cffyQhIYEZM2aQm5vL3r17jbclJiYydepUWrRoQZcuXdi4cSM%2BPj5cu3YNgCtXrjBhwgTatGlDQEAAS5YsITMzs8h9e3p64u7uzu7du03WR0RE0KtXL%2BPf165dw9vb29ivt7c3GzdupFu3bvj6%2BjJkyBBOnz4NQGRkpLFynG/GjBnMmDEDMFSep0yZwvTp02nevDn%2B/v7s2bOHFStW4OfnR%2BvWrVm5cqUx1tvbm8jISOPf27dvp3Pnzsa%2BOnfuzNq1a2nfvj0tWrTg/fff59tvvzWOLTg42KxtIoQQQoi/IQmwZmV%2BDvDGjRsZPHgwtra2DBs2jHXr1hmTz2nTpqHT6fj222/Jzc1l2rRp5OTkAJCWlsaoUaPo1asXH374Ibdu3SIkJITc3FxeeeWVIvffv39/duzYQVBQEACxsbEcP36cESNGsGXLlgfG7dq1i02bNmFra0tISAiLFi3io48%2BKlKfe/fu5YMPPmDBggUsXryYV155hZEjR3LgwAEOHDjApEmT6NevHzVr1vzbtmJiYoiPj2f//v0cPnyYcePG0b59e7Zu3UpSUhIDBw5k9%2B7dBAYGFmlscXFxxMfHm6yzsqqKi4tLkeILsrQ0XZrN11ctrkED06WKihXNj/H0NF0qsLVViytXznRpLp1OLS5/vKrjtrFRi9P6eAEcHc2PcXAwXT5OWvtW3VYVKpgulai8uec/KXU69eRA5QlmbW26LEVUdltQPM9rlf28vb3p0lxJSWpx4slQphPgmJgYDh06xKxZswAYPHgwK1as4MiRI9SpU4fvv/%2BePXv24OTkBMDMmTONyfH%2B/fvJzMzk5ZdfRqfTUb16daZMmUJISIhZCXDfvn0JDQ3l4sWLuLu7s337dnr27Em5v3m3GDFiBFWrVgWgR48erF69ush91q9fn%2B7duwPQvn171qxZw4QJE7C2tjZWd69fv16kBBhg/PjxWFtb06FDBwCGDh2Ko6Mjjo6OeHp6GivXRbFlyxaWL19usm7SpMmEhAQXuY17qe6UiYpS7hOA8HBt8arWrFEOra%2Bx69q1NTagyMOjZPp1c1OP1fA5hebN1WO1Kqm%2B27XTEm2nHqr66QqgiPvQQil86M%2Bn%2BpnfGK/YgL%2B/tn5L6rnVrJla3J49xTsOs5TBim1xK9MJcHh4ONnZ2fTr18%2B4Ljs7m3Xr1jFhwgQAatWqZbytdoF395iYGG7dukWrVq2M6/R6PVlZWSQkJFC5cuUijaFSpUp06tSJzz//nKlTp7Jjxw4%2B%2BOCD%2B%2BYF36tKlSrG362srNDr9UXqDzAm9AAWeS8ix7xyVP7fubm5RW7P2dkZAMu8vWbFAhmnhYWFWWMLCgoyJuH5rKyqkphY5CaMLC0NyW9SEuQV7s3i3EVxb9yggSH5HTYMTp1Sa0O1ArxmDYwdC2fPKnV7bu1%2Bpbhy5QzJ79WrkJFhfrxKDBhyEw8POH8e0tPNj9dSAXZzgytX1Md%2B44b5MQ4OhiQhKgpSUtT6VaW1b5X/Dxgqv%2B3awY8/wt/sFh/o2Y53zQ/S6QxPsPR0MGMfZuLWLfNjrK0NyW9cHBQ4FsQcOa7qibelpdr%2BEuCHH9TiiuN5rVoBbtYMoqMhNVWtX1F6ldkEOCMjg23btjFv3jz8/PyM68%2BcOcO4ceMYP348YEh03d3djb/nc3V1xc3Njf/%2B97/GdSkpKSQkJFCpUiWzxtK/f3/mzJmDn58f9vb2%2BPj4mMy7NUd%2BEpqZmYlN3rt7YmKiMUkF0JnxfbOFhYXJAXmJhWSi5rT3d1xcXO6b7hAfD1qOTczJUYw/elS9UzAkv6ptFPh/me3sWfjtN6VQ1SQlX0aGWht3FfKTgtLTIS3N/DgzPucVSvXxAty5o95vSoq2eC1U%2B1b5/xSUnIzSB2FA7R%2BdX2HT69WfKFqOf8jK0hZfArROCUhJUW9DeaobhuS31E1nkAqwZmV2C3755ZfodDr69OmDq6ur8cff3x8vLy%2B2b99OQEAA7733Hnfu3OHOnTssWrTIGB8QEEBqaipr164lMzOTpKQkpk%2BfztSpU81OCJ9%2B%2BmmysrKYO3cugwYN0vS43NzcsLKyYteuXQAcPnyYn376Sbk9Dw8P9u7dS3Z2NleuXGHbtm2axieEEEIIjeQgOM3K3iPOEx4eTp8%2BfbAu5ECDoKAgdu7cybx589DpdHTq1In%2B/fvj4%2BMDgLW1NQ4ODqxfv57IyEj8/f3p2rUrFhYWhIWFmT0WKysr%2Bvbty%2BXLl%2Bndu7emx%2BXi4sLMmTNZuXIlzZs3Z9OmTQwYMEC5vbfeeosTJ07QunVrXnrpJc0JuhBCCCH%2Bf0pISGDixIm0bNmSNm3aMG/evEJPL/vCCy/g6%2Btr8uPt7W08JuvmzZt4e3ub3H7v9EitdHpzJmiWMT/88AMtWrTANu8giNOnTxMYGEh0dPTfHqQmis89J4UoMisrw0yCxES1KRBVXRSndvj6GiazNW/%2BeKdANG0K%2B/dDp07KUyB%2BP6gwZxHDVMn69eHcucc7BaJ8eWjUCE6cUPuKXfX4Jltbw5Trs2fVp0BcumR%2BjKOj4UCjgwcf/xQIrX2rToFwdoZnn4X//U99CkRQb4UJnhYWYGdneHKqToGIizM/xsbGcPBcTIzyFIgcN3elONA2B1j1oLCKFf96bj3OKRAVK0L79oa5y6r99uihFqdZx47F3%2BahQ5qbGDFiBNWqVWPOnDncvHmTF198kcDAQF544YWHxm3bto3ly5ezdetWXFxc2LdvH3PmzOG7777TPKYHKbMV4KJYuHAhYWFhZGdnk5KSQlhYGH5%2BfpL8CiGEEEIUcPnyZY4cOcKrr76KnZ0dtWvXZuLEiWzevPmhcRcuXGDOnDmEhoYajwE6fvw4jRs3fqTjLbMHwRXF4sWLmTt3Lm3btsXCwoKOHTuazAN%2BkI8//pilS5c%2B8PY%2Bffowe/bs4hyqEEIIIcqKRzBnt7Dz8FetWvTz8J89exYnJyeqVatmXOfh4cH169dJSkoyOUNUQe%2B88w6BgYG0bNnSuO748ePcuXOH3r17c/PmTZo0acL06dOpX1/ryTr/IgnwQ3h6evLJJ5%2BYHTd69GhGjx79CEYkhBBCiDLvESTAhZ2Hf/LkyQQHF%2B08/KmpqdjZmZ5zO//vtLS0QhPgX375hWPHjhEaGmqyvmLFitSvX5%2BxY8diY2PDhx9%2ByOjRo9m9ezcVNF0V5y%2BSAAshhBBClHGFnYc//4JbRVG%2BfHnu3nNAR/7f9g%2B43N6WLVvo0aPHff0sXrzY5O/XX3%2BdiIgIfvnlFwICAoo8poeRBFgIIYQQojR5BBXgws7Dbw5PT09u377NzZs3jRfrOn/%2BPK6uroVWbbOzs/n2229ZsWKFyfqUlBRWrFjBc889Z7wibU5ODtnZ2caTEhQHOQhOCCGEEEJoUrduXVq0aMH8%2BfNJSUnh6tWrrFy58oGnTz19%2BjQZGRk0v%2Bca2A4ODhw%2BfJiFCxeSnJxMamoqc%2BbMoVatWibzhLWSBFgIIYQQojSEp9WGAAAgAElEQVR5Qi%2BEsXTpUrKzs%2BnSpQuDBw%2BmY8eOTJw4EQBfX1%2B%2B%2BOIL432vXr2Ko6NjoWfWWrlyJbm5uXTt2pWOHTsSHx/PmjVrCr12gyqZAiGEEEIIUZo8oVduq1KlygPPgnX0nvPid%2B/ene7duxd635o1a953QF5xezK3oBBCCCGEEI%2BIVIDFEy8jQy0u/%2BJNmZmQlaXQgMrV2MBweaH8pWobKpe8yr%2BUUVKS8iWzVLd1fjEiM1OtDdV%2Bray09VuMx1OYTeXiYvkxubnqFyd7wKk4/1b%2B2Y3s7NSuFJaSotZvcTxm4xPFHLq8K0FaWqpX21Ti8mM0fC2tcuVLMDzk/CvBlbZrxKr8i/OvHmdpqRZfop7QCnBpUtr%2B5UIIIYQQZZskwJrJFhRCCCGEEGWKVICFEEIIIUoTqQBrJltQCCGEEEKUKVIBFkIIIYQoTaQCrJkkwEIIIYQQpYkkwJrJFhRCCCGEEGWKVICFEEIIIUoTqQBrJltQCCGEEEKUKVIBFkIIIYQoTaQCrJlsQWDz5s14e3uzfv36x9bnsmXL8Pb25uWXX77vtszMTNq2bYu3t7fmfr744gt69eqluR0hhBBCPCHyL5VdnD9lTNl7xIXYvHkzQ4cOZcOGDWSrXkRdgbOzM9988w3Jyckm67/77juysrKKpY%2B%2Bffuya9euYmlLCCGEEOL/gzKfAP/4448kJCQwY8YMcnNz2bt3r/G2xMREpk6dSosWLejSpQsbN27Ex8eHa9euAXDlyhUmTJhAmzZtCAgIYMmSJWRmZha5b09PT9zd3dm9e7fJ%2BoiIiPuqtt999x1DhgyhXbt2PPXUUzz33HNcunQJgFmzZtG1a1dSU1MBQ0Lftm1bYmNj2b59O507dwYgMjKSzp07s3btWtq3b0%2BLFi14//33%2Bfbbb%2BnWrRu%2Bvr4EBwcbH8OIESNYtmyZcQzXrl3D29vb%2BPi9vb3ZsmUL3bp146mnnmLChAn8/vvvDBkyBF9fXwYOHMjly5eLvD2EEEIIUQRSAdaszM8B3rhxI4MHD8bW1pZhw4axbt06Y/I5bdo0dDod3377Lbm5uUybNo2cnBwA0tLSGDVqFL169eLDDz/k1q1bhISEkJubyyuvvFLk/vv378%2BOHTsICgoCIDY2luPHjzNixAi2bNkCwJ9//smUKVP48MMP6dy5M4mJiUyePJkVK1bw3nvvMXPmTAYNGsR7773HkCFDWLRoEcuWLaNatWr39RcTE0N8fDz79%2B/n8OHDjBs3jvbt27N161aSkpIYOHAgu3fvJjAwsEjj//LLL9myZQuZmZn06tWLiRMn8vHHH1O9enX%2B%2Bc9/smrVKt59990ib4%2B4uDji4%2BNN1un1Vala1aXIbeSzsjJdmq1pU7U4T0/TpYqkJPNjGjQwXSqws1OLs7U1XZpLdd%2BbP16t4zZXuXKmSxWOjubHODiYLlXY26vFlS9vujSXs7NaXMWKpkslOp16jEpsPhsb82M077jUh6z1Iav%2Bj4rjea2yqbU%2Bp%2B/58laUMmU6AY6JieHQoUPMmjULgMGDB7NixQqOHDlCnTp1%2BP7779mzZw9OTk4AzJw505gc79%2B/n8zMTF5%2B%2BWV0Oh3Vq1dnypQphISEmJUA9%2B3bl9DQUC5evIi7uzvbt2%2BnZ8%2BelCvwzlqpUiV27dqFm5sbKSkp/Pnnnzg7OxMbGwuAra0t77//PoMHD2b//v2MGjUKf3//B/Y5fvx4rK2t6dChAwBDhw7F0dERR0dHPD09jRXeonjuueeM28fT0xMfHx88PDwAaNu2Lb/%2B%2BmuR2wLYsmULy5cvN1k3adJkQkKCzWqnoMqVFQP371fuE4A1a7TFqwoPVw710dh1vXoaG1CkIefXxM1NPVbL56OWLdVjtWrUqGT69fPTEq2QHeWztlaPrV5dPbZqVeVQDY8WUH/ID3nbKZLmzbXFq1KtdXz9dfGOwyxlsGJb3Mp0AhweHk52djb9%2BvUzrsvOzmbdunVMmDABgFq1ahlvq127tvH3mJgYbt26RatWrYzr9Ho9WVlZJCQkULmIWVelSpXo1KkTn3/%2BOVOnTmXHjh188MEHJvOCra2t%2Beqrr/j000/R6XR4eXmRkpKCVYEKgZeXF61ateL7779n4MCBD%2B3TOa8UY2lpCUDFAh/bLSws0Ov1RRo7YEx%2B89tzLFDWMrctgKCgIOOUjXx6fVXycn2zWFkZkt%2BEBFCZ2l0tqJP5QWDIbNasgbFj4exZtTZUK8Dh4TBsGJw6pdTtH5uilOJsbQ3J74ULkJ5ufnxGhlK32NkZHvapU3D3rvnxqhWncuUMye%2BVK%2Bpjj4kxP8bBwZD8/vILpKSo9aulAtyoEZw4AWlp5scnJKj1W7GiIfk9fFjtZQHQvXPRp6YZ6XSGTDArC8zcjxmpPGgrK0PyGx%2BvtuMCMiurJd5aH/JPPyl1i4ODIfmNilJ/XqtWgJs2hd9%2BU3tOlyhJgDUrswlwRkYG27ZtY968efgVKC2cOXOGcePGMX78eMCQ6Lq7uxt/z%2Bfq6oqbmxv//e9/jetSUlJISEigUqVKZo2lf//%2BzJkzBz8/P%2Bzt7fHx8SEyMtJ4%2B549e9i0aRP/%2Bc9/qFOnDgBz5szhzJkzxvvs3r2bY8eO8cwzz/Daa6%2BxefNmY4J7L10Rv9%2BysLAwORgvMTFRua2icnFxwcXFdLrDtWuGHbKq7GzF%2BN9%2BU%2B8UDMmvahuFbOsiO3UKjh5VClVJIgtKT1drQ2u/d%2B9C3hR4s2j4lhkwJL8qCT/AnTvq/aakqMer5nL50tLUkhQtT2kwJL/KbWh50Hq9erwZx4TcJztbOV7r/1j1Iat%2BQMmXkqLehpbpSGlpMp2hLCqzHyG%2B/PJLdDodffr0wdXV1fjj7%2B%2BPl5cX27dvJyAggPfee487d%2B5w584dFi1aZIwPCAggNTWVtWvXkpmZSVJSEtOnT2fq1KlmJ4VPP/00WVlZzJ07l0GDBt13e3JyMhYWFtja2qLX6zl48CCff/65MTmNiYnhrbfe4s0332T%2B/PnExcXdN41AhYeHB4cOHSIpKYnk5GTWlNRX%2BkIIIYT4ixwEp1nZe8R5wsPD6dOnD9aFTHYKCgpi586dzJs3D51OR6dOnejfvz8%2BPoYZktbW1jg4OLB%2B/XoiIyPx9/ena9euWFhYEBYWZvZYrKys6Nu3L5cvX6Z379733d6/f3/8/Pzo1asXbdu2JSwsjJEjR3Lx4kUyMzOZNm0a7dq1o0%2BfPjg4ODB//nz%2B/e9/8/PPP5u/YQoYP348lStXpkuXLvTr1%2B%2B%2BqQlCCCGEEKWRTm/uJM0y5IcffqBFixbY5h0qfvr0aQIDA4mOjjY5SE08WmYck2fC2hqqVYPYWLUpELWamjeVxahpU8MBdJ06Pd4pEL6%2Bhkl0zZsrT4H49Re13YGdHfj4wB9/PN4pEPb2hod99KjaFAiVMzGAYc6zp6dhlovqFIgLF8yPcXQ0PK3271efAqHlSP1WreDnn9WmQPz5p1q/zs7QvTv897/qUyCGDlCYqK3TGSaWZmaqzylQedA2NoaD527cUJ4CkeFaRylO60NWPSisYkXDAXQHDz7eKRAVKkDbtoa5y6pTIJ55Ri1Os9Gji7/Njz8u/jafYGW2AlwUCxcuJCwsjOzsbFJSUggLC8PPz0%2BSXyGEEEKUHJkCoVmZPQiuKBYvXszcuXNp27YtFhYWdOzY0WQe8IN8/PHHLF269IG39%2BnTh9mzZxfnUIUQQgghRBFJAvwQnp6efPLJJ2bHjR49mtGP4usJIYQQQogyWLEtbrIFhRBCCCFEmSIVYCGEEEKI0kQqwJpJAiyEEEIIUZpIAqyZbEEhhBBCCFGmSAVYCCGEEKI0kQqwZrIFhRBCCCFEmSJXghNPvLNn1eLKlQM3N7hyBTIULgSlEgOGq4TVrw/nzqlfJUylb61XYwNo0VKnFqj1KnQNG6r127AhRETAwIFw8qT58SqXCATDht65E/r1M2xwFZUUrjTo7Q0bNsDzz8Pp02r9OjmpxdWvDytWwKRJhie3uVS3tacnrF4N48er7wzWrDE/xsYGateGq1eVr8iGlcKXrMVwJTjS0tTibG3B3R0uXlTbefXtq9ZvcbyeqlY1P8bLC9avh1Gj4MwZtX4PH1aL0yo4uPjbXLas%2BNt8gskUCCGEEEKI0kSmQGgmW1AIIYQQQpQpUgEWQgghhChNpAKsmWxBIYQQQghRpkgFWAghhBCiNJEKsGaSAAshhBBClCaSAGsmW1AIIYQQQpQpUgEWQgghhChNpAKsmWxBIYQQQghRpkgFWAghhBCiNJEKsGaSAAshhBBClCaSAGtWJrfgxYsXmT59Ov7%2B/vj6%2BtK1a1dCQ0NJTU0tlvZXrVrFCy%2B8UCxtlSRvb28iIyNLehhCCCGEKAUSEhKYOHEiLVu2pE2bNsybN4/s7OxC7/vCCy/QpEkTfH19jT8HDx4EICcnh4ULF%2BLn54evry8vvvgicXFxxTrWMpcAR0VF0b9/f2rWrMnnn3/O0aNHWbNmDceOHWPMmDHk5ORo7mPChAmsXbu2GEYrhBBCCHEPC4vi/ykGL730EuXLl%2BfQoUNs27aNH3/8kfXr1xd6399//52PPvqIo0ePGn/8/f0BCAsL44cffiAiIoJDhw5ha2vLv/71r2IZY74ylwDPmjWLwMBAQkJCqFSpEgDu7u4sWbKEypUrc/XqVcCQKD///PN06NCBJk2aMGDAAKKjowGIjIykc%2BfOhIWF0bFjR1q3bk1wcDApKSkALFu2jBEjRgCwfft2hg4dyty5c2nbti3t2rXjjTfeICsrCwC9Xs%2BGDRvo1q0bLVu2ZNiwYfz%2B%2B%2B8PHL%2B3tzcbN26kW7du%2BPr6MmTIEE6fPm0cl7e3t8n9Z8yYwYwZM4zjmjJlCtOnT6d58%2Bb4%2B/uzZ88eVqxYgZ%2BfH61bt2blypUm8d9//z09evSgTZs2hISEEB8fb7ztxIkTjBgxglatWvHss8%2Byfv169Hq9sa8xY8YwcOBAWrduzc8//6zw3xJCCCFEaXD58mWOHDnCq6%2B%2Bip2dHbVr12bixIls3rz5vvtevXqVO3fu4OPjU2hbn332GWPHjqV69eo4ODjwxhtvcPDgQWOOVhzK1BzgK1eucPbsWd5%2B%2B%2B37bqtSpYox%2BUtPT%2BfFF18kJCSEoUOHkp6ezsyZM1m0aBHh4eEAxMTEEBsby9dff01sbCzDhw8nPDyccePG3dd2VFQU/v7%2BHDp0iJMnTzJy5Ej8/Pzo1asX4eHhfPzxx4SFheHh4cHOnTsZPXo0e/bsoUqVKoU%2Bjl27drFp0yZsbW0JCQlh0aJFfPTRR0XaBnv37uWDDz5gwYIFLF68mFdeeYWRI0dy4MABDhw4wKRJk%2BjXrx81a9YE4MCBA6xduxZHR0deffVVpk2bxieffEJsbCwjR45k6tSprFu3jsuXLzNx4kRsbW0ZMmQIAD/%2B%2BCPr1q2jadOmlCtXrkjji4uLM0myAe7erYqLi0uR4guytjZdmkunU4vLf6hFfMiFUvkwbmtrulTi66sW16CB6dJc7u7a4lTjH/DV3N%2BqV890qcLR0fyYOnVMlyoqVFCLq13bdGku1W2ttV8AGxvzY7TuQACsFN5i82NUYvPl5qrF5W8nle0F8IBk5m8Vx%2BvJ2dn8GK2vpzNn1OKKwyOYA1zY%2B2/VqkV//z179ixOTk5Uq1bNuM7Dw4Pr16%2BTlJRExYoVjeuPHz%2BOvb09U6dO5fjx41SpUoVRo0YxaNAgkpOT%2BfPPP/Hy8jLev0qVKjg6OnL69Glqa9kXFFCmEuBbt24BPDCxzGdtbc2WLVuoU6cOGRkZxMTE4OTkxPHjx03uN2nSJGxtbalTpw5t2rTh4sWLhbZna2vLhAkT0Ol0NG3aFG9vb%2BN9N2/ezPjx42mQlzgMGjSIbdu28cUXXzBmzJhC2xsxYgRVq1YFoEePHqxevbrI26B%2B/fp0794dgPbt27NmzRomTJiAtbU1nTt3BuD69evGBDgkJMT4%2B2uvvUb37t2JjY3liy%2B%2BwMPDg%2BHDhxvb/ec//8mmTZuMCXDt2rVp165dkccGsGXLFpYvX26ybtKkyYSEBJvVTkHVqyuHalJMr1GzaXkPISpKW%2Bd5HxAfu9DQkul3yZKS6XfOnJLpFyDvG6XHrpi//iwyV9eS6TdvH18i8vb5Ztu5U1u/JfV6eucdtTg/v%2BIdhzkeQQJc2Pvv5MmTCQ4u2vtvamoqdnZ2Juvy/05LSzNJgDMzM2nWrBlTp07F09OTyMhIgoODsbe3xzevEFO%2BfHmTtmxtbYvtWC0oYwlwftIYHx9P3bp177v95s2bVKlSBUtLSyIjIxk7dixpaWnUr18fKysr49f797YHhqT53tvzVa5cGV2BcmLB%2B8bExLBw4UJCC7yBZ2dn07hx4wc%2BjoIJfGHjehgnJyfj7xZ5LyDHvEpU/t%2B5BaoHtWrVMv5eo0YNAGJjY4mJieHEiRO0bNnSeHtubi6WlpbGv1WqtkFBQcZEPN/du1W5csXsprC2NiS/N25A3owTs2Rmmh8Dhspv7dpw9SpkZKi1odK3ra0h%2Bb1wAdLT1fr1ea65WmCDBobkd9gwOHXK/HgtFeDQUJg2DR7wAfShtFSAlyyBqVMNG1yFagV4zhx48024fFmtXy0V4BkzYMECw5PbXFoqwP/6F8ydq9YvqCXP1taG5PfPP9V2IKBeAa5aFeLj1beZ6g7AxsaQ/MbEqO2EXnpJrd/ieD2pVoDfeQfeekv99fT/SGHvv1XN%2BCBWvnx57t69a7Iu/297e3uT9YGBgQQGBhr/7tChA4GBgezZswe/vA8W97aVnp5%2BXztalKkEuGbNmnh5ebF7925atWplcltCQgIBAQG8%2B%2B671K5dmzlz5vDpp58aE9F169Y9sMKrhaurKyEhIfTq1cu47sqVKyaJalHlJ5%2BZmZnY5H2FlZiYiHOBHYPOzO/14%2BLijNXp/Lk3tWrVwtXVlTZt2phMvUhMTDT5dGZuX2BImu9NnM%2BeVU8kwfDepRKvpc/8eNX3IS19p6fDPfuNojt6VL1jMCS/Km2obqh8Fy/CyZPmx6kmNvkuXIA//lCLzTsGQcnly5A3999sCvsWE1evwrlz5sdp3dZXrxp2BipUP82CYdyq8apTEcCQ/Kr2q/X1lJmp1obqayGflteTlor55cslO51BxSOoABf2/msOT09Pbt%2B%2BbSwmApw/fx5XV1cq3PPBe9u2bdjb29OjRw/juszMTMqVK4ejoyPVqlXj3LlzxmkQ8fHx3L5922RahFZl7iC4N998k4iICJYvX05iYiJ6vZ6TJ08yYcIEGjVqRLdu3UhOTsbCwgLbvMmU0dHRbNiwgUwtO9EHGDx4MGFhYZw/fx6AQ4cO0atXL6WDxtzc3LCysmLXrl0AHD58mJ9%2B%2BknT%2BJYtW0ZsbCx37txhwYIFPPvss1SqVIk%2BffoQHR3NF198QXZ2NnFxcUyYMIEFCxZo6k8IIYQQf%2BMJPAtE3bp1adGiBfPnzyclJYWrV6%2BycuVKBg0adN99U1JSmDNnDn/88Qe5ubns37%2Bfr776iqCgIAAGDBhAWFgYV69eJSUlhfnz59O6dWvc3Nw0jzNfmaoAA7Ru3ZpNmzaxatUqevXqxd27d6lSpQrdu3dn/PjxWFtb0759e4YNG8bw4cPJzc2lVq1ajBgxgsWLF3Pz5s1iHc%2BoUaPQ6/VMnDiRuLg4qlWrxqxZs%2BjSpYvZbbm4uDBz5kxWrlzJnDlzaNu2LQMGDLjvawRzdOzYkcGDB5Oenk5AQAAzZ84EDNX0tWvXEhoayty5c7G0tKRTp0688cYbyn0JIYQQovRaunQps2fPpkuXLlhYWBAYGMjEiRMB8PX15Z133qFv376MHDmStLQ0Jk%2BeTEJCArVr12bhwoXGaZWTJk0iOzub4cOHk5qaSps2bfjggw%2BKdaw6vTkTSIUoAarfepYrB25ucOXK450CYWsL9esbviV%2BnFMg7OwMB2H/8Yf6FIgWLRVPfeHraziArnlztSkQDRuq9duwIUREwMCBj3cKhI%2BP4YCffv0e7xQIb2/YsAGef/7xT4GoXx9WrIBJkx7vFAhPT1i9GsaPV98ZrFljfoyNzV%2BT%2BVW//VOZA2xj89fBC6r9pqWpxdnaGubVX7yotvPq21et3%2BJ4PalMgfDygvXrYdQo9SkQhw%2BrxWn17rvF3%2Bbrrxd/m0%2BwMjcFQgghhBBClG1lbgqEEEIIIUSp9ggOgitrJAEWQgghhChNJAHWTLagEEIIIYQoU6QCLIQQQghRmkgFWDPZgkIIIYQQokyRCrAQQgghRGkiFWDNJAEWQgghhChNJAHWTBJg8cRTPW9//jnoK1QwXCTCXJcuqfWry7uWREaG%2BgUpVC6Ekb8/1NKv8gUp3N3/WqqcQF/lIhZgOHE/GE7cr9JG/fpq/RYHlTew/CeXTqf%2BBpicrBaXmvrXUqUN1SdlUtJfy8REtTays82PsbQ0LHNy1OJV%2B82/NlVGhvrVeFSfG1qfXzk5av3m5v61VG3jxg3zY/IvRnPzplq8KNUkARZCCCGEKE2kAqyZbEEhhBBCCFGmSAVYCCGEEKI0kQqwZpIACyGEEEKUJpIAayZbUAghhBBClClSARZCCCGEKE2kAqyZbEEhhBBCCFGmSAVYCCGEEKI0kQqwZpIACyGEEEKUJpIAayZbUAghhBBClClSAS5jMjIySExMxNXVtaSHIoQQQggVUgHW7IneghcvXmT69On4%2B/vj6%2BtL165dCQ0NJTX/mvQarVq1ihdeeKFY2rqXt7c3kZGRj6RtLYYNG8bhw4dLehhCCCGEECXmiU2Ao6Ki6N%2B/PzVr1uTzzz/n6NGjrFmzhmPHjjFmzBhycnI09zFhwgTWrl1bDKMtPRITE0t6CEIIIYTQwsKi%2BH/KmCf2Ec%2BaNYvAwEBCQkKoVKkSAO7u7ixZsoTKlStz9epVwJAoP//883To0IEmTZowYMAAoqOjAYiMjKRz586EhYXRsWNHWrduTXBwMCkpKQAsW7aMESNGALB9%2B3aGDh3K3Llzadu2Le3ateONN94gKysLAL1ez4YNG%2BjWrRstW7Zk2LBh/P7770V6LOfPn2f8%2BPF06tSJpk2b0rNnT/bt2wfAtWvX8Pb2ZsGCBbRq1Yp33nkHgA0bNhAQEECbNm2YOnUqwcHBLFu2DIDMzEw%2B/PBDunTpQuvWrRk7diyXL1829hceHk7Xrl1p2bIlffr04bPPPgNgzJgxXL9%2BnbfeeovZs2ffN069Xs%2B///1v%2BvTpQ8uWLWnVqhWvvPIK6enpAOTk5PDBBx/Qvn17/Pz8eOuttxgyZAjbt28HICUlhdmzZ/P000/Trl07pk6dys2bN4u0jYQQQghRRJIAa/ZEzgG%2BcuUKZ8%2Be5e23377vtipVqrBy5UoA0tPTefHFFwkJCWHo0KGkp6czc%2BZMFi1aRHh4OAAxMTHExsby9ddfExsby/DhwwkPD2fcuHH3tR0VFYW/vz%2BHDh3i5MmTjBw5Ej8/P3r16kV4eDgff/wxYWFheHh4sHPnTkaPHs2ePXuoUqXKQx9PcHAwXbp0Yfny5ej1ekJDQ3n77bcJCAgw3ic1NZUffviB9PR0du3axfLly1m1ahVNmjRh69atzJ49Gy8vLwCWLFnCTz/9xPr163FxcWHNmjWMGTOG3bt3ExcXx7vvvsvOnTupV68ehw4dYtKkSTz99NOsW7eOzp07M3nyZAYMGHDfOPfs2cOGDRvYtGkTdevW5fz58wwbNowvv/ySf/zjH3z00Ud88cUXfPLJJ7i5ubFs2TKOHj3K4MGDAZg5cyapqals374dW1tbFixYwOTJk/nPf/6DTqcr0v8%2BLi6O%2BPh4k3VWVlVxcXEpUnxBlpamS3OVL68WZ2trulRhpfDKtLMzXSpp2FAtzt3ddGku1Y3VoIHp0ly1a6vF1atnulTh5GR%2BTJ06pksVRXwtFnvfGRlqcXXrmi5VlCtnfoyNjenycSmpfouj70aN1OI8PEyXKvT6x9/viRNqceKJ8EQmwLdu3QL428TS2tqaLVu2UKdOHTIyMoiJicHJyYnjx4%2Bb3G/SpEnY2tpSp04d2rRpw8WLFwttz9bWlgkTJqDT6WjatCne3t7G%2B27evJnx48fTIO%2BNdtCgQWzbto0vvviCMWPGPHScq1evplq1auj1emJiYqhYsSKxsbEm9wkMDMTGxgYbGxu2bdtGUFAQzZs3B2D48OHs2LEDMFRpP/30U5YuXUrtvDfvSZMmsXXrVvbv30%2BTJk2M9%2BnWrRvt2rUjOjoaiyJ8uvP396d58%2Ba4urpy69YtEhMTcXJyMo5127ZtjBs3jvr16wPw0ksvGceVkJDA3r172bNnD5UrVwYMCXHLli05ceIEjRs3/tv%2BAbZs2cLy5ctN1k2aNJmQkOAixRemYkW1OGdn5S4BbftyLVRzQQAiIrR1HhqqLV5V3gfex27JkpLpt5BvcB6bvG%2BpHrt33y2ZfmvUKFv9aun7yy%2B19fvBB9riVX34oVqc6gf%2B4lAGK7bF7YlMgKtWrQpAfHw8dQv51H/z5k2qVKmCpaUlkZGRjB07lrS0NOrXr4%2BVlRX6ez4J5rcHhqT53tvzVa5c2aRSWfC%2BMTExLFy4kNACb/DZ2dlFSuxOnTrFxIkTiY%2BPx8PDg0qVKt03hoIVzhs3btCtWzeT2/OT3Vu3bpGWlsaUKVNMktqsrCxiYmLo1q0bGzduZO3atUyYMIGcnBwGDBjAq6%2B%2BSrm/qYLo9XqWLFnCvn37qFSpEg0bNiQrK8s41hs3blCzZk3j/S0tLamRt6OMiYkBMFaDC97n2rVrRU6Ag4KC6Ny5s8k6K6uqqExdtrQ0JL9JSaAyZfz6dfNjwFDM9PCA8%2Bchb/aI2TIzzY%2BxszMkv6dOwd27av36zh2oFujubkh%2Bp02DB3zAfCiVGDA84PBwGDbM8MDNpaUCvGQJTJ0KFy6otUCj1D0AACAASURBVKFaAZ49G2bNggLTnsyipQL8zjvw1ltqfWupAL/7Lrz%2BOly6pNaGSvJsY2NIBK9fV3tBqiqpfouj72DFQoWHhyH5feklw45ThWoF%2BMMPYcoU9X5FqfVEJsA1a9bEy8uL3bt306pVK5PbEhISCAgI4N1336V27drMmTOHTz/91JhgrVu37oEVXi1cXV0JCQmhV69exnVXrlzB6W/exGJjY5kyZQrLly83JnZ79%2B7lf//7n8n9CibeNWvW5Po92df169epV68ezs7OlCtXjnXr1tGsWTPj7RcuXKBatWokJCSQk5PDihUryM3NJSoqipCQENzd3Rk%2BfPhDxxoaGsr169f57rvvcHBwAKBPnz7G22vUqGEyLr1ez40bNwCoVq0aYJhGUfADx7lz54zJe1G4uLjcN90hPh6ys4vcxH1yctTi09LU%2BwRD8qvahmquAIbkV/lEKSdPqncMhkRWpQ2t/Z46BUePmh%2BXnKyt3wsX4I8/1GL/5huuh7p8Gc6cUYtVTYC19q36qSzfpUtqH3JA2wsqM1NbfGnrV0vfWqcEnD%2Bv3oZKAlwc/ZYUqQBr9sRuwTfffJOIiAiWL19OYmIier2ekydPMmHCBBo1akS3bt1ITk7GwsIC27z5g9HR0WzYsIHMR/CpefDgwYSFhXE%2B71PioUOH6NWrFz///PND41JTU8nJycEub2LmuXPnWLFiBcADxzl48GC2bt3Kb7/9RnZ2NhEREcYD%2BywsLBg0aBCLFy/mzz//JDc3lx07dtC7d28uX77M9evXGTNmDD/%2B%2BCMWFhbGxNQ57/t8Gxsbkh/wpp%2BSkkK5cuWwtLQkIyODdevWcebMGeOBgEFBQcYPGJmZmaxYsYK4uDjAkAB36tSJefPmkZiYSFZWFmFhYQwaNIikpKQib2chhBBC/A05CE6zJ7ICDNC6dWs2bdrEqlWr6NWrF3fv3qVKlSp0796d8ePHY21tTfv27Rk2bBjDhw8nNzeXWrVqMWLECBYvXlzsZx8YNWoUer2eiRMnEhcXR7Vq1Zg1axZdunR5aFy9evV47bXXePXVV7l79y6urq4MHjyY9957jzNnzhRaQe7WrRtXrlxh4sSJZGZm4u/vT%2BPGjbG2tgZg%2BvTpLFu2jGHDhnH79m1q167N0qVL8fHxAQxn0Hj77beJi4ujQoUKDBs2jB49egCGuctLlizh%2BPHjJtM5wDCn9/XXX8fPz4/y5cvTokUL%2BvXrx5m8as/IkSOJj49nyJAhWFpa0rNnT1xdXY3jWrRoEYsXLyYwMJCUlBQ8PT1Zu3atSUVYCCGEEKKk6fQPmhArSsypU6eoUKGCyXzbAQMGMGTIkPvm2D5Ox44do2bNmsaDE/V6PW3btuX999%2Bnffv2j6zfe04KUWRWVoYD2RIT1aZAqE43LF/ecDD0iROPdwqEvT34%2BhpmAqhOgegwzkctsGFDwwF0Awc%2B3ikQvr4QFQXNm6tNgcg7oNNsPj6wcyf06/d4p0B4ecEnn8DIkY9/CoSXF6xfD6NGPd4pEA0awH/%2BA0OHqk%2BB%2BPRT82PKlTPMP7506fFORSipfouj77xCi9kaNTIcQNenz%2BOdAtGoEXz1FfTurd7vI5hyWSR5pzctVv/4R/G3%2BQQrezXvUuCnn35iwoQJxMfHo9fr2b17N%2BfOnaNdu3YlOq4vv/yS1157jeTkZLKzs/n4448BTOYiCyGEEEI86Z7YKRBl2XPPPUdMTAz9%2B/cnNTWVevXqERYWZtbBZI/CSy%2B9xOzZs3nmmWfIzMykUaNGfPTRR9jb25fouIQQQogypQzO2S1ukgA/gaysrHjjjTd44403SnooJhwcHFi0aFFJD0MIIYQo2yQB1ky2oBBCCCGEKFOkAiyEEEIIUZpIBVgz2YJC/B97dx4XVfU/fvzFsKOpKJIbuaBiWioIIpYaWpGhmXu5hLkELoBLaamZJmmLpgIqflwycsklTSX72KfMfrSolZqpkaIiCAoIuKDs8PtjZGIUlTmXRL68n49Hj5sz933OmTsL73nPufcIIYQQokqRCrAQQgghRGUiFWDNJAEWQgghhKhMJAHWTBJg8cBTXWegenXo0AHOnIHMTNPja9dW69fK6p9tYaFaGzdX91aKqV5dvwiIkpvLXpuseKWR/Hy1NlQXpCi%2BNKCTE9xhie%2B7io1V6/ehh/TbhATtbZii%2BNjm5akvkmBurhan9TnOzlbrt3jJ%2BNxc9TZUVtOpVk2/KERGhvrKMiqLfzz0kL7fixfVXtOgnhwV952Sota36rpaxXFFReptqHzYluxX9cNaVFqSAAshhBBCVCZSAdZMEmAhhBBCiMpEEmDN5AgKIYQQQogqRSrAQgghhBCViVSANZMEWAghhBBCaJaWlsbbb7/NwYMHMTc354UXXmDatGlYlHJm9saNG1m7di0pKSk4OjryyiuvMHToUAAKCwvp0KEDRUVFmJmZGWJ%2B%2Bukn7OzsymWskgALIYQQQlQmD2gFeOLEiTz88MNER0dz6dIlxo4dy9q1axk9erTRft9%2B%2By0ff/wxK1eupF27dhw5coTXXnsNBwcHfHx8iI2NJS8vj0OHDmFVfGmlcvZgHkEhhBBCCFE6na78/9Po3LlzHDx4kDfeeANbW1ucnJwYN24c69evv23f5ORkxowZQ/v27TEzM8PV1RVPT09%2B/fVXAP78809cXFz%2BteQXpAIshBBCCFHlpaSkkHrLNbPr1q2Lo6NjmeJPnTpFrVq1ePjhhw23OTs7k5SUxNWrV6lRo4bh9uKpDsXS0tL49ddfeeuttwB9ApyTk0P//v1JTEzE2dmZKVOm4ObmpvrwbiMJsBBCCCFEZfIvTIHYtGkT4eHhRrdNmDCBwMDAMsVfv34dW1tbo9uK/33jxg2jBLik1NRU/P39eeyxx%2BjVqxcANjY2tG3bluDgYGrWrMn69esZNWoUO3fuxKl4ASSNJAEWQgghhKjiBg8eTPfu3Y1uq1u3bpnj7ezsyLpl9cPif1erVq3UmCNHjhAcHIy7uzvz5883nCz35ptvGu03atQotm3bxg8//MCwYcPKPKa7kQS4iikoKCApKancvkEJIYQQ4j77FyrAjo6OZZ7uUJoWLVpw%2BfJlLl26hIODAwCnT5%2BmXr16PFTK0u9bt24lJCSEoKAgRo4caXTfokWL8PHxoXXr1obbcnNzsba2Vh7frUw%2BgmfPnmXatGl07doVV1dXnn76aRYsWMB11bXSbxEREXHb2YLl5fz580yYMIFOnTrh6enJuHHjSEhIUG6ve/fubNu2zeT7KtKkSZP48ssvK3oYQgghhFD1AJ4E16RJEzp06MC8efPIzMwkISGBZcuWMWDAgNv23bNnD7NnzyYsLOy25Bfg5MmTvPfee6SmppKbm0t4eDiZmZk888wzmsdZzKRHfOjQIfr27UvDhg358ssvOXz4MCtXruSPP/5g5MiRFBQUaB5QQEAAq1at0txOacaPH0/NmjXZu3cve/fupVatWowbN%2B5f6etBlZGRUdFDEEIIIcT/QaGhoeTn59OjRw8GDRpEly5dDHmWq6srO3fuBCA8PJyCggKCgoJwdXU1/Ddr1iwA5s%2BfzyOPPEKfPn3w9PTk4MGDfPLJJ9SqVavcxmrSFIhZs2bx4osvEhQUZLitadOmLFq0iFmzZpGQkECTJk04dOgQixcv5syZM1y5coUWLVowa9Ys2rdvz4EDB3jrrbcYOHAgGzZsICcnB09PT%2BbPn0/16tUJCwvj4MGDfPbZZ2zbto0tW7bQpk0boqKiMDMzo3v37syePRtLS0uKior47LPPWL9%2BPWlpabRs2ZLp06fz2GOP3Tb2K1eu4ODgQHBwsOEiyq%2B88gp9%2BvThypUrxMTE3HVcRUVFrFixgnXr1pGdnc3AgQPLnPAnJyczf/58jh49SlpaGg4ODowdO9bwrcjFxYXhw4eza9cuXF1diYiI4KuvviI0NJS0tDTatWtHgwYNyMvL4/3337/n496zZw%2BhoaFcvHgRR0dHevfuzbhx45gxYwa//fYbhw8f5vjx40RERNw21q1bt7JhwwYSExPJzc2lY8eOzJ8/n9q1awMQGRnJJ598wo0bN%2BjcuTP5%2Bfm0bNmSwMBAcnNzWb58OTt37uTatWu0a9eOmTNn0rhxY1NeZkIIIYS4mwf0OsAODg6EhoaWet/hw4cN/79r1667tlOrVi3mz59frmO7VZkT4Pj4eE6dOsXs2bNvu8/BwYFly5YBkJ2dzdixYwkKCuLll18mOzub6dOn8%2BGHH7JhwwYAEhMTSU5O5n//%2Bx/JyckMHTqUDRs28Nprr93W9qFDh%2BjatSvR0dH89ddf%2BPn50blzZ3x9fdmwYQOffPIJy5cvx9nZmR07dvDqq6/y9ddfG%2BafFKtZsyarV682um3Pnj00bNiQmjVr3nNcX3zxBZ9%2B%2BimrVq2iRYsWhIeHc/HixTIdu5kzZ1KrVi2%2B%2BuorrKysiIyMZO7cufTs2dMwMTw%2BPp59%2B/aRl5fH4cOHmTZtGqGhoXTt2pXvv/%2BeiRMn0rt3b4C7Pu7q1avzxhtvsHLlSjw9PTlx4gRDhw7lySef5L333iM%2BPp6OHTuWelbn0aNHCQkJITIykrZt23Lx4kX8/PyIjIxk4sSJfPXVV4SHhxMREcHjjz/O5s2beffdd2nZsiWgn7Ozf/9%2B1q5di6OjIytXrmTkyJHs3r27zPN2SrsMS3Z2XRwcTJ%2BXVHwy6i0npZaZjY1aXPFDLcepSvev3xLzrUzSrJnx9n7R2m8p89LKpFUr462WNkzRpInxVoXqH06tfd9yckyZNW1qvFVxhxNw7krrBwhAKatf3VPxKldaVrtSfY619t2mjVqcs7PxVkVR0f3v99gxtTjxQCjzuzM9PR3gtsTyVpaWlmzatInGjRuTk5NDYmIitWrV4s8//zTab/z48djY2NC4cWM8PT05e/Zsqe3Z2NgQEBCAmZkZbdu2xcXFxbDv%2BvXr8ff3p9XNPyQDBgxg69at7Ny5s9Q5JSVt3LiRNWvWsHz58jKNa8eOHQwaNIg2N9/gwcHBbN68%2Ba59FAsJCaFatWpYWlqSlJREtWrVyM7O5sqVK4YEuFevXtja2mJra8sXX3zBs88%2Bazgb85lnnuHpp582tHe3xz1kyBBsbGzYunUrhYWFuLm58fvvv6Mrwwdiy5YtiYqKolGjRly5coWUlBRq165NcnIyoK8ODx482HAdvqFDh7J9%2B3YAioqK%2BPzzzwkNDTWcYDd%2B/Hg2b97Mvn378PHxKdOxKu0yLOPHT8DHp2yXYSmNak6n1SOPVMJ%2Bd%2BzQ1vmiRdriK1u/N7/U33fz5lVMvwAhIRXT74IFFdOvli85WrRtWzH9ApTyK2qZREVp63fJEm3xqu5Qsbynivx18wGtAFcmZU6Aiy%2BFkZqaSpNSKgDFZ/2Zm5tz4MABxowZw40bN2jevDkWFhYU3fLtrOSlNYqnM5SmTp06RutAl9w3MTGRDz74gAUlPhjz8/NLnQJRLDc3l/nz57N7925WrFhBp06dyjSulJQU6tevb7jP3NycBg0a3LGfkhISEvjwww%2BJi4ujSZMmhikBhYWFhn1Knnl54cIFozMfAZycnLh06dI9H7eNjQ0bN25k2bJlTJkyhczMTHx8fJg5c6ah0n0nOp2OyMhIdu3ahZ2dHS4uLmRmZhqOwYULF25LZIuT3fT0dG7cuEFwcLBRsp2Xl0diYmKZjhOUfhmW8%2Bfr8vvvZW7CwNZWn/yeOKFWfLrDJQvvydpan4TGx0NOjlobFdVvi9f7qAU2a6ZPQidNgjNn1NqoiH5VT4Jt1Uqf/A4ZAjEx6m2YqkkTffI7fTrExan1q6UCHBICM2eq9a2lArxgAbz%2BOtyhUHJPM2eaHmNrq3%2BOYmLUx67yRrSz0ye/R4/CjRtq/WqpAD/2mL6yqdL3u%2B%2Bq9evsrE9%2Bg4Ph9Gm1NlQrwKGhEBSk3m9FkQRYszInwA0bNqRly5bs3r0bDw8Po/vS0tLw9vZm/vz5ODk5MXfuXD7//HNDIrpmzZo7Vni1qFevHkFBQfj6%2Bhpui4%2BPv%2BMk6fT0dMaOHUtubi5bt2416VJg9erVM7piRFFRESkpKfeMy8vLw9/fn8mTJzNkyBDMzMw4duyYYSJ4sZJJfsOGDUlKSjK6PykpybAk4N0ed2ZmJikpKSxcuBCAv/76i8mTJxMREcG0adPuOta1a9fy008/sWvXLkOlPyAg4J7jatasGfb29lhbW7NmzRrat29vuP/MmTNGq8LcS2mXYbl0CTIzy9zEbbKy1OK1rsCYkwPZ2drauO/9njihrfMzZ7S3cT/7jY3V1m9MDJSY13bfxMWpJ97m5tr7/vtv0%2BO0vIlBn/yqvra0XKUoK0s9XjVxBn0Ceu2aWqzW5Ei17%2BPHtfV7%2BrR6GyUKSkr9ynSGKsekd8nbb7/NF198QXh4OBkZGRQVFfHXX38REBBAmzZt8PHx4dq1a%2Bh0OmxuTqA8cuQIkZGR5ObmlvvgBw0axPLlyzl985tbdHQ0vr6%2BhrWkS8rLy2P06NFUr16djRs3mnwd3IEDB7J582YOHz5MXl4ey5cvv22uamny8vLIzs7GxsYGMzMzkpKS%2BOijjwz33amv//3vf0RHR1NQUMAPP/zAN998U6bHff36dcaMGcOuXbsoKirC0dERnU6Hvb09AFZWVly7wwdbZmYmFhYWWFpakp%2Bfz44dO4iOjjaMc9CgQWzevJmjR4%2BSn5/PF198wZEjRwB99XjAgAEsXLiQixcvUlhYyPbt2%2BnVqxfnzp0r41EWQgghxD09gJdBq2xMmqHfsWNH1q1bR0REBL6%2BvmRlZeHg4MBzzz2Hv78/lpaWPPHEEwwZMoShQ4dSWFhIo0aNGD58OAsXLjT8hF9eRowYQVFREePGjSMlJYWHH36YWbNm0aNHj9v2/f777zl%2B/DjW1tZ4eXkZ3ffVV1/ds69evXqRkZHBpEmTuHLlCs899xwuLi73jLOzs2PevHksWbKEkJAQ6tSpw6BBg4iNjeXkyZM0LeXEjscff5w5c%2BYwe/ZsMjIycHd3x8vLC0tLyzI97tDQUBYvXsysWbOwsbHh%2BeefZ8SIEQC8%2BOKLzJ49m2PHjhlOSiw2cuRITp48ibe3N9bW1rRu3ZohQ4awf/9%2BAHx8fIiPj2fcuHHk5ubStWtXHnvsMcO4pk2bRlhYGEOGDOHy5cs4OTkRGhp623QOIYQQQoiKZFZ0p8m3osKcPXuWwsJCnEucmRoYGEizZs2YNGlShY0rJiaGhx56iIYNGxpu69evHy%2B99BKDBg361/r94Qe1uOrVoUMH%2BP13tV9fb175zWQ2NtCiBZw6dX%2BnQJRHv4/3a6EW2Lq1/gS6Pn3u7xQIrf2qToFwdYVDh8DNTX0KhKur6THlMfdYdQqEiwusWwfDht3fKRCtW8MXX0D//uqvrZUrTY%2BpVk3/HB0%2BfH%2BnQDz0EHTqBPv33/8pEA89BB07wsGDan2rLmLVpo3%2BBLpeve7vFIjHHoOvvgJfX/UpEBX1C%2Be/MfVK5TOpEqt6Ne9KIDY2Fj8/P%2BLj4wE4cOAA0dHRdOvWrULHtX//fgICAkhNTaWoqIjdu3cTGxt7W0VdCCGEEP8imQKhmcJFCsW/7ZlnniE2NpZXXnmFK1eu0LBhQ%2BbOnWu4/FhFGTZsGImJifTt25fr16/TrFkzli9fbvJ8aiGEEEKIiiQJ8ANq7NixjB07tqKHYcTCwoIZM2YwY8aMih6KEEIIUXVVwYpteZMjKIQQQgghqhSpAAshhBBCVCZSAdZMEmAhhBBCiMpEEmDN5AgKIYQQQogqRSrAQgghhBCViVSANZMjKIQQQgghqhSpAIsHXpMmanFWVvptgwaQm2t6/NGjav3WrKlfke3CBbhyRa0NlUWNivtNTFTv93HV5e9q1vxnq9KGajWjVq1/tg4Opsc/9JBav61aGW9VaFnJKSZGPb5ePbU4e3v9NiUFzp83PT4/X63fy5f/2V66pNZGmzamxxSvmOfsDAUFav3a2JgeY2am37ZvD6oLtV69qhZncTMlcHZWe76KXyOmqlHjn61qGyrHum7df7YlVjitFKQCrJkkwEIIIYQQlYkkwJrJERRCCCGEEFWKVICFEEIIISoTqQBrJkdQCCGEEEJUKVIBFkIIIYSoTKQCrJkkwEIIIYQQlYkkwJrJERRCCCGEEFWKVICFEEIIISoTqQBrJkdQCCGEEEJUKVIBFkIIIYSoTKQCrFmVPoLDhw8nLCxMczujR48mIiKiHEb04AgLC2P48OEVPQwhhBBC3EqnK///qhipAJeDVatWVfQQhBBCCCFEGVW9lP8Otm3bxssvv0xISAidOnXCy8uLGTNmkJeXB0B%2Bfj5LliyhW7duuLm5MXToUGJiYgDjSvKpU6cYOnQoHh4eeHt7M23aNDIzMwHIzs7mww8/pFu3bnh4eDB8%2BHCOHj1qGIOLiwshISF4enoSEBBw2xjDwsIICgri9ddfx93dna5du7Jw4ULD/bdWtM%2BfP4%2BLiwvnz583tL9p0yZ8fHxo164dAQEBHDt2jJdeeglXV1f69%2B/PuXPnDPE3btzgzTffxNPTk549e/Lll18a7svNzWXJkiX06NGDjh07MmbMGKPYez0WIYQQQiiSCrBmUgEu4dChQ3Tt2pXo6Gj%2B%2Busv/Pz86Ny5M76%2BvixfvpyoqChWr15N06ZNCQ8Px9/fn7179xq1MWfOHLy8vFi3bh0ZGRn4%2BfmxZcsWXn31VWbPns2JEyeIjIykfv36bNy4kREjRhAVFUWDBg0AiI%2BPZ9%2B%2BfYbE%2B1bffPMN77//Ph988AE//vgj/v7%2B9OjRg/bt25fpMe7atYtNmzaRm5uLr68v48aN45NPPqF%2B/fqMGjWKiIgI5s%2BfD8CxY8fo27cvc%2BfO5eDBg/j7%2B9OoUSPc3d1ZtGgR%2B/fvZ%2B3atTg6OrJy5UpGjhzJ7t27sba2LtNjKU1KSgqpqalGtxUU1KVuXccyt1HMwsJ4a6qaNdXiqlc33qooLKyYfnFxUYtr3Nh4ayozs4rp14TXppEmTYy390urVsZbFQ4OanHNmxtvTVVQoBbXooXxVoW5uekxxQmBlsRA5XVdHKP6ngD1D73i46RyvED9dVke7ycrK9NjtH5%2BnDypFlceqmDCWt4kAS7BxsaGgIAAzMzMaNu2LS4uLpw9exaA7du34%2B/vT/ObH/5jx46lW7duFBUVGbVhbW1NdHQ0zs7OeHl5sWPHDnQ6HTk5OURFRbF06VIa33yz%2Bfn5sWvXLqKionjttdcA6NWrF7a2ttja2pY6xiZNmvDiiy8C0K1bN%2BrWrUtcXFyZE%2BBhw4ZRq1YtAFq0aEHr1q1xdnYGoFOnTvz%2B%2B%2B%2BGfR999FGGDRsGwBNPPIGPjw87duygQ4cOfP7554SGhuLk5ATA%2BPHj2bx5M/v27cPHx6dMj6U0mzZtIjw83Oi28eMnEBQUWOY2blW3rlpc/frKXQLg5qYtXpW7u4bgpyK1dT53rrZ4Ve%2B%2BWzH9zptXMf1u2FAx/QIsW1Yx/VbUeRaavlFqcLOQoMTGRlvfN/9GmEzr67Ki3k9z5qjFde5cvuMQ95UkwCXUqVMHsxLfui0tLQ0JbmpqqqFKC2BlZVVq0rl48WLCwsJYtGgRkydPxs3NjdmzZ1OzZk3y8vJo1KiR0f6NGjUyTFEAcHS8e6Wz7i3ZnKWlJYUmlAtrlfhgMzc3p2aJMqdOpzNK6G8da/369Tl58iTp6encuHGD4OBgdCW%2Bhebl5ZGYmFjmx1KawYMH0717d6PbCgrqcuGCyU1hYaFPflNTIT/f9PhTp0yPAf3fSzc3OHQIbs5%2BMZlqBdjdHX77Tb3fp9a8ohbYuLE%2B%2BX37bSgxFabMtFSA330XZs1S61dLBXjePJg%2BHeLi1Nq4OYXKJK1a6ZOMIUPU4kFbBXjZMhg3DmJjTY/XUgGOiICAAPU35fbtpsfodPo3VWam2hsS1KqSZmb65DcnB24psJTZ9etqcebm%2BuT38mW15ytQsVBRHu8n1QrwnDnwzjtqnx8VSSrAmkkCXEb169fnQoksLC8vj48%2B%2BojRo0cbbissLOTEiRMEBgYyffp0Lly4wPz583nzzTfZsmUL1tbWJCQkGCquoJ8mUDLhM9Pws5dOpzOabpCRkXHbPqa0n5KSYvTvhIQEGjZsiL29PdbW1qxZs8boS8CZM2d4%2BOGHlfoq5ujoeFvifO4c5Oaa3JRBfr5a/JUr6n2C/u%2Bmahuqf2%2B19svff6t3DPonS6UNrR/m586p/RyZk6Ot37g49UT08GH1fmNi1OPr1VPvF/TJ759/mh6n8i20pFOnoMQ5EyZRTb5B/2ZUjVdNYItjVeO1HuuCArU2VN8LxbS8n7RUvVU/P0SlJl8hyqhfv36sXr2as2fPkp%2Bfz4oVK/j222%2Bxt7c37KPT6QgJCWHx4sXk5ORQu3ZtrK2tsbe3R6fT0b9/fz7%2B%2BGPOnTtHbm4un376KbGxsfj6%2BpbLGJ2dnYmOjubq1atcu3aNlStXamrv6NGjfPHFF%2BTl5fH999%2Bzd%2B9eBg4ciE6nY8CAASxcuJCLFy9SWFjI9u3b6dWrl9GJcEIIIYT4F8hJcJpJBbiMRo8eTX5%2BPqNGjeLKlSs8/vjjrFy5EktLS6P9Fi9ezNy5c3nyyScpLCzEw8ODuTfnRU6dOpWwsDBGjBjB5cuXcXFxMZxUVx78/f2ZMWMGPXr04KGHHiIoKIg9e/Yot9e5c2e%2B%2B%2B47QkJCaNSoEUuWLKF169YATJs2jbCwMIYMGcLly5dxcnIiNDTUcL8QQggh/iVVMGEtb2ZFt57FJcQDRrWobGWlP5HtwgW1KRCqv7bWrAldu8L/%2B3/3dwpEzZrw1FOwb596v33meaoFurhAZCS88sr9nQLRsiV8%2Bin4%2Bd3fKRDlMRdXZQqDq6t%2Bcrmb2/2fAvH44/DNN/Dss/d3CkTbtvDdd9CjyiJsbwAAIABJREFUh/qbUuW1YW4ONWrA1avqUyBUfpY3M9PHZWerT4G4elUtzsIC6tSBtDS156tnT7V%2By%2BP9pHKsW7aEtWthxAj1KRA//6wWp9W1a%2BXf5kMPlX%2BbDzCpAAshhBBCVCZSAdZMjqAQQgghhKhSpAIshBBCCFGZSAVYM0mAhRBCCCEqE0mANZMjKIQQQgghqhSpAAshhBBCVCZSAdZMjqAQQgghhNAsLS2NcePG4e7ujqenJ%2B%2B99x75d7ik3g8//EDv3r1p3749PXv25Pvvvze6f%2BXKlXTt2pX27dszfPhwzpw5U65jlQRYCCGEEKIyeUBXgps4cSJ2dnZER0ezdetWfvnlF9auXXvbfnFxcQQGBhIcHMxvv/1GYGAgEydOJDk5GYDt27fz2WefsXr1ag4cOECbNm0ICgqiPJeukARYCCGEEKIyeQAT4HPnznHw4EHeeOMNbG1tcXJyYty4caxfv/62fbdv3467uztPP/00FhYWPP/883h4eLBp0yYANm/ezJAhQ2jRogXW1tZMmTKFpKQkDhw4oHmcxWQOsHjg1aihFmdurt9Wq6a2SFBFUnnM1ar9s1X%2Bklyrllpc8QpCDz2k1obqqkZmZv9si//fFMUvElMV/7HQ6dTbUFmRzcHhn63qim4XL6rF1a%2Bv3166pNZG8dgrQkqK6THW1vo3Ynq6%2BoqBKm9kCwv9B9a1a%2Bqr58XFqcVVq6ZfCS4xEa5fNz0%2BO1ut3%2BLjm5Oj3obKh17JfrOy1Pr9PyQlJYXU1FSj2%2BrWrYujo2OZ4k%2BdOkWtWrV4%2BOGHDbc5OzuTlJTE1atXqVHi/RAbG0vLli2N4ps3b07MzZUAY2NjGTNmjOE%2BS0tLmjRpQkxMDJ06dTL5sZVGEmAhhBBCiEqkCIUv/PewadMmwsPDjW6bMGECgYGBZYq/fv06tra2RrcV//vGjRtGCXBp%2B9rY2HDjxo0y3V8eJAEWQgghhKjiBg8eTPfu3Y1uq1u3bpnj7ezsyLqlkl7872rFP1HeZGtrS/Yt1f7s7GzDfve6vzxIAiyEEEIIUYkUFpZ/m46OjmWe7lCaFi1acPnyZS5duoTDzSlPp0%2Bfpl69ejxUPE3uppYtW3L8%2BHGj22JjY3nssccMbZ06dQpvb28A8vLyiIuLu23ahBZyEpwQQgghRCVSWFj%2B/2nVpEkTOnTowLx588jMzCQhIYFly5YxYMCA2/Z94YUXOHjwILt37yY/P5/du3dz8OBB%2BvTpA0D//v1Zt24dMTEx5OTksHDhQhwcHHB3d9c%2B0JskARZCCCGEEJqFhoaSn59Pjx49GDRoEF26dGHcuHEAuLq6snPnTkB/ctzSpUtZsWIFHh4eLFu2jLCwMJo2bQrAgAEDGDFiBOPHj6dTp06cOHGCFStWYGlpWW5jlSkQQgghhBCVyL8xBaI8ODg4EBoaWup9hw8fNvp3ly5d6NKlS6n7mpmZMXLkSEaOHFnuYywmFWAhhBBCCFGlSAVYCCGEEKISeVArwJWJJMBCCCGEEJWIJMDayRQIIYQQQghRpUgCXAXFqS6TKYQQQogK9yBeBq2yqTQJ8PDhwwkLC9PczujRo4mIiCiHEd1dWFgYw4cP/9f7MdXevXsZNWpURQ9DCCGEEKLCVLk5wKtWraroIVSoy5cvU1RUVNHDEEIIIYSiqlixLW%2BVpgJc0rZt23j55ZcJCQmhU6dOeHl5MWPGDPLy8gDIz89nyZIldOvWDTc3N4YOHUpMTAxgXEk%2BdeoUQ4cOxcPDA29vb6ZNm0ZmZiagX3P6ww8/pFu3bnh4eDB8%2BHCOHj1qGIOLiwshISF4enoSEBBw1/EWFRXxn//8h969e%2BPu7o6HhwdTpkwxrHP95ptvEhQURM%2BePenUqRPx8fGcP3%2BeUaNG4ebmxnPPPcfatWtxcXExtHn8%2BHGGDx%2BOh4cHzz77LGvXrjUktsnJyYwePZqOHTvStWtXJkyYQEpKCgcOHOCdd94hKSkJV1dXkpOTbxvr6dOn8ff356mnnqJt27Y8//zzfP/994b7T5w4wcsvv4yrqyt9%2BvRh%2BfLlRmuH//zzzwwYMAB3d3d8fX0NF70WQgghRPmQKRDaVdoK8KFDh%2BjatSvR0dH89ddf%2BPn50blzZ3x9fVm%2BfDlRUVGsXr2apk2bEh4ejr%2B/P3v37jVqY86cOXh5ebFu3ToyMjLw8/Njy5YtvPrqq8yePZsTJ04QGRlJ/fr12bhxIyNGjCAqKooGDRoAEB8fz759%2BwyJ9518/fXXREZGsm7dOpo0acLp06cZMmQIu3btYuDAgQBER0ezadMm6tWrR7Vq1XjhhRdo27YtP/74IxkZGYwfP97QXnJyMn5%2BfkyaNIk1a9Zw7tw5xo0bh42NDS%2B99BIff/wx9erVY/ny5eTk5BAUFMR//vMfZs6cyZw5cwgPD7/tWBQLDAykR48ehIeHU1RUxIIFC5g9ezbe3t5kZmYyevRoBg8ezKeffsrZs2cJCAjAzMwMgJiYGMaOHctHH31Ejx49%2BOOPPxg3bhz29vZ3vNj1rVJSUkhNTTW6zcamLnXrmr4%2BuU5nvDVVzZpqcdWrG29V2NqaHmNnZ7xV0ry5WpyTk/HWVNevq8U1bmy8NVV%2BvlpckybGWxX29qbHFD8/qs8TQP36anGtWhlvTaXyeAFatDDeqrC2Nj3Gysp4q8JC4U9scYxKbLFq1dTiij94VD6AAB59VC3u5upfhq0KledJ6/v4ZmFNVE6VNgG2sbExJF9t27bFxcWFs2fPArB9%2B3b8/f1pfvOPxNixY%2BnWrdttP/1bW1sTHR2Ns7MzXl5e7NixA51OR05ODlFRUSxdupTGN/%2Bw%2Bvn5sWvXLqKionjttdcA6NWrF7a2ttje48Oia9euuLm5Ua9ePdLT08nIyKBWrVpGFdj27dvTsmVLAH7//Xfi4uLYsmULdnZ22NnZMWnSJEO/O3fuxNnZmaFDhwLQvHlzRo0axbp163jppZewtrbm119/5auvvsLLy4tVq1ahK2MWuGLFCh5%2B%2BGGKiopITEykRo0ahnHu3bsXc3NzAgMD0el0uLi4MHr0aFavXg3A559/To8ePXj22WcBcHNzY9CgQaxfv77MCfCmTZsIDw83um38%2BAkEBQWWKb40qolo167KXQLg5qYtXlWbNhqCPZZq6/zNN7XFq5ozp2L6DQmpmH6XLauYfgE2bKiYfu/DuRululnwuO9UvzAA1K2rrW/VLxtffKGt3wULtMWrmj9fLc7VtXzHYYKqWLEtb5U2Aa5Tp46h8ghgaWlpSHBTU1MNVVoAKysr2rdvf1sbixcvJiwsjEWLFjF58mTc3NyYPXs2NWvWJC8vj0aNGhnt36hRI86fP2/4t6Nj2aqSRUVFLFq0iO%2B//57atWvz6KOPkpeXZ5SQl2zr4sWL2NvbY1eilFdyLImJiRw/fhx3d3fDbYWFhZibmwMwc%2BZMVqxYwerVq3nzzTdp1aoVM2fONNr/TmJiYhg3bhypqak4OztTu3ZtwzgvXrxIgwYNjJJppxIVv8TERPbv32/UT0FBAY888kiZjhPA4MGDjaZUgL4CfPVqmZsw0On0yW9mptqHxZEjpseAvk83Nzh0SN%2B3CtUKcJs2cPw43Lih1q/H2vH33qk0Tk765Pf99yEhwfR4LRXgOXPgnXfg3DnT47VUgENCYOZMUL2qSkqK6THNm%2BuT33HjIDZWrd9Ll9TiWrXSJ79DhqhVvrRUgCMiICAATp1Sa%2BPml3STWFnpk9%2BkJMjNVetXpRJrYaE/VhkZ6q/PCxfU4mxt9cf71CnIyjI9XvWLaNOm%2BuT39dfhZiHLZKoV4Pnz4a231N/HotKqtAnw3dSvX58LJT4A8vLy%2BOijjxg9erThtsLCQk6cOEFgYCDTp0/nwoULzJ8/nzfffJMtW7ZgbW1NQkICzs7Ohpj4%2BHij5KxkAn43CxYsICkpib1791L9Zjmyd%2B/eRvuUbKtBgwakp6eTlZVlqC4nJSUZ7q9Xrx6enp6GyitARkYG128mESdOnGDw4MEEBgaSnp7O0qVLmTBhAvv377/rOJOTkwkODiY8PNzwOPfs2cM333xjGFdSUhJFRUWG8d46rr59%2B/Luu%2B8abktJSTHppDtHR8fbvlhkZEBBQZmbuE1hoVr8lSvqfYI%2B%2BVVtQ8vjvXFDPfFWTqqKJSSotXHtmrZ%2Bz52DkydNj7vH9KV7iouDv/9Wiy3xZdpksbHw559qsRcvqvcL%2BuT38GHT4xwctPV76hSUOA/DJDk56v3m5qrHq0y9KJafr54Aq36hLJaVpdbGX39p6/fsWfU2tBzruLhKN51BKsDaVcqT4O6lX79%2BrF69mrNnz5Kfn8%2BKFSv49ttvsS9RgdDpdISEhLB48WJycnKoXbs21tbW2Nvbo9Pp6N%2B/Px9//DHnzp0jNzeXTz/9lNjYWHx9fU0eT2ZmJtbW1pibm5OTk8OaNWs4efLkHecOt2vXjubNm/P%2B%2B%2B%2BTlZVFcnIyoaGhhvt79%2B7NkSNH2LlzJ/n5%2BaSkpBAQEMD7778PQEREBHPnziUzM5MaNWpga2treOzW1tZkZWWRX8oH6/Xr1ykoKDAk3bGxsSxdqv9JPDc3l%2B7du1NUVERERAS5ubmcOXPGKAkfMGAAUVFR/PjjjxQWFhIXF8ewYcNYs2aNycdMCCGEEKWTk%2BC0%2Bz%2BZAI8ePZrevXszatQoPD09%2Be2331i5ciWWlpZG%2By1evJjTp0/z5JNP0rlzZ65du8bcuXMBmDp1Kk8%2B%2BSQjRozA09OTr7/%2B2nBSnakmTpxIdnY2nTt3pnv37hw5coQ%2Bffpw8g4VK51OR2hoKHFxcXh5eeHn54eHh4dh/A0bNmTVqlVs2rSJzp0706dPH5o1a2ZIgN99910KCwvp0aMHHh4e/PHHHyxZsgQADw8P6tSpg4eHB3/fUrlq1qwZU6dO5Y033qBDhw4EBwfTv39/LC0tOXnyJHZ2dixbtozvvvuOjh07MnnyZJ544gnDuNq1a8fHH3/Mxx9/jIeHB8OGDaN79%2B5MmTLF5GMmhBBCCPFvqTRTID777DPD//fr149%2B/frd8X4LCwsmTJjAhAkT7tqOs7Mza9euLbU/W1tbpk6dytSpU0u9/9bk8VaBgf%2BctOXk5MS6devuuG9x4losOzubCxcusGbNGsO83r1797Jr1y7DPq6urqxfv77U9hwdHQ2V29Lui4qKuuNYRo0addtCGX5%2BfoB%2BmkVeXh5bt2413PfZZ58ZLjEH8NRTT/HUU0/dsX0hhBBCaFMVK7bl7f9kBbiys7S0ZOLEiWzevJnCwkLS0tJYs2YN3t7eFTqugoIC/Pz8%2BOGHHwA4f/48GzZsqPBxCSGEEEKYQhLgB5C5uTlLly5l%2B/bteHh40Lt3b1q0aMGbFXWJqZscHBxYvHgxCxYswNXVlaFDh%2BLj4yNLKwshhBD3kcwB1q7STIGoatzd3dm8eXNFD%2BM2Tz/9NE8//XRFD0MIIYSosqpiwlrepAIshBBCCCGqFKkACyGEEEJUIlIB1k4qwEIIIYQQokqRCrAQQgghRCUiFWDtJAEWQgghhKhEJAHWThJg8cCzUHyV6m5O8DE3BzMz0%2BNv3FDrt3hJ%2Buxs9TYyM02PKV7pOy0NMjLU%2BuUOy3PfU/HS2vn5am1kZan1m5Pzz1aljexstX6L%2B8rKUnuy4J9jZoqCgn%2B2KvEADg5qccUvMHt7tTYuXVLrt/jFnJGh3kb16qbHFK8camf3z/9XFo0aqcUVP05HR7X38c2Fm5TjzM3V27CyMj2m%2BPFaWqrFi0pNEmAhhBBCiEpEKsDaSQIshBBCCFGJSAKsnVwFQgghhBBCVClSARZCCCGEqESkAqydVICFEEIIIUSVIhVgIYQQQohKRCrA2kkCLIQQQghRiUgCrJ1MgRBCCCGEEFWKVICFEEIIISoRqQBrJxVgIYQQQghRpUgCXAXFxcVV9BCEEEIIoaiwsPz/q2o0JcDDhw8nLCxM8yBGjx5NRESE5nbKYuPGjfj4%2BODq6oqPjw/r169Xbmvbtm10797d5Psq0okTJ%2BjVq1dFD0MIIYQQiiQB1u6BmAO8atWq%2B9LPt99%2By8cff8zKlStp164dR44c4bXXXsPBwQEfH5/7MoaKdu3aNfLy8ip6GEIIIYQQFabcpkBs27aNl19%2BmZCQEDp16oSXlxczZswwJFv5%2BfksWbKEbt264ebmxtChQ4mJiQGMK8mnTp1i6NCheHh44O3tzbRp08jMzAQgOzubDz/8kG7duuHh4cHw4cM5evSoYQwuLi6EhITg6elJQEDAbWNMTk5mzJgxtG/fHjMzM1xdXfH09OTXX381jGPhwoUMHToUV1dXevbsye7duw3xp0%2BfZvjw4bi6utK7d29OnDhR5uOzdetW%2BvXrh6enJ66urvj7%2B5Oeng5AWFgYI0eOpH///nTs2JFff/2VjIwMJk2aRIcOHejRowefffYZrVu35vz58wDEx8cTEBCAp6cn3t7eLFq0iNzcXAAyMzOZNGkSnp6ePPHEE4waNYrTp0%2BTkJDAmDFjAHB1deXw4cOlHqOJEyfSvXt32rVrR48ePdi6davh/vPnzzNq1Cjc3Nx47rnnWLt2LS4uLob7jx8/zvDhw/Hw8ODZZ59l7dq1FBUVlfk4CSGEEOLupAKsXblWgA8dOkTXrl2Jjo7mr7/%2Bws/Pj86dO%2BPr68vy5cuJiopi9erVNG3alPDwcPz9/dm7d69RG3PmzMHLy4t169aRkZGBn58fW7Zs4dVXX2X27NmcOHGCyMhI6tevz8aNGxkxYgRRUVE0aNAA0CeG%2B/btK7XKOXToUKN/p6Wl8euvv/LWW28Zbtu8eTOffPIJzZs3Z%2BnSpcyaNYsePXqg0%2Bnw9/ena9eurFq1ivj4eMaMGYNOd%2B/vEEePHiUkJITIyEjatm3LxYsX8fPzIzIykokTJwLwyy%2B/sGbNGtq2bYu1tTX%2B/v6YmZnx3XffUVhYyOuvv05BQQEAN27cYMSIEfj6%2BrJkyRLS09MJCgqisLCQKVOmsGbNGjIzM/nhhx/Q6XTMmjWLBQsWsHz5clauXMkrr7xSavILMHPmTGrVqsVXX32FlZUVkZGRzJ07l549e2JjY4O/vz9t27blxx9/JCMjg/Hjxxtik5OT8fPzY9KkSaxZs4Zz584xbtw4bGxseOmll%2B55nABSUlJITU01us3Ori6Ojo5lii%2Bp%2BKkpw1NUKnt7tbiHHjLeqlD5MKpRw3irpEULtTgnJ%2BOtqa5eVYtr0sR4a6qbXxpN1rSp8VbF5cumxxQ/P6rPkxZa%2B87IUItr1cp4q8LS0vQYCwvjrQozs8rTb3n0/eijanHl8X6ysTE9pnFj462p/v5bLU48EMo1AbaxsSEgIAAzMzPatm2Li4sLZ8%2BeBWD79u34%2B/vTvHlzAMaOHUu3bt1uqw5aW1sTHR2Ns7MzXl5e7NixA51OR05ODlFRUSxdupTGN1%2Bsfn5%2B7Nq1i6ioKF577TUAevXqha2tLba2tncda2pqKv7%2B/jz22GNGc2J9fHxo3bo1AH379iUiIoK0tDTOnz/PhQsXmDp1KtbW1rRo0YJXX32VTz/99J7HpWXLlkRFRdGoUSOuXLlCSkoKtWvXJjk52bCPk5MTXl5egD6R/PHHH/n666%2BpVasWANOnT8fX1xeAffv2kZuby%2BTJkzEzM6N%2B/foEBwcTFBTElClTsLGxISYmhi%2B//JInnniCefPmlSlRBwgJCaFatWpYWlqSlJREtWrVyM7O5sqVK8TExBAXF8eWLVuws7PDzs6OSZMmGY79zp07cXZ2NnzRaN68OaNGjWLdunVlToA3bdpEeHi40W3jx08gKCiwTPGlucdL4Y6efVa5SwBuPp33XefOGoKfW6Gt85kztcWrmj%2B/YvpdsKBi%2Br1P50w8UH1v2FAx/dauXTH9qn4DLw8ODmpxmzdr6/eDD7TFq5o7Vy3O07N8x2GCqlixLW/lmgDXqVMHsxLfPC0tLQ0JbmpqqqFKC2BlZUX79u1va2Px4sWEhYWxaNEiJk%2BejJubG7Nnz6ZmzZrk5eXRqFEjo/0bNWpkmBYAlKlSeOTIEYKDg3F3d2f%2B/PlYlPi2W7duXcP/F99eWFhIcnIy9vb22JT4lvnII4/csy8AnU5HZGQku3btws7ODhcXFzIzM42S/5LjvnDhguGxFXMqUVlLTEwkPT0dDw8Pw21FRUXk5eWRlpbGmDFjsLKyYuvWrbz77rs4OTkxZcoUni1DRpeQkMCHH35IXFwcTZo0MXzZKCws5OLFi9jb22NnZ2fYv%2BQYExMTOX78OO7u7obbCgsLMTc3L9NxAhg8ePBtJw/a2dXl%2BvUyN2Gg0%2BmT36wstQ%2BLn34yPQb0lV8vL/jlF7h2Ta0N1Qpw587w88/qBdXntvurBTo56ZPfkBBISDA9XksFeP58eOstULm6iZYK8IIF8PrrcPNLvslUK8ARERAQAKdOqfWrSmvfWirAGzbAkCFwc9qcyf77X9NjLCz0yW96OuTnq/WrWgG2t9cfL9V%2Bb/5aqNS3gwNcuqTWd1CQWr9Nm%2BqT32nT1N9PqhXguXPh7bfh3Dm1fiuIJMDa3beT4OrXr29I7ADy8vL46KOPGD16tOG2wsJCTpw4QWBgINOnT%2BfChQvMnz%2BfN998ky1btmBtbU1CQgLOzs6GmPj4eKOEyeweHzhbt24lJCSEoKAgRo4cadL409PTuX79OtWqVQPg4sWLZYpdu3YtP/30E7t27cLh5jfrW%2Bcolxx38ReFxMREmt78SSgxMdFwf7169XjkkUf4b4kP9czMTNLS0qhduzZ///033bt3Z8SIEVy7do0NGzYwadIk9u/ff9dx5uXl4e/vz%2BTJkxkyZAhmZmYcO3aMnTt3GsaVnp5OVlaWocKelJRkNC5PT09Wr15tuC0jI4PrJmSvjo6Ot32JuXZN25tddX6T6t/rYteuqbeh5fFevaph7FqTqoSE%2B5scFYuLU0uOsrO19Xv2LJhwLoCRS5fU%2Bz11Ckqc/3Bfqfat5fGC/vm9w9Ste9Jy4m9%2Bvnq86vyr4n5VE2DVuJLxKo/5r7%2B09Xv2rHobJQozJjt3TqYzVEH37TrA/fr1Y/Xq1Zw9e5b8/HxWrFjBt99%2Bi32Jn3l0Oh0hISEsXryYnJwcateujbW1Nfb29uh0Ovr378/HH3/MuXPnyM3N5dNPPyU2NtYwNeBe9uzZw%2BzZsw0nnZnC1dWVpk2bEhISQlZWFufOnWPNmjVlis3MzMTCwgJLS0vy8/PZsWMH0dHRd7wag6OjI97e3nz00UdcuXKFK1eu8OGHHxru9/b25vr166xatYrc3FyuXr3KtGnTmDRpEmZmZmzZsoWpU6eSlpZG9erVqV69OnZ2dlhZWWFtbQ3orwZxq7y8PLKzs7GxscHMzIykpCQ%2B%2Bugjw33t2rWjefPmvP/%2B%2B2RlZZGcnExoaKghvnfv3hw5coSdO3eSn59PSkoKAQEBvP/%2B%2B2U%2BzkIIIYS4OzkJTrv7lgCPHj2a3r17M2rUKDw9Pfntt99YuXIllrecnLB48WJOnz7Nk08%2BSefOnbl27Rpzb87PmTp1Kk8%2B%2BSQjRozA09OTr7/%2B2nBSXVmEh4dTUFBAUFAQrq6uhv9mzZp1z1hzc3P%2B85//kJKSQufOnRk9ejQ9evQoU78jR46kfv36eHt706VLF3bu3MmQIUM4efLkHWPee%2B89zMzMeOqpp%2Bjbt69hXrKlpSXVq1dn7dq1HDhwgK5du/L000%2Bj0%2BlYvnw5AJMnT6Zx48b4%2Bvri5ubGtm3bWLZsGdbW1rRs2ZIOHTrQpUsXfvjhB6M%2B7ezsmDdvHkuXLsXV1ZVXXnmFJ554AgcHB06ePIlOpyM0NJS4uDi8vLzw8/PDw8PD8Bw2bNiQVatWsWnTJjp37kyfPn1o1qyZJMBCCCFEFXfjxg3eeustPD096dChA1OnTr3rL8R79uyhT58%2BuLm50b17d8LDwykskan37NmTdu3aGeVzp0%2BfLvN4zIrkGlUPpJ9%2B%2BokOHToY5hz//fffvPjiixw5csRQxb3fsrOzOXz4MB07djTM6927dy/vvPMO0dHR/1q/qvNodTqoVg2uX1f7dlviCngmsbfXn0D3zTf3dwqEvT0895x%2BuqNqvy%2BvVFy8pUULWLEC/P3v//zQjRvh5Zfv7xSI1q3hiy%2Bgf//7OwWibVv47jvo0eP%2BT4HQ2rfqFAhXVzh0CNzc1KdAlDhPpMwsLcHREVJS7u8UCAsLqFsXUlPv/xQIS0uoVw8uXlR7zM8/r9bvo4/qT6AbNOj%2BToFwcYHISHjlFfUpEAcOqMVp9G/8ye3SpfzbLOmtt97iwoULLF68mIKCAiZOnEjz5s155513btv32LFjDB06lMWLF9OtWzfOnj3LmDFjGDZsGCNHjiQzMxN3d3e%2B%2B%2B47GjZsqDQeWQr5AfXBBx%2BwfPly8vPzyczMZPny5XTu3LnCkl/QV58nTpzI5s2bKSwsJC0tjTVr1uDt7V1hYxJCCCGqmn9jCkRKSgrHjx83%2Bi8lJaVcxpuVlcWuXbsICgqiVq1a1KlTh9dff51t27aRlZV12/6JiYm89NJLeHt7o9PpcHZ25plnnjGs23Ds2DFq1aqlnPzCA7ISnLjdwoULDYuK6HQ6unTpYjQPuCKYm5uzdOlSPvzwQxYsWIC1tTU%2BPj688cYbFTouIYQQQmhT2mVIJ0yYQGBg2S5Dmp2dbXR515KysrLIy8ujZcuWhtucnZ3Jzs4mLi6OR2%2B5hrSPj4/RCr3Z2dns27eP3r17A/Dnn39ia2vLsGHDOHXqFA0bNiQwMNCkgpwkwA%2BoFi1alOkaw/ebu7s7m7Ve61EIIYQQyv6Nk9ZKuwxpyUvD3ssff/zBK6%2B8Uup9wcHBAEaXUS2%2BmtS9rhSVmZlJcHAwNjY2jBgxAtBfOevxxx9n8uTJNGjQgP/%2B978EBgaybt26Ui%2BxWxpJgIUQQgghqriv3QySAAAgAElEQVTSLkNqCk9PT/6%2Bw1zqEydOsGTJErKysgyXki2e%2BlC9evU7tnnmzBmCgoKoU6cOkZGRhn1LXkIX4IUXXiAqKoo9e/aUOQGWOcBCCCGEEJVIZbsMWtOmTbG0tCQ2NtZw2%2BnTp7G0tKTJHZay/%2BGHHxg4cCBdunRh9erV1KxZ03Df6tWr%2BeWXX4z2z83NNek8KUmAhRBCCCEqkcqWANva2tKzZ08WLFhAeno66enpLFiwgF69ehmtsFvsyJEjjB8/nrfeeotp06YZrdgL%2BhVz58yZQ0JCAvn5%2BWzdupXDhw/Tt2/fMo9JEmAhhBBCCPGveuedd2jSpAm9e/fmueeeo1GjRkbrMPj6%2BhIREQFAREQE%2Bfn5vPfee0bX%2BS2e%2BjB16lS6du3KkCFDcHd35/PPP%2Bc///kPjRs3LvN4ZA6wEEIIIUQlUhlXbqtevTpz5841LG52q6%2B%2B%2Bsrw/8WJ8J1YWVkxffp0pk%2BfrjweqQALIYQQQogqRVaCEw%2B81FS1OAsL/epoGRlqCyPVtbv7pVnuSKcDW1vIylL/mm6h8OOMmRlYWUFuLqi%2BrVVWzAJ9v05OkJCg799UqitXWVtDkyYQFwc5OabHq764qlXTr1B2%2BLB%2BqUEVbdqYHmNuDjVqwNWrUFCg1q/qhe21Huu7nOl9V%2BWxIlujRqbHlMcKdCWueVpmrVvD9u3Qt6/6KoOqx6lNG9i1C3r3huPHTY/fs0etX2treOQRiI9Xe20B3Lhheoytrf45OnlS/3mtol07tTiNvv66/Nvs2bP823yQyRQIIYQQQohKpDJOgXjQyBQIIYQQQghRpUgFWAghhBCiEpEKsHZSARZCCCGEEFWKVICFEEIIISoRqQBrJwmwEEIIIUQlIgmwdjIFQgghhBBCVClSARZCCCGEqESkAqydVICFEEIIIUSVIgnwAyIuLq6ihyCEEEKISqCwsPz/q2qqZALcvXt3tm3bdtvt27Zto3v37mVqY%2BfOnfj6%2BpZp37CwMIYPH37H%2B9evX8/bb799x/tdXV357bffytSXEEIIIf5vkwRYO5kDrOiFF17ghRdeKJe20tPT73r/YdV16IUQQgghxG2qZAW4rOLj4wkICMDT0xNvb28WLVpEbm4ucHu1%2BOeff%2BbFF1/Ezc2Nl156iY8%2B%2Bsio6nv9%2BnVmzpzJk08%2BiaenJ4sWLQJg%2B/btrFixgt9%2B%2Bw13d/dSx%2BHi4sKBAwcAffV6xYoVvPjii7i6uvLiiy%2Byf//%2BOz6Gu40rLCyMkSNH0r9/fzp27Mivv/7K6dOn8ff356mnnqJt27Y8//zzfP/99wBMnTqVKVOmGLU/ceJE5syZc8/jJYQQQojyIRVg7apsBXjOnDnMmzfP6La8vDzq1KkDwI0bNxgxYgS%2Bvr4sWbKE9PR0goKCKCwsvC0JPH/%2BPAEBAcyYMYP%2B/ftz5MgRAgICePTRRw37nDhxAj8/P%2BbOncuBAwcYMWIETz31FH379uX8%2BfMcPHiQzz77rExj/%2BKLL1i5ciWOjo7MmTOH2bNn89///ve2/coyrl9%2B%2BYU1a9bQtm1brK2teeGFF%2BjRowfh4eEUFRWxYMECZs%2Bejbe3N4MGDWLUqFFkZmZSvXp1rl69yt69e9m4caNJx%2BtuUlJSSE1NNbrNwqIujo6OZW6jmLm58dZkOsXvh2Zm/2y1tqHaryorK7U4S0vjralUn6Ti8aqOu1o1tThbW%2BOtCpXHXPx6Un1dAVhbq8VpPdaqrw0LC%2BOtCldX02NatTLeqmjc2PSYZs2Mtyry89XinJ2Nt6ZSfW1p/fwAKCoyPaZ4vKrjzspSiysHVTFhLW9VNgF%2B55136Nevn9Ft27ZtIzw8HIB9%2B/aRm5vL5MmTMTMzo379%2BgQHBxMUFHRbQrdr1y4effRRBg8eDIC7uzuDBg3izz//NOzTokUL%2BvTpA0CnTp1wcHAgPj4eV4UP5gEDBtD45gdr7969%2BfLLL0vdryzjcnJywsvLy/DvFStW8PDDD1NUVERiYiI1atQgOTnZEF%2B/fn2%2B/vprBg4cSFRUFM2aNaNNmzbs3r27zMfrbjZt2mR4DoqNHz%2BBoKDAMrdxqxo1VCM1JDcANjba4lVp%2BSPi5KSt73r1tMWratBALa5JE239akmOtKheXT1W/Q2hp3qstapdWz320CH12A0b1GO1WLiwYvoFWLy4YvqtX79i%2BlX5ogLwxx/lOw5xX1XZBPheEhMTSU9Px8PDw3BbUVEReXl5pKWlGe174cIFGjZsaHSbk5OTUaJZq1Yto/utrKwoKChQGpuDg4Ph/y0sLCi6wzffsozr1spqTEwM48aNIzU1FWdnZ2rXrm3U/sCBA9mxYwcDBw5k%2B/btDBw4ELj38SqurN/L4MGDbzsR0cKiLhkZZQo3Ym6u/1t/9SqoHGp7G8Vv92Zm%2BuQ3O1utKgFq1UEzM33ym5en3u/NLzsms7TUJ78XL%2Br7N5XiewErK31ClpQEKtNtVF5YoK/8tmoFMTHqVSCVKptOp09%2BMzPVS0D3OOfgjrQeazs7tX4tLPTJb3q6emXzuedMj2nVSp/8Dhmif55VqFaAFy6EKVPgzBm1frVUgBcvhokT4fRp0%2BOXLlXr19JSn/xeuKD2%2BQH6z1tTWVvrn6Nz5yAnR63fCiIVYO0kAb6DevXq8cgjjxhNLcjMzCQtLY3at1QiGjZsaJgnWywpKem%2BjPNuyjIusxI/lycnJxMcHEx4eLghCd2zZw/ffPONYZ%2B%2BffuyePFifv75Z/7%2B%2B2969eoFmHa87sbR0fG2pDw1Vf3zHPS5lVK86idM8c/TRUXa21BRVKSeAGuds52Xp9aGlicY9H2q/AG7fl1bv1lZ6m2oJv2gf12pxmv9Q696rLX8MgH614hqcqTlROKYGPV4La%2BvM2fgxAm1WNXjVOz0aTh%2B3PQ4ra%2BtvDz1NrRMR8jJqdDpDKJiyElwd%2BDt7c3169dZtWoVubm5XL16lWnTpjFp0iSjpBGgT58%2B/PXXX3z55ZcUFBTwxx9/sHnz5jL3ZW1tTWZm5h0ruapMHdf169cpKCjA9ua8xtjYWJbe/EZffDJb7dq18fb2ZubMmTz77LPUrFkTMO14CSGEEEKdnASnnSTAd1C9enXWrl3LgQMH6Nq1K08//TQ6nY7ly5fftm%2B9evUIDQ1l5cqVuLu788EHH/Dkk09iWcaKh7e3N5cvX6ZDhw5cvXq13B6DqeNq1qwZU6dO5Y033qBDhw4EBwfTv39/LC0tOXnypGG/QYMGkZiYyIABAwy3mXK8hBBCCKFOEmDtzIrKu%2BxYBV24cIGMjAxat25tuO39998nNTWVhRV4IsODOi5T3XJRiDKzsAB7e/00T5Vf2OvaKf58qdPp54hmZal/qqic8W5mpp%2BnmZurPgXi/Hm1OCsr/Ql0CQn3dwqEtbX%2BRLa4OLWfTlVfXNWq6a8scPiw%2Bs/cbdqYHqN1YjtASopanNZjrXrinqUlODrqx636036jRqbHuLrqT55zc1OfAtGypekxrVvD9u3Qt%2B/9nwLRpg3s2gW9e6tNgdizR61fa2t45BGIj1efAnHjhukxtrb65%2BjkSfUpEO3aqcVp9Mkn5d/mq6%2BWf5sPMqkAl4OMjAyGDBnCsWPHAP2JZDt37sTb21vGJYQQQohyJRVg7eQkuHLQunVrZsyYweTJk0lNTcXBwYHXXnvNcIKYjEsIIYQQ4sEhCXA5GThwoOGSYA%2BSB3VcQgghhFBTFSu25U0SYCGEEEKISkQSYO1kDrAQQgghhKhSpAIshBBCCFGJSAVYO6kACyGEEEKIKkUqwEIIIYQQlYhUgLWTBFgIIYQQohKRBFg7SYDFA0/1jV4cp3yRb9UVs6ysoGFDSE9XWxUN9KvJqfRbvz6kpan3q7ICXck4Cwu1g626EpxWqqs/FT/enBz1NmxsTI8xM9NvrazUV/urUeP/s3fnYVGV7QPHv%2Bzilkqg4pJJKJobLqBmKLilQCaimHtqZi64oLmWW0qm5gK%2BWpn6amm4piT5%2BitNyz1zyQz3FRAQNGXf5vcHMTGCxpzhzEjcn%2BvyOnLO3M99ZhjO3POc5zlHWVzecy5XLvfOXcaS95zNzJT9XYCyO7K98MLfS6V3%2B8t3C/kiK1cud3nzprJ4gEqVlMU9fPj38v59/eOtrZXltbL6e6n0fZ2Wpn9Mcby3RIklBbAQQgghRAkiPcCGk688QgghhBCiVJEeYCGEEEKIEkR6gA0nBbAQQgghRAkiBbDhZAiEEEIIIYQoVaQHWAghhBCiBJEeYMNJD7AQQgghhChVpAdYCCGEEKIEkR5gw0kBLIQQQghRgkgBbDgZAiFUFRcXR0pKiql3QwghhBBCSwpgE/Dy8qJx48a4urrq/Bs6dKipd62A48ePU79%2B/SI/fuHChbi6uuLu7s69e/fo2rUriYmJKu6hEEIIUbrk5BT/P7WlpKQwbdo03N3dadGiBe%2B99x7JT7nN%2BKxZs2jUqJFOnRQWFqbd/vnnn%2BPh4UGzZs0YOHAg165d02t/ZAiEicyZMwc/Pz9T70ax27BhA0uXLqVLly7cuXNHen%2BFEEIIwbx584iJieF///sf2dnZjB8/nsWLFzNr1qxCH//bb78xb948evbsWWDbzp072bhxI1988QW1a9dm6dKlBAYGEh4ejpmZWZH2R3qAn0EZGRksX76cjh074ubmxttvv83Nmze12%2BvXr09YWBhdu3aladOmjBw5kvPnz9O3b19cXV3p1auX9vEhISEMHDhQp30vLy927NgBwMCBA5k6dSqenp506NCBpKSkp%2B7brVu3GDlyJO7u7nh6erJ06VIyMjK4f/8%2Brq6uZGVlMWnSJCZPnoyPjw8APj4%2BREREFOdLJIQQQpRaJa0HODU1lfDwcAIDA6lUqRJ2dnZMmjSJHTt2kJqaWuDxGRkZXLp0iUaNGhXa3pYtW%2BjXrx/Ozs7Y2NgQFBREdHQ0x48fL/I%2BSQ/wM2jp0qUcO3aM9evX4%2BDgwOeff87QoUOJiIjAxsYGgPDwcMLCwsjIyMDb25tRo0axbt06qlevzrBhw1i9ejXBwcFFynfkyBG2bt2Kra0t5cuXf%2BLjUlJSGDJkCN7e3ixfvpzExEQCAwPJyckhKCiI06dPU79%2BfT7//HPc3d25c%2BcOHTt25Ntvv6VmzZpF2pe4uDji4%2BN11llY2GNv71CkeN043aXerK2VxVlZ6S6VMFfw3dTSUndpTIbm1miUxeX9jpT%2BripUUBZXtqzuUoki9lIUGqMkNo/S35Gp3l/FkbdhQ/1j6tbVXSpRrpz%2BMS4uuksllL6vnZ11l/pS%2BndYHL/jMmX0j/nr81S71FdamrK4YqBGwVrY56%2B9vT0ODkX7/E1LSyM2NrbQbampqWRmZlKvXj3tOicnJ9LS0rhx4wYNGjTQeXxkZCRZWVmsWLGCU6dOUaFCBXr16sXw4cMxNzfnypUrvP3229rHW1lZUadOHSIjI2ndunWR9lcKYBOZM2cOCxYs0Fl36NAhbG1t%2Bfrrr1mxYgW1atUCYPTo0WzZsoUff/yRrl27AjBgwAAqVaoEgLOzMw0bNsTJyQmA1q1bc%2BrUqSLvi4eHB1WrVv3Hx/34449kZGQwceJEzMzMqF69OuPGjSMwMJCgoKAi53uasLAwQkNDddaNHj2GwMCxitv862VSoIbinAAU8aBR7OztTZPXlLkdHZXF1aljWN4mTQyLV0rpBzYoKxTyq1zZsHhT5N25U3nskiXKYw2xaZNp8gKsWWOavKY6ftSurSzut9%2BKdz9MrLDP3zFjxjB2bNE%2Bf8%2BePcugQYMK3TZu3DgAyubrNLC1tQUodBzwo0ePcHNzY%2BDAgXzyySf88ccfjB49GnNzc4YPH05ycrI2Pk%2BZMmX0GnYpBbCJzJo1q9AxwAkJCaSkpDBu3DjM8/UCZmZmEhUVpf25Ur6qzsLCgueee077s7m5ORo9etSK%2Bu0uKiqKxMREWrVqpV2n0WjIzMwkISEBOzu7Iud8koCAALy8vHTWWVjYk5Cgf1sWFrnF74MHkJ2tf7xdWtQ/P6gwVla5xW9cHGRmKmtDaQ%2BwvT3Ex0NWlrK8ShmaOz1dWV5r69ziNzoaMjL0j797V1nesmVzi99z50DpOPdmzfSPMTPLLX7T05X3mj96pCzO0jK3CL1/37jvr%2BLIO2KE/jF16%2BYWv0FBoOfkGq18Q9eKzMUlt/jt1w8iI5XlNaQHeM0aGD4cLl/WP/7rr5XlLY5j1z8M3yuUjU1u8XvrlvJjkImo0QNc2OevvR5fStzd3bl48WKh2y5cuMDy5ctJTU2l3F9nRvKGPhR25vmVV17hlVde0f7cpEkTBg8eTEREBMOHD8fW1pa0x3rg09LStG0XhRTAz5jKlStjY2PD2rVraZbvA/LatWs6vbRFHuRtbk5mviIsJyeHBw8e6DymqG1Vq1aN2rVrs3fvXu26pKQkEhISqFKlSpHa%2BCcODg4FCvLYWMM%2Bb7OzFcYrKajyy8xU3oaSAjhPVpbh%2B27s3IZ%2B%2BGRkKGtDaTGYJyVFeRtKC9i8WKXxhhavWVnG/4JlaN4LF5TnvXZNefylS8rzRkbC6dPKYpWf9sp1%2BXLulzt9GXrcMeTYZchwhPR0kw5neFYU9vlbXF588UWsrKy4cuUKTZs2BeDq1avaoQuP%2B/7777l37x59%2B/bVrsvIyKDMX2ewnJ2duXz5Mp6enkBuJ%2BGNGzd0hlj8E5kE94wxNzfH39%2BfJUuWcPfuXXJycti5cyc%2BPj46E%2BGKysnJiYsXL3L58mWysrJYs2aN4iszeHp6kpyczJo1a8jIyODhw4dMmTKFCRMmFFpE541X/qeJdUIIIYQoupI2Cc7W1pZu3bqxePFiEhMTSUxMZPHixfj4%2BGiL2vw0Gg3BwcEcPXoUjUbD6dOn2bBhAwEBAQD06tWLL7/8ksjISNLT01myZAnPP/88LVu2LPI%2BSQH8DJoyZQpNmzalX79%2BtGzZkvXr17NixQoaKpjI0alTJ3x9fRkyZAivvvoq9%2B/fp0WLFor2q3z58qxfv57jx4/j4eFBp06dMDc3Z9WqVYU%2B/vnnn6dz584EBASwefNmRTmFEEIIoaukFcCQO/SzTp06%2BPr68tprr1GzZk0%2B%2BOAD7XZvb29Wr14NQOfOnZk2bRqzZ8/G1dWVyZMnM3bsWHr06AGAv78/Q4YMYfTo0bRu3ZoLFy7w6aefYqXH5HMzjT6DRYUwgSdMKv1HlpZgZwcJCcrOnFZNua4ssbU11KgBUVHGHQJhbQ3Vq0NMjPGHQBiaW%2BnpRxub3IlsN24oGwJx65ayvBUqQOvWcOyY8iEQ7drpH2NmljuJLS1N%2BRCIx4ZAFZmpxpgXR94OHfSPadgwd/Jcz57GHQLh6gq//grNmxt/CESTJnDwILRvr2wIxJkzyvIWx7Hr4UP9Y8qUyR33fPmy8mNQ48bK4gw0Y0bxtzl/fvG3%2BSyTMcBCCCGEECWIMXps/%2B1kCIQQQgghhChVpAdYCCGEEKIEkR5gw0kBLIQQQghRgkgBbDgZAiGEEEIIIUoV6QEWQgghhChBpAfYcFIACyGEEEKUIFIAG06GQAghhBBCiFJFeoCFEEIIIUoQ6QE2nBTA4pl38qSyuIoVwcMDfv9d2U2CunV7UVliwALIrlZDcbySG16ZmYE1kGFXXfFNwmyu/aEsMO9onJam7I5KSu58VxyU5s2LMzdX3oaSN6WlZe7dq5KTld8V7cYNZXHlyuXekS0mJje/vmrWVJbXzCx3mZ2t/DlnZuofk5crK0tZPCi7I1uFCn8vld7RTend/vLuavjokbI2lN7FLe93nJmpvA0l743s7L%2BXxry7oXgmSAEshBBCCFGCSA%2Bw4aQAFkIIIYQoQaQANpxMghNCCCGEEKWK9AALIYQQQpQg0gNsOOkBFkIIIYQQpYr0AAshhBBClCDSA2w4KYCFEEIIIUoQKYANJ0MghBBCCCFEqSI9wEIIIYQQJYj0ABtOeoCFEEIIIUSpIj3AQjXZ2dlER0dTq1YtU%2B%2BKEEII8a8hPcCGkwK4mHl5eREfH4%2Blpe5L6%2Brqytq1a020V0X3wQcfADB37tx/fOzAgQNxc3Nj7NixhW6fMGECzs7OT9wuhBBCCP1JAWw4KYBVMGfOHPz8/Ey9G4oUpfAtqvv37xdbW0IIIYQQxUXGABtZRkYGy5cvp2PHjri5ufH2229z8%2BZN7fb69esTFhZG165dadq0KSNHjuT8%2BfP07dsXV1dXevXqpX18SEgIAwcO1Gnfy8uLHTt2ALk9tFOnTsXT05MOHTqQlJSk89jjx4/Tvn17goKCaNmyJZ999hlTp05l6tSp2sds2LABT09P3N3dmTBhAmPHjiUkJES7/ebNmwwdOpRWrVrRsWNH9u7dC8CMGTP45Zdf%2BPTTTxk5cmTxvohCCCFEKZaTU/z/ShvpATaypUuXcuzYMdavX4%2BDgwOff/45Q4cOJSIiAhsbGwDCw8MJCwsjIyMDb29vRo0axbp166hevTrDhg1j9erVBAcHFynfkSNH2Lp1K7a2tpQvX77A9rt371K3bl0%2B%2Bugj0tPT%2BfDDD7Xb9uzZQ2hoKKtXr6Zx48Zs2bKFuXPnUq9ePe1jDh8%2BzJo1a2jQoAGrVq1i2rRpdOzYkfnz53Pr1q2nDpEoTFxcHPHx8TrrMjPtsbNzKHIbefKebiFP%2B5lnZqY8RkmsVpkyyuKsrXWX%2BlK604bmrVBBWVzZsrpLJSwVHH4tLHSXSpQrpyzO1lZ3qS8rK2Vxea%2BTktcrz8sv6x/j5KS7VOLhQ/1jnJ11l0o8eqQszsVFd6mvvz7D9Jb33lD6HgHIztY/Jm9/le53aqqyOPFMkAJYBXPmzGHBggU66w4dOoStrS1ff/01K1as0E4MGz16NFu2bOHHH3%2Bka9euAAwYMIBKlSoB4OzsTMOGDXH66yDcunVrTp06VeR98fDwoGrVqk99jL%2B/P1ZWVlg9dvDZtm0bAQEBNG/eHID%2B/fuzc%2BdOncd0796dl//6cOnevTsrVqwgISGBatWqFXkf8wsLCyM0NFRn3ejRY%2BjZU/k44r923%2BgMqVEMiTXkM4QXXzQgGKhRw7B4pRwdlcXVqWNY3kaNDItX6q/jgyJ2doblNqQwM8TzzyuPDQ9XHrtsmfJYQ6xZY5q8AJs2mSZv9eqmyav0uHf6dPHuhx5KY49tcZMCWAWzZs0qdAxwQkICKSkpjBs3DnPzv0efZGZmEhUVpf25Ur4PNwsLC5577jntz%2Bbm5mg0miLvi4PDP/ecPukxMTEx2qI8z%2BNXdMi/r3kFdFZWVpH373EBAQF4eXnprLt0yZ5Dh/Rvq3z53OL311/hsdEfRfLKK/rH5LGwUNYhkUdJrJlZbvGbmQl6vEV0WEddVxhonVv8RkVBRob%2B8Yb0ADs6QnS0srxxccryli2bW/yePw8pKcraUNKzaGGRW/w%2BeKD8DZbvWKMXW9vc4vfyZWU9X0U4FhXK0jK3%2BL13D5QeW95%2BW/8YJ6fc4nf8eLh6VVlepT3Aa9bA8OG5r7UShvQAb9oE/fpBZKT%2B8d98oyyvlVVu8RsTk3sAU0LJ36GNTW7xe/06pKcry2siUgAbTgpgI6pcuTI2NjasXbuWZs2aaddfu3ZNp5fWrIjFgLm5OZn5DhY5OTk8ePBA5zFFaetJj6lRowbR0dE666Kjo6lbt26R9k8JBweHAgX59evKPkfyJCUZFm8KSgvYvFjF8WlpyhNDbhGqpA1zA6cjZGQo%2BwBTWijkSUlR3oYBXxTJzlYen5ysPC/kFr9K2lBa2OTJylLexu%2B/K8979aryeEMmAl%2B%2BDOfOKYt97HNAb5GRyno3DS0iMzOVt2HIcIT0dBnOUArJJDgjMjc3x9/fnyVLlnD37l1ycnLYuXMnPj4%2BOhPhisrJyYmLFy9y%2BfJlsrKyWLNmDSlKe6MK0adPH7Zs2cK5c%2BfIyspi%2B/btnDlzpsjx1tbWPDK0wBBCCCGEDpkEZzjpATayKVOmEBISQr9%2B/Xjw4AG1atVixYoVNGzYUO%2B2OnXqxJEjRxgyZAg5OTm88cYbtGjRotj2tWvXrty6dYtRo0aRkZGBh4cHjRo1KjBW%2BEneeOMNZs%2Bezfnz59lkqjFlQgghhBCPkQK4mO3fv/%2Bp221sbJg0aRKTJk0qdPvFixd1ft64caPOz/mvqGBpacncuXOfeO3ex2Mf5%2B7uXiDfRx99pP1/ZGQk3bt35%2B184%2Bf8/PyoUqVKoe3XrFlTpz1fX198fX2fug9CCCGE0E9p7LEtbjIEQjzRsWPHGDlyJPHx8Wg0GiIiIrhy5Qpt2rQx9a4JIYQQpZYMgTCc9ACLJxowYABRUVH07NmT5ORk6taty6pVqwpcCUIIIYQQoiSRAlg8kaWlJTNmzGDGjBmm3hUhhBBC/KU09tgWNxkCIYQQQgghShXpARZCCCGEKEGkB9hwUgALIYQQQpQgUgAbToZACCGEEEKIUkV6gIUQQgghShDpATacmUaj0Zh6J4R4KgW3iQbA2hqqV4eYGMjI0Dv8299eUJS2YkXw8IBDh%2BDhQ0VNmCyvzwRnZYENG8KuXdCjB1y4oH98drayvC%2B/DOHh4OsLv/%2Buf7zSw9/LL8O334KPj7K8AJUr6x/j4gKbNkG/fhAZqSxvWpqyuAYNYPt26NUL/vhD/3gLC%2BV5t2yBPn2U5QXYsUP/GBsbqF0bbt2C9HRlea2tlcUYcNwClMcZ%2Bpzr1VOW19UVfv0VmjeH06eVtVGzpv4xjRrBd99Bt25w/ryyvLdvK4szkLd38be5Z0/xt/kskx5gIYQQQogSRHqADScFsBBCCCFECSIFsOFkEpwQQgghhChVpAdYCCGEEKIEKYk9wCkpKcybN4/9%2B/eTlZVFx44dmTVrFuXKlSvw2A8%2B%2BIDw8HCddWlpabRt25YvvviCnJwcWrRogUajwczMTPuYw4cPU7Zs2SLtj/QACyGEEEIIVc2bN4%2BYmBZf8C0AACAASURBVBj%2B97//sW/fPmJiYli8eHGhj507dy6nT5/W/gsJCaFixYpMnToVgCtXrpCZmcmJEyd0HlfU4hekABZCCCGEKFFycor/n5pSU1MJDw8nMDCQSpUqYWdnx6RJk9ixYwepqalPjU1MTGTSpEnMmDEDZ%2BfcKxX99ttv1K9fH2slV1r5iwyBEEIIIYQoQdQoWOPi4oiPj9dZZ29vj4ODQ5Hi09LSiI2NLXRbamoqmZmZ1Mt3qTwnJyfS0tK4ceMGDRo0eGK7ixcvplGjRrz%2B%2Buvadb/99hvp6en06tWLqKgonJycCAoKonnz5kXaV5ACWAghhBCi1AsLCyM0NFRn3ZgxYxg7dmyR4s%2BePcugQYMK3TZu3DgAnSEKtra2ACQnJz%2Bxzdu3b7N79262bt2qs75MmTI0adKEcePG8dxzz/HVV18xbNgwdu/eTa1atYq0v1IACyGEEEKUIGr0AAcEBODl5aWzzt7evsjx7u7uXLx4sdBtFy5cYPny5aSmpmonveUNfShfvvwT29y%2BfTuurq4FeojzxgLnGTZsGDt27ODgwYMMGDCgSPsrBbAQQgghRCnn4OBQ5OEO%2BnrxxRexsrLiypUrNG3aFICrV69iZWVFnTp1nhi3b98%2Bhg4dWmD90qVL6dq1Kw0bNtSuy8jIwMbGpsj7JJPghBBCCCFKkJI2Cc7W1pZu3bqxePFiEhMTSUxMZPHixfj4%2BFCmTJlCY%2B7fv8/Vq1dp1apVgW2XLl1i/vz5xMfHk5GRQWhoKElJSXTu3LnI%2ByQFcDHx8vJiRyH3m9%2BxY0eBUwqmFhISwsCBAw1uZ/jw4axevboY9kgIIYQQRVXSCmCAWbNmUadOHXx9fXnttdeoWbMmH3zwgXa7t7e3Tk1x584dAKpWrVqgreDgYGrXrk2PHj1wd3fnxIkTrFu3jkqVKhV5f2QIhFBszZo1pt4FIYQQQpQA5cuXZ968ecybN6/Q7Xv27NH5uXHjxk8cU1ypUiWCg4MN2h/pATaSDz74oMA4lrlz5/Lee%2B9x584d6tevz8aNG3nllVdo0aIFkydPJikpSfvYPXv24OvrS4sWLfDz8%2BPnn3/Wbhs4cCBTp07F09OTDh06cPHixX9sL49Go%2BGzzz7D19eXli1b0qpVK4KCgkhLSwPg8uXL9O/fn1atWuHp6cmUKVO07QwcOJCQkBAAkpKSmDlzJl26dKFZs2a8%2Buqr0jsshBBCqKAk9gA/a6QHuBjNmTOHBQsW6KzLzMzEzs4Of39/AgICiI2NpWrVqmRkZLBnzx6WL1%2Bufey%2BffsIDw8nOzub0aNHM2fOHBYtWsTBgweZNWsWq1atonnz5hw6dIixY8eyZcsW7UWhjxw5wtatW7G1teXhw4dPbS%2B/7777jg0bNvDll19Sp04drl69Sr9%2B/QgPD6d3797MmTOHNm3a8OWXX3L//n0GDx7M1q1beeutt3TaWbx4MXfu3GHbtm1UqFCBffv2ERgYSLdu3XjhhReK/BoWeh3C7Gwc9JiJqmVpqbvUU8WKisLIm9D6lImtqiiWvPkmFOilbl3dpb6UHn2dnHSX%2BtJoTJMXlL3B8iaLPGXSyD9KT1cW9%2BKLukt9WViYJi%2BAHhNjtKysdJdKKIk18LgFQL5bw%2BrF0Ofs6qoszsVFd6lEIafJ/5Ghf8fnzyuLKwalsWAtblIAF6NZs2bh5%2Bens27Hjh2EhobSpEkTnJyc%2BPbbbxk2bBg//vgj5cuXx93dnaioKACmTZtGlSpVAAgMDOTdd99l/vz5fPnll7z55pvageCenp54eXnx9ddf8/777wPg4eGhHSeTVwA/qb38PDw8aN68OdWqVSMxMZH79%2B9TqVIl7cWsbWxs%2BOmnn3BycqJNmzbs2rULc/OCJw7Gjh2LhYUF5cuX5%2B7du9qZmHFxcXoVwIVeh3D0aMYGBha5jQKUFM%2BAR3XlKQH0uB53sTIor8cuw5IvXWpYvFLLlpkmb74vsEb12Bdto3rCrUtVt3ChafJWN/BAoJTC41axUPqcf/3VsLybNhkWr9RjnzlFVsTrzYpnkxTARuTn58c333yjvV5dz549Mcv3TT1/oVi9enUyMjJ48OABUVFRnDhxgs2bN2u3Z2dn07p1a%2B3PhV265Ent5afRaFi6dCkHDhygSpUqNGjQgMzMTDR/9YwtW7aMkJAQli5dysSJE2nevDmzZ8/W9jznSUhIYP78%2BVy4cIGaNWvSqFEjAHL0/Jpa6HUIs7MhJkavdoDcHhR7e4iPh6wsvcMPXVb2IVC%2BfG4R%2BuuvUMioE9UUR16PJT2UBdatm1v8TpgA167pH29ID/CyZTB%2BPFy9qn%2B8IT3Ay5fDuHHK8oLyHuAFC2D6dLhxQ1leQ3qAFy%2BGSZPg%2BnX94w3pAV64EKZMUZYXlBXtVla5hWBMDGRmKsurtAfYgOMWYNj%2BGvKc33hDWV4Xl9zit18/iIxU1obSHuDQUBgzRvnfsYlID7DhpAA2oh49evDJJ59w%2BvRpDh8%2BrDP7ESA2Npa6f51CvnPnDra2tlSuXJlq1arxxhtvMGLECO1jo6OjdS4dYlbIKa8ntZff4sWLiY6OZv/%2B/dqLUfv6%2BgK5xeuFCxcYO3Ys06dPJyYmhuDgYKZOncr27dt12hk3bhxeXl588cUXWFpacv/%2BfbZs2aL3a1TodQhv3oSMDL3b0srKUhT/V0e6YklJhrdh9LwXLhiW/No1ZW1kZxuW9%2BpV%2BP13/eOUFsCG5gV47G9RLzduKC8U/hrfr9j16/DHH/rHKS2ADc0Lyot%2ByC0ElcYb8v5SeNwCDDtegvLnfPq0YXkjI5W3UbOm8rxXr5p0OIMwDZkEZ0R2dna0b9%2BeuXPn0rJlSxwdHXW2L1myhKSkJGJjY1mxYgU9evTAysqKPn36sGHDBs6dOwfk3gPbz8%2BPb7/99qn5ntRefklJSdjY2GBhYUF6ejpr167l0qVLZGZmYm5uzocffsiyZctIT0%2BnSpUq2NjYFCiiAR49ekSZMmWwsLAgMTGRDz/8EMgdAy2EEEKI4iOT4AwnBbCR%2Bfn5ceHCBXr16lVgW%2B3atfHx8eH111/H1dWV6dOnA/Daa68xceJEpk%2BfTvPmzRk3bhxDhgz5x2v5Pqm9/MaPH09aWhpt27bFy8uLM2fO0KNHDy5dugTkDoG4evUq7dq1o23btjx69KjQS5gEBwcTERFB8%2BbN8fPzo2rVqjRs2FDbjhBCCCGKhxTAhpMhEMVk//79ha738/PTmRhXo0YNKlasWOjdSvr378%2BUKVOK1E5%2BGzduLHT9k9obO3as9v%2B1atXiyy%2B/LDQewMnJifXr1/9j3ldffZXvvvvuie0IIYQQQjwrpAA2kqSkJKKjo1m2bBl%2Bfn563a9aCCGEECJPaeyxLW4yBMJI7t69S0BAAH/%2B%2BSejRo0y9e4IIYQQQpRa0gNsJC%2B99BKnnzC7tWbNmk%2B83Z8Sxd2eEEIIIZ4d0gNsOCmAhRBCCCFKECmADSdDIIQQQgghRKkiPcBCCCGEECWI9AAbTnqAhRBCCCFEqSI9wEIIIYQQJYj0ABtOCmAhhBBCiBJECmDDSQEsnn3lyyuLs7DIXdragrW14nClcRYWytuwVPCXmfcUra1B8X1W7O2VxVWu/PdSSRsxMcryajR/L/P%2Brw%2BlnyL58ypto0wZ/WPy/5KVxIOy1%2Bnx3EreYAr%2BBoG/n2eZMlC2rLI2UlL0j8l7ndLSIDVVWd60NP1j8p5vUpKyeICsLGVx2dm5y5QUZc%2B5Zk1leatW/XuptI07d/SPyTtWxcYqixclmhTAQgghhBAliPQAG04mwQkhhBBCiFJFeoCFEEIIIUoQ6QE2nBTAQgghhBAliBTAhpMhEEIIIYQQolSRHmAhhBBCiBJEeoANJz3AQgghhBCiVJEeYCGEEEKIEkR6gA0nBbAQQgghRAkiBbDhZAiECtLS0rh7966pd0MIIYQQQhSi1BXAH3zwAa6urri6utK4cWNcXFy0P7u6uvLLL78YnKNv374cP34cgCNHjtCwYUO94r///nv69u2Lq6srzZs3p1evXuzatcvg/VJi69atdO7c%2BYnbJ02axIwZM4y4R0IIIUTplpNT/P9Km1I3BGLu3LnMnTsXgB07dhAaGsr%2B/fuLNUdiYqLi2BMnTvDee%2B%2BxfPly2rZti0aj4aeffmLixIlYWlri7e1djHsqhBBCCFH6lLoe4KLIyclh/fr1dO3alZYtWzJgwAB%2B//137fakpCRmz56Nh4cHbdq0ISgoiISEBAAGDRpEXFwcM2fOZP78%2BdqYzz//nE6dOtGsWTPGjRtHUlJSobl//fVXHB0dadeuHRYWFlhaWuLp6UlQUBDp6enax%2B3atQtvb29cXV3p3r07//vf/7TbwsLC6NatG82bN8fX15c9e/Zot7355ptMmzaNDh064OXlRUpKCt9//z0BAQG0adOGpk2bMnDgQG7duqWNyc7OZsGCBbRp04ZOnTqxbt06NBpNofu/e/dufH19adGiBX5%2Bfhw5ckTPV18IIYQQTyM9wIYrdT3ARbFx40Y2bNjAqlWrqFu3Ljt37uStt95i7969VKlShSlTppCRkcE333yDtbU1CxYsIDAwkK%2B%2B%2BooNGzbg4eFBUFAQPXr04MiRI2RnZxMXF8eePXtITEykd%2B/efP311wwfPrxAbi8vLz799FPefPNNunbtStOmTXn55ZcZMGCA9jFHjhxh5syZrFy5knbt2nHo0CHGjBmDs7Mzp06dYtGiRaxcuZKWLVty7NgxxowZg62tLV5eXgAcPXqUsLAwbG1tuX//PuPHj2flypW0b9%2BexMRERo0axapVqwgODgYgKioKW1tbDh48SGRkJMOHD8fe3h4fHx%2Bdff/hhx%2BYN28eq1evplmzZvz444%2BMHj2abdu24eTkVKTXPi4ujvj4eJ119tbWONjb6/U7BMDCQnepp4oVFYVRrpzuUgklu1y2rO5SkXr1lMW98ILuUl9VqiiLy3tfFfH9VcATvsipnhdAyXva0NcZIN8Xab3UqaO71JeVlbK44njOtrb6x9jY6C6VMDMzTd7sbGVxhuZu1EhZnKn%2BnlxcdJf6On1aWVwxKI0Fa3GTArgQmzZt4t1336V%2B/foA9OnThy1bthAeHs5rr73G999/z//93/9R5a8P7enTp%2BPm5kZkZCQuT/hDCgwMxMbGhurVq9OiRQudHtb86tWrx65du/jyyy/Ztm0bCxcuxNrams6dOzN9%2BnTs7OzYuXMn3bp1w8PDA4AOHTqwadMmHBwc2L59O/369cPd3R2AV155hT59%2BhAWFqYtgNu3b0/VqlUBKFOmDBEREdSuXZukpCTu3r1LlSpViI2N1e7T888/T2BgIBYWFjRp0gR/f3927dpVoAD%2B6quv6N%2B/Py1atACgY8eOeHh4EBYWxvTp04v02oeFhREaGqqzbszo0YwNDCxSfKEqVFAU9sorylMCNGtmWLxSTZoYENx6vWHJ58wxLF6p5ctNk3fFCtPkNdXrDPDXF2OjmzfPNHkNKbwNUbu2afICvPiisrjvvjMs72PHfqPZtElZnJIvOOKZIQVwIaKioliwYAELFy7UrsvKyiI6OpqoqCgA/Pz8dGIsLS25c%2BdOoQWwhYUFFfIVYVZWVmQ/5Rt67dq1tQXjw4cPOXHiBJ988gnjx49n48aNxMfH0%2Byx6qrJX1XPvXv3qFWrls62mjVrcvjwYe3PDg4OOvuye/duwsLCsLCwoF69ejx8%2BJAyZcpoH1O9enUs8nVJOjo68vPPPxfY76ioKE6dOsWXX36pXZednU27du2e%2BFwfFxAQoC3U89hbW8ODB0VuQ8vCIrf4ffRIUY/I4d8r6Z%2BT3J7fZs3gzBlITlbUhOIe4CZN4Nw5SElRlrf16iHKAl94IbcomzULbt7UP/7ePWV5nZxyi99x4%2BDqVf3jDekBXrECAgOV5QXlPcCGvM5gWA9wcDBMmwY3bugfb0gP8Lx58P77yp/zzJn6x9jY5Oa%2BeVP5a6a0B7h2bbh1S3leQ3qAX3wRrl9XlnvCBGV5nZxyi98xY5T/PeXrtCkyF5fc4rdfP4iMVJbXRKQH2HBSABeiatWqTJ48mddee0277tatW1SuXJlHjx4BsG/fPm0PsEaj4erVq9Quhm/sffv2pVWrVgQFBQFQsWJFOnXqhEajYdq0aUBuQRoTE6MTt2bNGlq2bEmNGjUK9C7funUL%2B3wftmb5Dsrh4eF8/fXXbN68WVs4z5o1i5v5PmgeH5Jw%2B/ZtatSoUWDfq1atSp8%2BfRg2bJh2Xd7wiaJycHDQKdABSEhQfkCH3FgF8Q8fKk8JucWv0jYsDfjLTEnJrfkVuXRJeWLILRaUtPHY%2B1lvV69CvnH6RWbop8jVq3D%2BvLLYQv6Gikzp6wyQmqo8L%2BQWv0qKBWtrw/LevAkXLyqLNeQ5p6crjzc3YJpNejqkpSmLzcpSnjcvt5LnrPRvIY8hf0937ijPGxlp0uEMwjRkElwhAgIC%2BM9//sP169cBOHjwIN27d9eZoDZ//nwePHhAZmYmK1eupHfv3tri2MbG5omT3P6Jr68vmzZtIjw8nMTERHJycrh27RobN26kS5cuAPTs2ZO9e/dy5MgRcnJyOHjwICtXrqRChQr07t2bzZs3c/z4cbKzszl69Cjbtm2jV69eheZ79OgR5ubm2NjYaNsKDw8nMzNT%2B5i7d%2B/y2WefkZGRwalTp9i2bRt9%2B/Yt9HX773//y/m/DmDnzp3Dz8%2BP7ww9LSaEEEIILZkEZzjpAS5EXg/mO%2B%2B8Q3x8PNWqVWPOnDm0b98egCVLlrB48WJef/11kpOTqVevHl988QV2dnYA9O7dm0WLFvHbb7/x%2Buuv65W7f//%2BVKhQga%2B%2B%2BorZs2eTlZVFtWrV8PX1ZcSIEQC4ubkRHBxMcHAwUVFR1KxZk2XLluHk5ISTkxPJycnMmTOHmJgYqlWrxvTp0wuM183j7%2B/P6dOn6d69OxYWFjg5OTFo0CDCwsK0RXDDhg25du0a7u7u2NvbM336dO1rkZ%2B3tzcpKSlMmTKF6OhoKleuzLBhw%2Bjfv79er4EQQgghnqw0FqzFzUzzpOtZCfGs%2BOsSc3qzsIBKlXLHDysYAvHdCTtFaStWzJ1Ad/iwcYdAVKgArVvDsWPKh0B0ntVWWWC9erB%2BPQwZYtwhEC%2B/DN9%2BCz4%2Bxh0C0agR7NkD3t7GHQJh6OsMyk/nu7jA5s3w5pvGHQJRvz5s2ACDBikfAvHZZ/rH2Nrmvt6XLhl3CESZMuDsDJcvG38IhK1t7u85MlLZc9azw0erUaPcCXTduhl3CISrK/z6KzRvrnwIhIlKKIVzu59K8dC5Ekp6gIUQQgghShDpATacjAEWQgghhBClivQACyGEEEKUINIDbDgpgIUQQgghShApgA0nQyCEEEIIIUSpIgWwEEIIIUQJUpKvA5yamkpAQAA7dux46uPOnj1L7969cXV1xcvLi61bt%2Bps37lzJ507d6ZZs2b4%2BflxWs8reUgBLIQQQghRgpTUAvjy5cv079%2BfM2fOPPVxf/75JyNGjOCNN97g5MmTzJ8/n%2BDgYM6dOwfA8ePHmTdvHh999BEnT57k9ddf59133yVVj8v3SQEshBBCCCFUdfToUQYPHkzPnj1xdHR86mP37dtHpUqV6N%2B/P5aWlrRp0wZfX1%2B%2B%2BuorALZu3Yq3tzctWrTAysqKIUOGULlyZSIiIoq8PzIJTgghhBCiBFGjxzYuLo74%2BHiddfb29jg4OBQpPi0tjdjY2EK32dvb4%2BLiwoEDB7CxsWHdunVPbevy5cvUq1dPZ91LL73Etm3bALhy5Qq9evUqsD1Sj5v0SAEsnn12yu7IFhcXR1hICAEBAUX%2BA86vWzdFaYmLiyMkJExxXqWKJW/nI4pzh4WEEPDxx0Z/zmEhIQSsXWuavOvWmSavkV9nndzLl5vmOS9ebJq8AQE4PPZBbLS8zs5Gy1sgt5LX%2BvZtw/L%2B97%2Bm%2BR3v3Wv0vydDqXEDupCQMEJDQ3XWjRkzhrFjxxYp/uzZswwaNKjQbStXrqRTp05F3pfk5GRsbW111pUpU4aUlJQibS8KGQIh/rXi4%2BMJDQ0t8I1W8v57ckvef39uyfvvz13a8j6r8iam5f8XEBBQ5Hh3d3cuXrxY6D99il8AW1tb0h67FXhaWhrlypUr0vaikB5gIYQQQohSzsHB4ZnpCa9Xrx6HDx/WWXflyhWc/zor4uzszOXLlwts9/DwKHIO6QEWQgghhBDPjM6dO3Pv3j3Wr19PZmYmx44dIzw8XDvu19/fn/DwcI4dO0ZmZibr168nISGBzp07FzmHFMBCCCGEEMKkvL29Wb16NQCVK1dm7dq17N27F3d3d2bOnMnMmTNp3bo1AG3atGHWrFnMnj0bNzc39uzZw%2Beff06lSpWKnM9i9uzZs9V4IkI8C8qVK4ebm5te44Ikb8nKLXn//bkl778/d2nLW9oNHjyYBg0a6Kzr378/LVu21P5ctWpV/P39eeeddxg0aFCBx7u4uDBgwABGjhxJnz59qFatml77YKbRqDGXUAghhBBCiGeTDIEQQgghhBClihTAQgghhBCiVJECWAghhBBClCpSAAshhBBCiFJFCmAhhBBCCFGqSAEshBBCCCFKFSmAhRBCCCFEqSIFsBBCCCGEKFWkABZCCCGEEKWKFMBCCL398ssv5OTkmCz/hQsX2LdvHxkZGSQkJJhsP9T27rvvkpSUZOrdKHUSExONluu7774rdH1YWJjqud99991C1w8YMED13EKYmqWpd0CIki4zM5OIiAiioqIKFIVjxoxRLe/t27dZvXp1oXk3bNigWl6A0aNH8%2BOPP2Jra6tqnsclJCQwevRozp8/j5WVFdu2bcPf35%2B1a9fi6uqqau6MjAwOHjxIVFQUAQEB3Lx5ExcXF1Vznj59Gmtra1VzPM3hw4fZuHEjcXFxfPrpp6xdu5agoCAsLdX96NiyZYs2786dO/noo48IDg6mXLlyquXMysoiJCSEL7/8kuzsbMLDwxk/fjyrVq3CwcGhWHOlpqZy//59AKZPn06zZs3QaDTa7Y8ePeKjjz4iICCgWPMC3Llzh2%2B%2B%2BQaAn3/%2BmdDQUJ3tSUlJXLx4sdjzPiuuX79OWFgYMTExzJkzh%2B%2B%2B%2B44333zT1LslTEAKYPGvk56ezp9//kmlSpWMUjwEBQVx/PhxnJ2dMTMz067P/381TJw4ESsrK1q3bo25uXFP5tSqVYvffvsNNzc3o%2BZdsGAB9erVY926dXh4eODk5MSIESP4%2BOOP2bx5s2p5b926xdChQ8nMzOThw4e0b9%2BeXr16ERoaiqenp2p5fXx8CAwMxNfXF3t7e533VKtWrVTLCxAeHk5wcDC9e/fm5MmTAOzfvx8zMzPee%2B891fKuX7%2BezZs3M2zYMD7%2B%2BGPKlStHXFwcwcHBfPjhh6rlDQkJ4dixYyxfvpwJEyZgZ2dHtWrVmD9/PsuXLy/WXElJSXh7e5OWloZGo8HLy0u7TaPRYGZmRqdOnYo1Zx5HR0cuX75MYmIi2dnZHD9%2BXGe7jY0Ns2bNUiW3qR09epTRo0fj4eHBTz/9REpKCsuWLSMlJYVhw4aZeveEsWmE%2BJf45ZdfNH379tU0aNBA4%2BLionn55Zc1AwYM0Jw9e1bVvK6urprbt2%2BrmqMwzZo106Smpho9r0aj0QwdOlTTsGFDTZcuXTQDBgzQDBw4UPtPTW3bttWkpKRoNBqNplWrVhqNRqPJyMjQtGzZUtW8I0aM0KxcuVKTk5OjzbVjxw7NG2%2B8oWre%2BvXrF/rPxcVF1bwajUbj4%2BOjOX36tEaj0Wif8/Xr1zWvvvqqqnm7dOmiuXLlikaj%2Bft3HBsbq2nbtq2qeT09PTV3797Vyfvnn39q3NzcVMl37949ze3btzXNmjXT3LlzR%2BdffHy8KjkfN2PGDKPkeZJbt25pTp48qTlx4oTmxIkTmsOHD2vWrVunWr5evXpp9u/fr9Fo/n5Pnz17VtOxY0fVcopnl/QAi3%2BFU6dO8dZbb9GlSxcGDBhA5cqVSUhIYP/%2B/QwePJhNmzbRoEEDVXLb29tTqVIlVdp%2BGhcXF%2B7evUudOnWMntvV1VX1IQeFsbKyIi0tDVtbW%2B0p4%2BTkZFVPjQOcOXOGkJAQzMzMtL2wPXr0YP78%2BarmjYyMVLX9p7l79y5NmzYF/j6b8cILL5CSkqJq3vv37/Piiy8CaH/HdnZ2ZGVlqZo3JSWFKlWq6OQtU6aMamdXvL29OXbsGJ07d6ZGjRqq5PgnH374IdnZ2dy7d4/s7GydbY6Ojqrm/vTTT1m6dKn2vaX5q%2Be7QYMGDBkyRJWcN27coEOHDsDf7%2BkmTZpoh6OI0kUKYPGvEBISwrvvvltgUoevry%2BhoaGsWrWKFStWqJJ7ypQpjBs3jn79%2BlGxYkWdbWqepp45cyZDhgyhS5cuBfKqOfbYGO0/iZeXF5MnT2bmzJmYmZmRkJDAhx9%2BSPv27VXNW6FCBe7du6dTFMTHx/Pcc8%2BpmhdMM/YYoE6dOvzwww86p%2BKPHDnCCy%2B8oGpeFxcXwsLCePPNN7VFSkREBM7OzqrmbdasGaGhoUyYMEGbd%2BPGjTRu3FiVfBkZGXz//ffs27ePPn366IwBzqP2MJd9%2B/YxdepUUlNTtevyCtE//vhD1dybNm1ixYoVWFtbs3//fiZOnMi8efOoXr26ajmrV6/O2bNnadasmXbd77//rmpO8ewy0xT2VydECePm5sb%2B/fspX758gW0PHz7Ex8eHQ4cOqZJ76dKlfPrppwXWq/0hMnLkSH799VecnZ11eqnMzMxUnwQHppmolJyczLRp09i3bx%2BQ%2B1zbt2/PokWLqFChgmp5ly9fzsGDBwkKCmLcuHGsXbuWRYsW4erqysSJE1XL%2B/jY4x07duDj46P62GPILXZHjRpFx44d%2Bf777%2BnZsyfffvstS5YsUfULx%2B%2B//86QIYUmoQAAIABJREFUIUNwcnLi/PnztGnThjNnzrBmzRptj7Qabt26xZAhQ8jKyiIhIYEXXniB5ORk1q1bR926dYs938KFC9m4cSPZ2dmFFr/GKEI7deqEn58f3bt3x8rKSmeb2r3Srq6unD59mrt37zJq1Ch27NhBYmIi/v7%2B7N%2B/X5Wcu3fvZv78%2BfTv359169YRGBjIf//7X8aOHUuvXr1UySmeXVIAi3%2BFvIPpk7Ro0YJTp06pkrtVq1YsWbKEdu3aGXUymqurK//3f//H888/b7SceR6fqPTDDz8wYsQInJ2dVZ2olCcxMZE7d%2B5QrVq1Yp%2BhX5jMzEw%2B%2BeQTvv76a1JTU7GxscHf358pU6aoOtHynXfeoWnTprz77ru4ublx8uRJdu7cyYYNG9i5c6dqefNERkYSFhZGVFQU1apVw9/fnyZNmqieNzY2lt27dxMdHU21atXw9fVV/ZQ85F6d4cCBA9q8HTp0KPRLdXH6p2OXmtzc3Dhx4oRJcnft2pXt27dTrlw53N3dOX78OGZmZqoeqwF%2B%2BOEHNm3apD1%2BBAQE0L17d9XyiWeXFMDiX6F58%2Bb8%2Buuvircbol27dhw8eBALCwtV2n%2BSrl27sm3bNlV7Pp%2BW%2Bz//%2BQ9OTk7aD9G4uDh69uzJ4cOHVcubd/mmx1lZWVGlShWaNWum%2BqXZEhMTqVy5supX%2BQBwd3fnp59%2BwtraWvs65%2BTk4Obmxi%2B//KJ6/tJk3rx59O7d2yjDS/J78OCBdg5BYmKidhyyMQwbNozJkycb/TlD7hCu6Oholi1bRmBgII0bN8bGxoaIiAgiIiKMvj%2Bi9JExwOJfQaPREBMTU%2BipxLztannrrbdYvHgxI0eONMqY0DzDhg1j1KhRDBo0iOeee86ol8gy1USlsLAwzpw5g52dHTVq1CAmJob4%2BHiqVatGamoqZmZmrF27ttgnPD5%2BrdQ81tbWVK5cmbZt26pyytiUY4%2B9vLwKLfLzvmx4enoybNiwYj/r4eLiUmheS0tLbd6pU6dSpkyZYs2bkJBAQEAATk5O9O7dGx8fH6N8uSxfvjxLly7VXn949%2B7dTJgwQZXrD%2BfJez9XqVKFYcOG0a1btwITedUe5z916lSWLFlCVlYW06dPZ/z48Tx69Ijg4OBiz/X%2B%2B%2B//42PmzZtX7HnFs00KYPGvkJqaipeX1xMLXTV767766iuio6NZv359gW1qjuH74IMPALTXaM1jjLGDppqoVL9%2BfVq1asX48eO1hVdoaCh//vknM2bMYO3atQQHBxf7GOhLly6xb98%2BGjduTK1atYiOjubMmTM0btyY7Oxs5s%2Bfz6pVq2jTpk2x5vX19WXMmDEEBQWRk5PDuXPnWLRoEd7e3sWapzB9%2BvRhy5YtDB8%2BnFq1ahEVFcXatWtp27YtdevWZdOmTaSlpTF27NhizTt16lR27drF%2BPHjtXlDQkJwc3OjRYsWrF27lsWLFzNz5sxizbts2TIePXpEeHg4O3fuZOHChXTt2hV/f39Vv1A%2Bfv3h559/XrXrD%2BfJf%2B3funXrFrjxhTHObpQvX157veEqVaqo2uublpamWtui5JIhEOJfISoq6h8fo9akjqeNoTP2jSKMxVQTldq1a8eBAwd0JuxkZmbi6enJzz//TFZWFq1bty724QETJ06kZcuW9OvXT7tu%2B/btHD9%2BnI8//piIiAjWrVvH1q1bizWvqcYeA/Ts2ZOPP/5Y50vNtWvXmDRpEjt27ODOnTsMHDiQAwcOFGve7t2788UXX%2BjMzI%2BNjeWtt94iIiKChIQEevTowc8//1yseR939OhRZsyYQUxMjKpfKL28vNi8eTNVq1bVDnN5%2BPAhnTt3LnCTin%2BDzz77jBEjRjzxrAqY7iozonSRHmDxr1CUCUFqHVRNVeRGR0c/cZvaE4Zefvllvv32W8LDw2nQoAHVqlVjzpw5RpmodPv2bZ1Z%2BVFRUdqhF2lpaQVmsxeHI0eOsGjRIp11b7zxBh9//DEA3bp1K9JpVn1ZWVkxZcoUpkyZYtSxxwA3b94scI3pWrVqcf36dQBq1qzJw4cPiz1vbGxsgXGwzz33HDExMUBub6FaPXrJycns3buXb775hnPnztGhQwfVT40b%2B/rD%2BZliaM/JkycZMWLEE4t7td/fERER7Nq1i9jYWGrUqMGbb75Ju3btVM0pnk1SAIt/hX/qKVHzoPqksZKQO%2BNY7bx5H5r590HtIRAAVatWpUePHtoPksqVK6ue09/fnxEjRvDOO%2B/g6OhIdHQ0X3zxBX5%2BfiQkJPDee%2B%2BpcomusmXLcv78eZ3e7QsXLmh7YRMSElSbfHfq1Cl27dpFXFwcNWrUMNpELRcXFz799FOdL45r167lpZdeAuDQoUOqnFVxdXVl3rx5vP/%2B%2B9jY2JCens7ChQtp1qwZGo2GsLAwnJycij1vUFAQ%2B/fvp1q1avTu3Zvly5cbZUKasa8/nJ8phvZ8/vnnQO5zNLb169ezatUq/P398fDw4Pbt20yYMIEZM2bwxhtvGH1/hGnJEAghDPR473NiYiLbt2%2Bnd%2B/evPXWW6rlfXzYR2JiImvWrKFjx468/vrrquUFuHfvHpMmTeL48ePaC%2Bd36dKF%2BfPnq3rZqJycHNasWcP27duJiYnB0dGRgIAABg8ezPnz5wkPD2f8%2BPHFfi3iDRs2sHLlSvr27UuNGjWIiopi69at2glEI0eOpE2bNkybNq1Y837zzTe8//77dOnSBUdHR27fvs2BAwdYsWKF6jf/uHDhAm%2B//TaWlpZUr16dmJgYcnJyWLVqFRkZGQwePJjly5fj5eVVrHmjoqJ45513uHHjBpUrV%2Bb%2B/fu89NJLLF%2B%2BnJiYGMaNG8eqVato3rx5seadMmUKvXv3pmXLlsXa7j%2B5ffs2gwcPNtr1h/Mz1dAeyO353rJlC0OGDOHq1atMnTqVKlWqMHfuXKpWrVrs%2BQC6dOnCkiVLdL5cnDp1iunTp/O///1PlZzi2SUFsBAquHXrFhMnTmTbtm1Gzfvo0SN69uzJ999/r2qe8ePHk5GRwXvvvYejoyO3bt1i4cKF2Nvbs2DBAlVzm8qePXsKFN5dunQhMjKSY8eOMXDgwGK/FJ63tzczZsygbdu22nUHDhxg6dKl7N69u1hzFSYpKYn9%2B/dz9%2B5datSogZeXF7a2tjx48IDs7Gzs7OxUyZuTk8Pp06eJjY3F0dGRpk2bYmZmRnp6OlZWVka73nZWVhaXLl2iYcOGquYxxfWHAVq3bs3hw4d13rfZ2dm0bdtW%2B%2BW2ZcuWqlyXd%2BrUqfzxxx/s2rWLAQMGYGdnh42NDY8ePWLVqlXFng%2Bgbdu2HDx4UGeYVEZGBq1bt1btMpni2SVDIIRQQY0aNbhx44ZJcqsxLvNxx48f5/vvv9f2tL700kssXryY1157TdW8GRkZhIeHExsbS05ODpA7UezSpUuqfWjm8fb2LvTqCy4uLqoNSUhISMDd3V1n3auvvqrq3efyK1%2B%2BvM7ZhKysLC5cuKB6QZienk6NGjW0E%2BFu3brFpUuX6Ny5s2o5Dx48yOzZs4mNjdW5moylpSW//fabankBbG1tTXIzBlMO7Tlx4gQ7duzgzz//5Ndff%2BXAgQNUqlRJ1fG4vr6%2B/Oc//yEwMFA73OS///0vXbt2VS2neHZJASyEgR6/DFlmZiZ79%2B4tMIGouD0%2BgSUzM5OffvpJ5z73aqlcuTKPHj3SGWqQnp6OjY2NqnmnT5/OTz/9ROXKlcnMzKRs2bJcvnxZ9fF79%2B/fZ%2BPGjYUW3mr2xHp6ehIWFqZzijo8PJxXXnlFtZx5fvzxR%2BbMmWP0gnD79u3MmzeP9PR0nfV2dnaqFsCLFi2iS5cuVKxYkYsXL%2BLj48PKlSvx9/dXJd%2BTrnecn9pj%2BYcMGcKIESMKHdoTHR3NyJEjVbvkXnJyMpUqVWLv3r3UqlWLqlWrkpGRocp8jS5dumBmZkZmZibR0dFs3boVR0dH4uPjiYmJUf0LnXg2SQEshIEGDhyo87O5uTlOTk7aa1yq5fGJfxYWFri6uvLOO%2B%2BoljOv2O/UqRMjR45k3Lhx1KhRg7i4OEJCQlQrFvL89NNPbN68mcTERDZv3sySJUtYu3Yt586dUzXvtGnTuHHjBlWqVCEpKQlHR0d%2B/vln%2Bvfvr0q%2BgQMHYmZmRkpKCt988w3btm2jZs2axMXFce7cuWK/3nBhFi9ebNSCMM/q1au147hPnjzJ4MGDWbRokepF/%2B3bt5k8eTJ37tzh2LFjdOnShbp16zJhwoQCf%2BPFobivVa3EoEGDsLOzY/v27ezbtw9HR0dmz56tHdrj5%2BenynMHcHZ25j//%2BQ%2BHDh3C09OTpKQkli1bxssvv1zsud5%2B%2B%2B1ib1OUfDIGWAhRZP90ql/tm3C0atWKkydPkpiYyIABA4iIiCA9PZ2OHTuqel3YFi1aEBERQWxsLJ999hmhoaHs2rWLb7/9VjurvTg97RqpedS%2BVmrTpk05deoUd%2B7c4f3332fjxo1cuXKFCRMmEB4erlreZs2acfr0aaKiopg0aRJff/010dHRDBkyhH379qmW19PTkx9%2B%2BIGsrCw6dOjAkSNHgL/fc6J4XblyhTlz5mBjY8OyZcu4cOEC8%2BbNY8WKFdq7TBpLdna20W9lL0xPeoCFKAYxMTFERUUVuBOdmneQyszMJCIigqioKO1p%2BTxqFUeRkZGqtFtU1apV4/bt29SqVYuEhARSUlIwNzcnOTlZ1byWlpZUrVoVW1tb7V2zvL29tdcBLm7Pwo0AqlSpgrm5OY6Ojly9ehXIHet99%2B5dVfPa2dmRmZlJ9erVtdccdnR0JCEhQdW89evXZ/ny5YwePRo7OzsOHjxImTJlVB/WYwqzZ89m9uzZT71qiRq3JM7vpZde0rkUmpubm6pfrCC3l3/VqlU6w3oyMzO5du0ahw8fVjW3ePZIASyEgVatWlXoLUvV7g0NCgri%2BPHjODs764ybM9aNEgor%2Bs3MzFS9jJSvry/9%2BvVj27ZtdOjQgXfffRcbGxsaNWqkWk7IndR4/vx5GjVqRHJyMomJiVhaWqp%2Bi9Xbt2%2BzevXqQr/kqH0K3VQFYZMmTfjggw94//33qVOnDps3b6ZMmTJUqlRJ1byTJ08mMDCQPn36EBgYyKhRo8jJyeG9995TNa8pPAsnfk0xoXXGjBlkZmZSpUoVEhIScHFxITw8nMGDB6uSTzzbpAAWwkDr169n5cqVT70hhhp%2B/vlndu/eTc2aNY2WM4%2Bpiv4RI0ZQq1YtKlSowPvvv8%2BiRYtISkpS5S5s%2BfXr14%2BBAweyZ88efHx8GDx4MJaWlqr28ANMmDABa2trWrdubbRLf%2BUxVUE4bdo0Zs6cSXJyMpMnT2bkyJGkpaWp3iN5//59du/ejYWFBTVq1ODAgQMkJyerfjo%2BJSWFsmXLqprjcXPmzAHAycmJN998s9ivm10UppjQ%2Bttvv2kvN7dixQpmz56Np6enKsOYxLNPxgALYaBXXnmFQ4cOGX0MWdeuXdm%2BfbtRrhf6OHd3dxYsWGD0ot%2BUzp07p525v27dOpKTkxk6dCjPPfecajldXV05evQoZcqUUS1HUcXFxRmlIHxcVlYWmZmZql2OK4%2B7uzs//vij6nke5%2BXlxe7du03yd%2Bzm5sbRo0dNMv7V3d39iRNaly1bpkrOtm3bcuTIEZKTk/H19WX//v1oNBratm3L0aNHVckpnl3G7VIQ4l%2Bof//%2BLF26lKSkJKPmnTJlCuPGjeOHH37g5MmTOv/UZmlpSYcOHUpN8Qu5p%2Batra2xsrJixIgRTJgwQdXiF3InHao95raoHBwcjF78Qu57zRhFaa1atVS/3u%2BTpKammiTvq6%2B%2Byueff05cXJzRc%2Bfk5FC3bl3q1q2rPWvUv39/fvnlF9Vy1q5dm59%2B%2Boly5cqRlZVFVFQU9%2B7dIysrS7Wc4tklQyCEMFDdunUJCgriiy%2B%2BKLBNzeEAZ8%2Be5fDhwwUmb6g9DAH%2BLvpHjhxpkp6r0mLmzJkMGTJEezmy/J6FiXL/Js899xxvvfUWNWvWxMHBQefLnZrjrd3d3enduzceHh44ODjobFP7d3zq1Cn27NlT6HAmtY8hppjQOnz4cEaPHs2ePXvo06cPAQEBWFpa4unpqVpO8eySAlgIA3300UcMHTqUtm3bGvVU4qZNm/jss89o166d0ceHmqroL21CQkJISUnh999/1/kdl6aed2NxdXXF1dXV6Hnv3LlDrVq1uH79uvaqF2Cc37FaVzEpClNMaG3Xrh179%2B7FwcGBMWPGULt2bZKSklS/trV4NskYYCEM1KJFC06dOmX0vO3atePgwYMmGb/XoUMHfH19Cy363dzcVMv74YcfMnPmzALr33vvPZN%2BmKvF1dWV//u//%2BP55583eu74%2BHjs7e0LrL98%2BTLOzs6q5T179qzOrXnzHPr/9u49LOf7/wP4M51Eo6MSzWS2bCFSqZAiJsU6qJVKDSmHDmvMcdFhWc2ZsoiN1IxQkRy2bM1IxJwSm1PnWncOJTp9fn%2B4ur9uFfvpft%2Bf1OtxXbuu9bm7er5Lh9f9vt%2Bf1%2Bv33zFmzBhmuUTyjh49CnNzczQ2NgpvaA0ICIC2tjaTvHHjxiE5OZletSIAaAeYkDazsrLCiRMnmI5pbYmXlxe%2B%2B%2B47%2BPj4MD%2BL%2BrLHjx8jKChIIlmlpaXCG1T27dvXbIfo8ePHOHHiBNM1%2BPr6ttiayc3NDfHx8cxye/XqxVsf2okTJyInJ0fkWkNDA5ydnZtdFycvL69mH7%2Bqqgr%2B/v64ePEis9ym6Xsvk5WVhYqKCiwsLGBtbS323EOHDrX6GOsR33ybNGmS8P%2BbOlOw1NjYiKdPn1IBTABQAUxImz19%2BhT%2B/v4YMGAAlJSUJHZ2cM%2BePSgqKsIPP/zQ7DHWxxAkWfQrKysjPj4eAoEAtbW12Lhxo8jj8vLyTM5KFhQUCIuTP/74o9l0tqqqKuFQDFZmzpyJuXPnwsPDAz179hT53mLRgu3evXuYOXMmOI5DTU0Nxo0bJ/L406dP0adPHya5kydPRkNDAziOw6BBg5q9z/Dhw8We%2B6KhQ4di7969cHJygra2NoqKirB3716MGTMGampqCA8PR0VFhdhHA7/8/fzw4UPU1NTAwMCgQxbArT3ReBGr35sjR46Ek5MTzM3Nm53z9vHxYZJJ2i8qgAlpo/fffx/vv/%2B%2BxHNXr14t8cwmkiz65eTksH//fgDPC8KWzh2zoKWlhVu3bkEgEKChoQFZWVkij8vLyyM4OJjpGr7%2B%2BmsAaNbZg9WNjv369cOyZctQWVmJlStXNntiIS8vz6Tw7tevH/bt24dHjx7B29u7WV9WeXl5fPDBB2LPfVFOTg5iYmJEBrmMGzcOUVFRiIqKwtSpU%2BHv7y/2AvjXX38VeZvjOGzbtg0PHjwQa05L%2BOhBbGxsLNG8F927dw%2BamprIy8sTefIqJSVFBXAnRGeACSH/by/vhr6I9Z3r//77L9TU1FBbW4v9%2B/dDRUUFn3zyCdPM5cuXIywsjGlGe3Pu3Dmm57lb09QZoElVVRXk5OQgJyfHNHfEiBE4d%2B6cyM2GjY2NGDFihPBIxvDhw5ke/2jS0NCAMWPGMB/Py2cP4iZ1dXV4%2BPAhlJWVebmfgXRetANMSBstWbKk1cdYTK%2BytbVFamrqK4dQ/PLLL2LPfRFfLbj27duH8PBwXLp0CVFRUUhLS4OUlBRu376NuXPnMssNCwsTjkJ%2B/Pgxtm7dChUVFeFEOFaKiopafUxLS4tZLgD0798f33zzDZYuXYrz58/Dz88PysrK2LBhA9NXPGprazFv3jxs2bIFJ06cQGBgILp3747o6GgYGBgwy9XW1kZSUhKmTZsmvJaamir8Ol%2B7dq3FmwJZuHPnjsQ6fdTU1PBSAFdXVyMkJATp6emora1F165dYWdnh8WLFzN9snPq1Cn89NNPKCwsRK9eveDg4MDkbDdp/6gAJkTMKisrcfbsWTg4ODD5%2BN7e3gCeF6F8tcOSdNHfJD4%2BHlu2bEFDQwMOHDiAbdu2QV1dHe7u7kwL4JiYGGzfvh0XLlxAaGgorl69ii5duqCkpATLli1jltv0JKfphboX/71Zn/MOCQnBkydPwHEcvvnmG1hbW0NBQQGhoaH48ccfmeV%2B88036NWrFziOw9q1a%2BHn54fu3btj9erV2LdvH7PchQsXwtfXF0lJSejTpw%2BKiopw48YNbNy4Ebm5uXBzc2Pyb/3ymdi6ujrk5eVhypQpYs96GZ89iFetWoV79%2B4hOjoavXv3Rn5%2BPjZt2oTvvvsOS5cuZZKZlpaG5cuXw8nJCaNGjcK9e/ewbNky1NTUMPt9TdovKoAJaaOWCr4///wTCQkJTPJsbW0BPG%2BD9vIfLQBMJym1hnXR36S4uBhmZmbIycmBjIyM8MaoR48eMc09fPgw9uzZg9raWhw7dgx79%2B6Furo6pkyZwrQAfnknXyAQYPv27c1uTmPhypUrSEtLQ3l5OXJzcxEXF4d33nmH%2BRnOvLw8bN26FYWFhbh//z5cXV3RvXt3rFmzhmmuqakp0tLSkJqaiuLiYlhYWGD9%2BvXQ0NBASUkJEhISWrw5r61e/np26dIFnp6eGD9%2BvNizXsZnD%2BKMjAykp6dDVVUVwPPe4rq6upg6dSqzAvj777/H5s2bYWpqKrxmaWmJsLAwKoA7ISqACWHA1NQUfn5%2BTDM%2B/fRTREVFwczMDMDzm2c2bdqE2NhYXL16lWm2pIv%2BJj179sS9e/dw7Ngx4fnUs2fPMn9puqysDLq6ujhz5gzeeecd6OrqAmA/wvbljgt9%2BvRBWFgY7OzsmO8Q1tTUoGvXrjhx4gQ%2B%2BOADKCsro6qqiumRDwCor68Hx3E4ffo0Pv74YygqKkIgEEikHVyfPn1avBlKU1MTmpqaTDL5nOi3e/du3rLl5eWbnfnt3r0707HXhYWFMDExEblmbGyM4uJiZpmk/aICmBAxq6%2Bvx%2BHDh6GiosI0Z968eZg3bx68vLxgb2%2BPRYsWobS0VGJdEl4miaLfy8tLuAO%2Be/duXLhwAXPmzGHejUFDQwPZ2dk4dOiQ8A/o4cOHmTXsfx3WO94AMGTIEKxcuRIXLlzApEmT8O%2B//yIkJIT5jXGmpqZYsGABbty4gZkzZyI/Px%2BLFi3C2LFjmebyJT8/X7jj3djYKPIYyzaKAL89iH18fODn54elS5eiX79%2BKC0txZo1a2BtbS1y9l2cZ9179eqFCxcuiHT6OH/%2BPHr37i22DPL2oC4QhLSRrq5us5cMpaWlsWzZMri4uDDNzsvLg4%2BPD8rKyjB%2B/HiEh4fzckNLU9EfHR2N48ePM83Kz8%2BHjIwMevfuDYFAgKKiIqbjUwHg2LFjWLRoEbp27YrExESUlpbC29sbmzZtYlqYvdxto66uDpmZmVBTU0NsbCyzXOD5rvfatWshLy%2BP5cuX4/r164iJiUFYWBjTyXTV1dXYsWMH5OXl4e3tjRs3bmD//v0ICgpiujvIF0dHR8jJyWHkyJHNRpqz3h22tLQUefvFHsSsd4ebXkUBIHLO/cW3xd3ub%2B/evVizZg1cXFygra2N/Px8JCYmYuHChSI3P5LOgQpgQtro3LlzIm936dIF/fr1Y/6yfE1NDaKionDw4EEYGhrir7/%2BQnBwsETuaOaz6K%2BtrcVvv/2GwsJCODs74969eyJ/TFl59uwZgOcv3VZVVeHJkyctnsEWp5d7zkpLS2PAgAGYM2cO8%2Bz2QCAQMH8lhW/Dhg3DmTNn0LVrV76XItKDeNGiRUyzbt68ie7du7/2/cQ9eGXfvn1ISkpCRUUF%2BvTpg2nTpmHy5MlizSBvByqACXlLWVlZoWvXrli7di0GDhyItLQ0rFy5EmZmZli3bh3TbL6K/vv37%2BPzzz9HXV0dHj16hAMHDsDGxgabN2%2BGhYUF02yBQICUlBQUFhbC398f2dnZzDP59vPPP2P37t0oKyvDwYMHsXr1akRERPynwuVN1dXVYfPmzYiPj0dDQwNSU1MREBCAmJiYDln0u7i4ICIiAu%2B99x7fSwHQ8XsQ3717F2pqalBUVMRff/2FHj16oH///hJdA2kf6AwwIW1069YtREZG4u7du83O8LHsx2tqaoqlS5cKbw6ytrbG0KFDsXDhQmaZTfgYkAAA4eHhsLe3h6%2BvL4yMjNC/f3%2BEhYVh48aNTIvRa9euwcvLCzo6OsjLy4OHhwf8/f0RHBzM/O7xy5cv486dO3h5r4L1Gc0ffvgBiYmJmDlzJiIjI9G9e3eUlpYiIiKC6VCQzZs34%2BzZs9iwYQMCAwOhqqoKTU1NhIeHY8OGDcxys7KysGrVKty9e7fZ15ply7nly5fD09MTEyZMQI8ePUQe4%2BMGuY7cg/j48eMICgpCYmIi9PT0cP78eWzZsgUbNmzA6NGjJbYO0j7QDjAhbeTi4gIFBQVMmjSp2R3ydnZ2El9PXV0dZGVlmWbwVfQbGxsjMzMTcnJyMDIywrlz59DY2AgjIyOm7d/c3Nxgb28Pe3t7GBoaIjs7G5mZmYiIiEBaWhqz3LVr1wp7Hb/4vSUlJcV82MnEiRMRHR2NAQMGCL/WZWVlsLOzY7o7aGlpicTERGhoaAhzHz16BCsrq2bjqMXJzs4Ourq6sLW1bfZzzPIJn4%2BPD3JycjBw4ECRM8BSUlLMb4J7VQ/ilStXMs1esmQJzpw5I9EexDY2Nli4cCHMzc2F13777TesW7fulTcEko6JdoAJaaO8vDz8/vvvEn8p7/79%2B9iyZQtKS0uFRWhdXR3u3LmDs2fPMs3%2B%2BuuvoaCgAG9vb%2BZtsV70zjvv4N9//xW5M7y8vBw9e/Zkmnvz5k1MnToVwP96pI4ePRoBAQFMc1NSUrB161aRP9iSUllZKXxpuGmfRFVVFfX19Uxznzx5Ijz325TbtWvXZjeIidvdu3fx008/SaTd2ouysrJw4sQJpjcWtqaz9SAuKipq9rM0ZsytLGDtAAAgAElEQVQYBAUFMcsk7RcVwIS0Ua9evVBbWyvx3GXLloHjOCgrK6OiogIfffQRDh06BE9PT%2BbZfBX9tra2mD9/PoKCgtDY2IjLly8jKiqK%2BU0sKioquH37NgYOHCi8dvv2beZFS3V1NcaMGcM0ozW6urrYu3cvXFxchEVJWlqayNeABX19fWzevBmBgYHC3N27d2Pw4MFMc9977z2UlZVJvLVdr169JF50N%2BlsPYi1tLRw%2BvRpYe904HkfcWqD1jlRAUxIG7m5uWHevHnw8PBoVhAZGhoyy7169SpOnTqFoqIirF%2B/HsuXL8eYMWPw/fffM//DxlfRP3fuXDx9%2BhTz589HTU0N3N3d4ejoyPzzdXV1xZw5c%2BDj44P6%2BnqkpaUhJiYGzs7OTHPHjh2L1NRUiYzFfdlXX30FT09PJCcn48mTJ5g9ezYuXbqE7du3M81dtmwZZsyYgYMHD6K6uhrW1taorq7Gzp07meZOmjQJs2bNgqOjY7ObOVmet545cybmzp0LDw8P9OzZU2QHlOXvD4DfHsQA8M8//yAxMRElJSUIDQ3FkSNH4Obmxixv9uzZ8PX1hbW1NbS0tFBcXIz09HR88803zDJJ%2B0VngAlpo9ZacIm7h%2BXLTE1N8eeff6K6uho2NjbIyMgAAJiYmODMmTPMcgEgPj4eR44ckXjR/yKBQABlZWWJ3bCzZ88eJCQkoLCwEBoaGnB2doanpyfTl%2Bb9/Pxw8uRJvPfee82%2BzpIoUEpLS5GSkoKioiJoamrC1tZWrIMJWlNTU4OMjAxh7tixY5m/2vByT9wmrM9b8/X7A%2BC3B/Hp06exYMECWFhYICMjA0eOHIG9vT28vLzg7e3NLPfPP//EoUOHUF5eDk1NTeG5ftL5UAFMyFvqs88%2Bg6%2BvL8zNzWFubo74%2BHjIycnBxsYG2dnZTLP5%2BqP98mCIJnJyclBWVoapqanY%2B4byqbXPF%2BD35WuWXpwC9iJZWVn07NkTcnJyEl5Rx8VnD2IHBwf4%2BfnB3NxceGPplStXEBAQwPwGT0IAOgJByBsrKSmBpqZmq3%2BwAfGO8XyZt7c3/Pz8cPjwYTg7O%2BOzzz6DtLQ0xo0bxyyzyY0bN5hntOTmzZs4fvw4Bg8eDG1tbRQVFeHSpUsYPHgwGhoaEB4ejpiYGOG4YnFZsmRJi9dlZWWhoqKCsWPHQl9fX6yZAL9FbkvDTgBARkYGKioqsLCwwOLFi8VePFlZWTV7Ob5Jly5dYGpqim%2B//VZsAzIuXLgAAwODVp80SklJiYzOZYGv4S66urooKSnhpQfxvXv3hOfbm77PBg8ejIcPHzLJS0tLQ21tLT799FMIBAIEBgYiNzcXEydORHBwsERv5iXtA%2B0AE/KGhg8fjpycHGGh0PSjxGqMZ0tKS0uhoqICWVlZpKWloaqqCp9%2B%2BmmH3SX74osvMGLECLi6ugqvJSUlISsrC5GRkUhLS8POnTuxb98%2BseZ%2B/fXXOHDgAMaPHy8svI8fPw5TU1PIy8sjMzMT4eHhEpnCJyk//PADkpOTERAQAG1tbRQWFmLTpk0wMjKCgYEBduzYgQ8//BDLly8Xa258fDwyMjKwdOlSaGtro6CgAJGRkdDT08OECRMQExMDGRkZREVFiSXvxZ/jlrD%2BOeZzuMu1a9cwb948XnoQT5kyBcHBwTAwMBC2u7ty5QqWLl2K1NRUsWalpKQgODgYX375JaZPn44vv/wSubm5CAwMxO7du2FsbIy5c%2BeKNZO8BThCyBspKiriOI7jCgoKWv2PiJexsTFXX18vcq2%2Bvp4zMjLiOI7jGhsbueHDh4s9d9asWdyJEydErp06dYqbM2cOx3Ecd/bsWc7GxkbsuXyaNGmS8Hu8SUlJCTdp0iSO4zju33//5czMzMSeO378eK6yslLk2oMHD7hx48ZxHMdxjx8/Fv57dwTe3t7cli1buMbGRm7EiBEcx3HcgQMHuE8//ZR59pw5czhDQ0PO1dWVc3NzE/7n7u7OPPvw4cOcoaEht3btWk5fX5%2BLjY3lRo8ezR08eFDsWY6OjlxGRgbHcRz37NkzbsiQIdwvv/zCcRzH/fPPP9zEiRPFnknaP9rzJ%2BQNNbXOkfSZ00GDBr32fVjvPPOlW7duuHr1KoYOHSq8dv36deGOd0VFBRQUFMSe%2B9dff%2BH7778XuTZ69Ghh/1BjY2MUFhaKPZdPTa8uvKhnz54oLi4G8Lw13NOnT8WeW1lZCWlpaZFrUlJSqKioAAAoKCi0ekTibXTp0iVs2rQJUlJSwqMAU6dORXh4OPNsPnsQT548GYqKitizZw%2B0tLRw9uxZLFu2DBMnThR71p07d4THLa5du4a6ujrhcJP%2B/fujtLRU7Jmk/aMCmJA3xFch2rNnT9TX12Py5MmwsrLqsMcdWuLp6Qlvb2989tln6NOnDwoLC7Fv3z7MnDkTRUVF8PHxYdITWEVFBZmZmSJN9M%2BcOQMlJSUAz9tJsRjG4evri6ioKIn3Wwae3yAVGhqKFStWQF5eHs%2BePcO3334LfX19cByHvXv3YsCAAWLPbXpisWzZMmhpaaGoqAhRUVEYNWoUamtrsWXLFnz88cdiz%2BULX8NdAH57EAOAubk5TE1N8fDhQygrKzd74iMujY2NwicXf/31FwYMGCD8mXrw4AGd/%2B2k6F%2BdkDfEVyGamZmJX3/9FUlJSVi0aBGsra3h6OgokZtmXiQQCJCSkoLCwkL4%2B/sjOzub%2BZlFDw8PqKqqIikpCcePH4eWlhZWrlyJCRMm4MaNG7C3t4e7u7vYcxcsWID58%2BdjwoQJ6Nu3LwoLC3Hy5EmsXLkSt2/fxowZM5j0L7148SJvT3BWrVqFOXPmwMDAAMrKyqisrMT777%2BPjRs3IisrC%2BvWrUNMTIzYc4ODgxEUFISJEycKi5axY8ciPDwc58%2Bfx6lTp7B27Vqx5/KFr%2BEuAL89iKuqqhAaGor09HTU1taia9eusLOzw%2BLFi8X%2BPf/BBx/gzJkzMDU1xbFjx0QGYZw%2BfRrvv/%2B%2BWPPI24FugiPkDdXV1QkL0atXr/JSiJaWluLgwYM4ePAgunfvDkdHR9ja2uKdd95hmnvt2jV4eXlBR0cHeXl5SElJweTJkxEcHAwHBwdmuaGhoQgMDORlR/TixYs4cOAAiouLoaWlBScnJ%2Bjp6eHu3bv4%2B%2B%2B/mYyPDQsLQ0FBAWxtbaGuri7xIQl9%2BvTBxYsXUVpaCi0tLQwdOhRSUlJ49uwZZGVlmfRAPn/%2BPIYNG4Z///0XJSUl0NLSajaYQpKqqqqYfr/V1dVh7dq1%2BOmnn1BTU4OuXbvC0dERixYtYv7kh88exIsWLcK9e/fg5%2BeH3r17Iz8/H5s2bcLw4cOxdOlSsWadPHkSX331FbS0tFBYWIjk5GRoa2tj7dq1SExMRHBwMGxsbMSaSdo/KoAJEQO%2BCtEXnTt3DqGhocjPz8elS5eYZrm5ucHe3l7YRD47OxuZmZmIiIhAWloas1wjIyP8%2BeefEn/Jkq%2BjCHwWKKampjh%2B/LjEP2djY2OcOnWKyVnuV2nqRPCyESNG4Pz58xJZg6SHu/DJ0NAQ6enpUFVVFV4rLS3F1KlTcfbsWbHnnTt3DhcvXoSlpaVwnPdnn32GKVOmiHSVIZ0HHYEgRAw0NDTg4%2BMDHx8fYSEaGRnJvBAFnvfTPHjwIJKTk1FXVwcXFxfmmTdv3sTUqVMB/K%2BH5%2BjRoxEQEMA018HBASEhIbCzs0OvXr1ECgWWPZf5OorAV79lAFBSUkJpaanEC2BtbW1cuXJFeJMSS/fu3cPXX38NjuNQVVUFDw8PkcerqqqatQcTl5kzZyIuLk749tOnT8XW2/j/g68exPLy8s3O/Hbv3p3ZEx8jI6Nm31M//fQTkyzydqACmBAxkWQhWlVVhaNHjyIpKQnXrl3D2LFjsWLFCpibmzO7keRFKioquH37tnAnBQBu377N/G7ynTt3AgB%2B/vlnYfHLSaDnso2NDfz8/Hg5ilBSUoLU1FQUFhaiV69esLGxwbvvvss0EwAGDhwIJycn6Ovro1evXiKPRUREMMvt2bMnvLy80Ldv32ZPcsQ9/rlfv36YMGECKisrkZOT06xAkpOTa3VEcltdvHhR5O0xY8a0uAPN0ss9iM3NzeHg4CCRHsQ%2BPj7w8/PD0qVL0a9fP5SWlmLNmjWwtrYWGS4kidHbpHOiApiQNuCjEA0KCsIvv/yC/v37w87ODtHR0RLfOXJ1dcWcOXPg4%2BOD%2Bvp6pKWlISYmBs7Ozkxz%2BRqRGh8fDwA4deqUyHXWhfeVK1fg6ekJHR0d9O3bF1euXEFsbCzi4uJgYGDALBd43nJuwoQJTDNaMmzYMAwbNkxiedOnTwcA9O3bF59%2B%2BqnEcl/Gx2nE8PBw2Nvbw9fXF0ZGRujfvz/CwsKwceNG5gVwWFgYAODTTz8VGSQEADt27JDYMCHSedEZYELe0MuFqI2NjUQKUV1dXSgrK%2BP9999v9ayguHfKWrJnzx4kJCSgsLAQGhoacHZ2hqenJ5Mbo16lvr4eN2/exEcffSTRXEnw8PDA%2BPHjRV6a//HHH5Geno7ExEQeV9Yx/fXXX7h//z4aGhpErrMojJsm0DVp7QwyS8bGxsjMzIScnJwwv7GxEUZGRszPPf/XvtmS7rNOOg/aASbkDR05cgTKyspQVFTEyZMncfLkyWbvw6IQZT2i9L%2BaPn26cPdMUk6dOoVVq1ahtLRUZMdIRkYGV65cYZpdU1ODhw8fCocw1NXV4ebNm7CysmKWmZeXhx07dohcc3V1xcaNG5llNqmtrUVqaipKS0ubfc4s2p81qaysxO7du1vMTUlJYZa7bt06xMbGQk1NDbKyssLrUlJSvO4Ms8RnD%2BJNmzbBwcGB%2BREiQlpDBTAhb4ivQvT/k7ty5UqsXLlS7GtoaGjAsWPHcPfu3WZTuVh%2BXb777jtMmDABPXr0QF5eHmxsbLBlyxY4OjoyywSApKQkhIaG4tmzZyLXVVVVmRbACgoKKC4uhra2tvBacXGxRAqUpUuXIjMzE8rKyqirq0O3bt1w69Yt5sXgkiVLcPfuXaioqKCqqgpaWlr4448/mD/Z%2Bvnnn/HDDz/A2NiYaU6T%2Bvp6HDp0SPh2XV2dyNsAm53nF/HZg7hbt25YsGAB3nnnHdjZ2cHe3h6amppMM588eYKffvqpxV3%2B0NBQptmk/aEjEIRIAKtC9HVefplVXJYvX44jR45AV1dXpCWZlJQU0%2BMXQ4cOxYULF1BQUIAVK1Zg9%2B7d%2BPvvvxEYGIjU1FRmuVZWVpg%2BfTq6d%2B%2BO7OxszJgxA1FRUTAzM8Ps2bOZ5UZGRuLPP/9EUFAQ%2Bvbti/v372PdunUYNWoUvvzyS2a5wPOXxxMTEyEQCJCYmIg1a9Zgx44duHz5MtavX88s18DAAGlpaSgtLUVsbCw2b96M5ORkHD58GNu2bWOWO2rUKPzxxx/MPv7LXndznZSUFPMz73z2IG7Kz8jIwMGDB3H69GkYGhrCwcEB48ePZ5IfEBCACxcuwMDAQGSXHwCioqLEnkfaOY4QwtywYcN4ydXX12fycU1NTbnLly8z%2BdivMnbsWK6hoYF79uwZZ2JiIrw%2BYsQIprlDhw7lGhsbufz8fM7Z2ZnjOI4rLCzkrKysmOY%2BffqU%2B%2Bqrrzg9PT3uww8/5IYMGcKtWrWKe/r0KdNcjvvf17SiooKbNGmScD1mZmZMc42MjDiO47iHDx9y48eP5ziO4%2Brq6jhTU1OmucuXL%2BdSU1OZZrRnFRUVXGNjI2/5Fy9e5Ozs7LgPP/yQMzIy4lavXs09evRIrBn6%2BvpcSUmJWD8meXvREQhCJIDj6YUWVg31Gxsbebnp7MMPP8SGDRswb948qKqq4rfffkPXrl0hLy/PNFdVVRV1dXXo3bs37ty5A%2BB5e6aKigqmufLy8li9ejVCQkLw8OFDqKmpSWxIgqamJvLz86GtrY2Kigo8efIEXbp0QXV1NdPcPn364OrVq9DT00N1dTUEAgFkZGTw9OlTJnnu7u6QkpJCdXU1kpKSEBsbCyUlJZH3kcRNpZLUXnoQl5eX4/Dhw0hOTsY///wDc3NzzJ8/H1paWli/fj18fX2FHVjEQU1NjZfPk7RPVAATIgEdbbKTjY0N4uLi4O3tLdHchQsXws/PD05OTvDz88PcuXPR2NiIRYsWMc0dMmQIvv76a6xYsQLvvfceEhMT0bVr12aFkrhVV1dj37598PT0xKNHjzB37lyoqKggJCQEGhoaTLNtbW3h6uqK/fv3Y%2BzYsfD19YW8vDz09PSY5rq6usLd3R1HjhyBjY0NZsyYARkZGWY3S7145pd166/2oj30IJ45cybOnDmDAQMGwN7eHlOnThUpTr/44guxt1V0dnbGd999Bz8/P3Tv3l2sH5u8fegMMCESwOosLl%2B5rq6uyMnJgYKCQrMdFUn26i0rK0N1dTX69%2B/PPGf58uUICwvD/fv34ePjg6dPnyIiIgK2trbMchcvXozc3FwkJyfDzc0NqqqqkJeXx%2BPHj5l2Ymhy9OhRmJubo7GxEVFRUaiqqkJAQIDITXksXL58Gbq6upCSksLOnTtRXV2Nzz//XCI3/3UGL/9eaBpnLknBwcFwdHTE4MGDW3y8uroaJSUlGDBgQJuzPv74Y2Gv4YaGBkhJSTXr03716tU255C3CxXAhEhARyuADx482OpjdnZ2Ys/7L3%2BcJdlOqb6%2BHnV1dczGtjaxtLTEgQMHICUlBRMTE2RkZEBJSQmjRo2SeMHS0TUdhXiZrKwsVFRUYGFhAWtrax5WJn589iC2tLQUFqOtvTIm7ifRZ86cee37mJiYiDWTtH90BIKQDozV81sWRe6ruLu7v/JxVhOjXm5L1RKWraqqq6uhpKSE9PR0aGtrQ0NDA7W1tUyP1LRWCL6IxZnYpsLoVVi%2BujB06FDs3bsXTk5O0NbWRlFREfbu3YsxY8ZATU0N4eHhqKioeO33Inm1BQsWAHj%2BuykkJATBwcHMM5uK24iICCxZsqTZ40uWLKECuBOiApgQCWD9QotAIGjx5g5/f3%2Bx5nh7eyM2NvaVRRKL4ujGjRti/5j/xesGTrAekjBw4EBER0fj999/h4WFBaqqqrB%2B/Xp8/PHHzDIl1Qf3ZU2FEV9ycnIQExODESNGCK%2BNGzcOUVFRiIqKwtSpU%2BHv798hCmA%2BexC/%2BOR59erVzJ9Ml5aWCne3f/rpJwwePFjk9/Hjx4%2BRnp6OiIgIpusg7Q8VwISIkaQKUeD5H7FNmzYhPj4eDQ0NSE1NRUBAAGJiYtCrVy8AgKenp1gzDQwMAPBXJEnar7/%2Bymv%2BypUrsWrVKigqKmL%2B/Pm4fv06srKymE6C42vAi6RfVXjZzZs3MXz4cJFrgwcPxvXr1wE8H0FeXl7Ox9LETk1NTeR7SFlZWeTtjjT9TklJCTt27IBAIEBtbW2zfr/y8vLw9fXlaXWET3QGmJA2%2Bi%2BFKAvr1q3D2bNnsWDBAgQGBuK3337DwoULISMjgw0bNjDLJaQjsrOzg6urK6ZNmya8lpycjG3btuHw4cO4du0avvjiCxw7dozHVXYskjx7DAAzZszAjz/%2BKLE80r7RDjAhbbRp0yacPXsWGzZsQGBgIFRVVaGpqYnw8HCmhWhqaioSExOhoaEBKSkpdOvWDREREUxH8zaprq5GQkJCi6OQ6aXEtouNjYW3tzc2b97c6vvwtVPbUS1cuBC%2Bvr5ISkpCnz59UFRUhBs3bmDjxo3Izc2Fm5sbli1bxvcySRs0Fb95eXkoKCjAmDFj8PjxY%2BoN3ElRAUxIG/FViD558kT4i7vphZyuXbuiS5cuTHOB5zeNXLx4EcbGxs1GipK2y87Ohre3N7Kyslp8vKP1lW4PTE1NceTIEaSmpqKkpAQWFhZYv349NDQ0UFJSgoSEBAwaNIjvZb71Xnf2GGB3/lggEMDPzw8XL16EnJwc9u/fDycnJ%2BzYsQNDhw5lkknaLzoCQUgbjRw5EpmZmZCVlRX206ytrYW5ufl/ar/zpnx8fPDhhx8iMDBQ%2BFJiXFwcsrKyEBsbyywXeH4GeP/%2B/cz7wbZEIBAgJSUFhYWF8Pf3R3Z2doceYMBxHBobGyEtLY3y8nKoqKg062HKgq%2BvL6KioqCoqMg8i3QelpaWr3xcSkqKWbePoKAgKCgoYPHixbCwsEB2djY2b96MM2fOYM%2BePUwySftFO8CEtJG%2Bvj42b96MwMBA4c7c7t27W23wLi7Lli3DjBkzcPDgQVRXV8Pa2hrV1dXYuXMn01zg%2BY0jrCeRteTatWvw8vKCjo4O8vLy4OHhAX9/fwQHB8PBwUHseXy3Qbtx4wZ8fX2xYcMGDBkyBNu3b8fJkyexfft25sM/mnbJJIWvNmi2trZITU19Zb4kh7t0dHzeWHr27FmcOHEC3bp1E/5bz5kzh84Fd1K0A0xIG%2BXn52PGjBmor69HRUUF%2BvXrJyxEdXR0mGbX1NQgIyMDRUVF0NTUxNixYyWyY7d161aUlZVh/vz5Ej0/5%2BbmBnt7e9jb2wt32zMzMxEREYG0tDSx5zXtVjU2NqK0tBRKSkrQ0tJCWVkZ/v33X3z44Yf/qUh%2BU%2B7u7jA0NMTcuXMhIyOD%2Bvp6bN26FTk5OdixYwezXAAICwtDQUEBbG1toa6uLlIcshg60jRc5dq1a/jll1/g5eWFd999F8XFxdi5cyfGjRuHr776Suy5qampsLW1lfhwFyJ55ubmSE5OhpKSkvD3x8OHDzFlyhT89ttvfC%2BPSBgVwISIAR%2BFaFFRUYvXZWVl0bNnT6a7d5aWligqKmpxx4zFQIomRkZGOHPmDKSlpUXuIDcwMMCFCxeY5X777beQk5ODv7%2B/8Ix1dHQ0CgoK8M033zDLHTFiBLKzs0W%2Bzg0NDRg5ciTzSXC6urotXmc1dKTJlClTsG7dOpERuPfu3YO3t7fEOjC01s6QvN1WrlyJ4uJiLF%2B%2BHA4ODjh%2B/DhCQ0PRrVs3hIaG8r08ImF0BIKQNmoqRPX19aGvrw8AePToEWpqapgWolZWVs06MDTp0qULTE1N8e233zL5Q7569Wqxf8z/QkVFBbdv38bAgQOF127fvg01NTWmuUlJSTh9%2BrTIDYbe3t4wNjZmWgArKirizp07Iq8k5Ofno0ePHswym/A1fCQ/Px/vvvuuyDUNDQ2UlZUxzW2tneHWrVuhrq7ONJtIxpdffomvvvpKeIOyiYkJRo0aJZFpdKT9oQKYkDbiqxBdsmQJMjIysHTpUmhra6OgoACRkZHQ09PDhAkTEBMTg4iIiGaN38XByMioxesCgUDsWS9ydXXFnDlz4OPjg/r6eqSlpSEmJgbOzs5Mc%2BXl5fHPP/%2BI7IpevXqVeSFqZ2cHX19fzJo1C1paWigqKkJcXBzs7e2Z5jYpKSlBamoqCgsL0atXL9jY2DQrTsVNT08P3377LRYtWgQ5OTnU1NQgLCxMOISFldbaGYaFhVFf7Q5CUVERW7ZsQVlZGQoLC6GpqYnevXvzvSzCEzoCQUgbxcfHv7YQlZGREXshamVlhX379kFJSUl47eHDh3BwcMDJkydRVVWFcePGtdpKqy0uX76MyMhIlJaWCov/uro6CAQCXL16Vex5L9qzZw8SEhJQWFgIDQ0NODs7w9PTk2n7t61bt2L37t2YNm0atLS0kJ%2Bfj59//hl%2Bfn6YPn06s9yGhgZER0fj0KFDKC8vR%2B/evWFvb49Zs2Yx7wRx5coVeHp6QkdHB3379sX9%2B/fxzz//IC4ujmkxevv2bcyZMwfFxcVQVlZGZWUl%2Bvfvj9jYWKbFiqWlpbCdYdPxmkePHsHKyorJzxDhx4MHD5CamoqioiLMmzcPOTk5GDNmDN/LIjygApiQNuKrEB0xYgQyMjLwzjvvCK89evQI5ubmuHjxItOzoo6OjtDW1oaSkhLy8/NhZmaGXbt2wcPDA15eXmLPaw/279%2BPlJQUlJaWonfv3pg2bRomT57M97KY8fDwwPjx4%2BHh4SG89uOPPyI9PR2JiYlMs%2Bvr65GTk4OysjJoampi%2BPDhzPtb89XOkEjO9evXhTdX/v3330hJSYG1tTVCQ0M7zOhn8t/REQhC2qiysrLZbpyUlBQqKioAAAoKCq0ekWiL0aNHIygoCMuWLRO%2BPB4ZGQkzMzPU1tZiy5Yt%2BPjjj8WeCwC3bt1CfHw8CgoKEB4eDi8vLwwbNgwhISFMC%2BCGhgYcO3asxQl0rCejOTo6wtHRkWnGy/j8fPPy8pp1mnB1dcXGjRuZ5gLPu268%2B%2B676Nu3L4DnRzEAQEtLi1kmX%2B0MieRERETgyy%2B/xLRp02BoaAhtbW1s3rwZkZGRVAB3QlQAE9JGfBWiwcHBCAoKwsSJE4V/sMeOHYvw8HCcP38ep06dwtq1a8WeCwA9evRA165doa2tjVu3bgF4XkAUFhYyyWsSHByMI0eOQFdXFzIy//v1xXoyGl%2Bjn/n6fIHnT9yKi4tFhp0UFxejZ8%2BeTHOPHj2Kr7/%2BGlVVVcJrHMcx7z7BZ19tIhl5eXnC8/NNP0Pm5ub44osv%2BFwW4QkVwIS0EV%2BFqJKSEuLi4lBaWoqSkhJwHIcDBw7A0tISly5dQnJystgzm%2Bjo6CAxMREuLi7o1q0bcnNzIScnx7wwy8jIwK5duyS%2BK8fX6Ge%2BPl8AsLa2xoIFCxAUFCQ8A7xu3TpYW1szzd20aROmT58OOzs7kaKfNW1tbRw5coSXvtpEMpSVlXHnzh28//77wmt3795l3kWGtE9UABPSRnwWosDztlFxcXH47bffMHDgQCxcuJBpHgD4%2B/vD19cXZmZmmDlzJpycnCAtLQ0XFxemuY2Njfjoo4%2BYZrQkKyuLl9HPfH2%2BwPN/Y4FAgLlz56Kurg7y8vJwcHDAggULmOYWFxdj/vz5Ei1%2BmygoKDAv8Al/XFxc4OPjA19fX9TX1%2BP48eOIjo7GtGnT%2BF4a4QHdBEeImE1HLi0AABFxSURBVJw/f16kEHVycmLWIaCxsRHp6enYuXMnbt26hfr6esTExGD06NFM8lry7NkzyMrKokuXLrh8%2BTIeP34MMzMzppnh4eFQV1eHt7c305yXjRkzBidPnpToaGCAv8/3RbW1tXj48CHU1NQkcvTCzc0Ny5cvb3UQh7jxNYKZSB7Hcdi1axcSEhKEu/zTpk3D7NmzJfK9TdoXKoAJaQM%2BCtEff/wRu3btQmNjI1xcXODk5IRPPvkEycnJ0NDQYJbbhOO4ZsMK0tLSMHHiROatuVxdXZGTkwMFBYVmfZVZFil8jX7m6/MFnp973rdvHzw9PfHPP/9g8eLFUFFRQUhICNPvs7Vr1%2BLnn3/GJ5980uylaRY3/jWNQOY4DiEhIS0ORaBRyG%2B3CxcuMO8jTd4%2BVAAT8ob4KkR1dXXh6uqKxYsXC3ckR44cKZEC%2BMmTJ/j888%2BhpqaGzZs3AwAqKipgYWEBPT09bN%2B%2BHd26dWOW31SstIRlkcLX6Ge%2BPl8AWLx4MXJzc5GcnAw3NzeoqqpCXl4ejx8/RkxMDLNcd3f3Fq9LSUlh165dzHIBiIzXJh3H8OHDkZOTw/cySDtDZ4AJeUMRERHNClFJWLFiBRISEmBubg4nJye4urpK7OW7mJgYyMrKYtWqVcJrqqqqyMjIgK%2BvL77//nsEBgYyy%2BdrJ46v0c987jyeO3cOBw4cwMOHD5GTk4OMjAwoKSlh1KhRTHN3797N9OOTzof2%2BUhLqAAm5A3xVYhOnz4d06dPx5kzZxAfHw8rKys0NDTgzJkzsLW1ZXoM4dixY9i2bRtUVVVFrquqqmLVqlUICAhgUgB7e3sjNjYW7u7urX6NWe4ODho0SGTgSJOm/rTi1rS7/iqs%2BwBXV1dDSUkJ6enp0NbWhoaGBmpra5l%2Bj584cQLZ2dnQ09ODjY2NyPCLlStXYuXKlcyyScdF53tJS6gAJuQN8VmIAoCJiQlMTExQWFiIhIQErF69GpGRkZgyZQoWL17MJLOiogL9%2BvVr8bFBgwahvLycSW7T%2BT1jY2MmH781BQUF8PX1xd9//40%2BffogLCwMI0eOFD5ubW3N5KXV100NlMQf9IEDByI6Ohq///47LCwsUFVVhfXr1zMbrpKQkID169fD2NgYKSkpSE1NRXR0tLDtXEpKChXA5I3U1NRg3Lhxr3wfutGx86EzwISISVMhmpSUhC5dujAtRFtSW1uLlJQUJCQk4MCBA0wyzM3NcejQISgrKzd77MGDB5g8eTJOnz7NJJsP8%2BfPR8%2BePeHh4YH09HT88MMPiI2NhaGhIQBg2LBhuHjxIs%2BrZOPvv//GqlWrIC8vj/Xr1%2BP69esIDQ3Fxo0b0b9/f7HnffLJJ1i9ejX09fVRUVGB2bNnY8CAAYiKigLA7mt96NAh4f%2BvWrWqxZvgaErY223IkCEix7ZaQjc6dj5UABMiZpIoRPmyePFi9O3bt8WX36Ojo3Ht2jVs2bKFWb6kJ7KZmJjg119/hYKCAgAgMTERGzZswIEDB6ClpUU314iRgYEBLly4IHy7vLwc06ZNg6enJzw9PZkVwJaWlq98XEpKinYH33L0c0paQkcgCBEzOTk5ODo6wtHRke%2BliN2cOXNgb2%2BPyspKWFtbQ11dHWVlZTh69CiSkpIQHx/PNJ%2BPiWwvnkN1cXHBzZs3sWDBAiQmJnbIm2tiY2Ph7e39ynPILM4fq6ur4/LlyxgyZIjw7fXr18PLywsDBw5kduzj119/ZfJxSfvREX9OSdtRAUwI%2Bc/69%2B%2BPuLg4BAcHY8%2BePZCSkgLHcfjggw%2Bwbds26OnpMc2X9ES2ESNGIDw8HH5%2BfsKetEuXLoW7uzvmzZvXIf%2BwZmdnw9vbu9VzyKwK0RkzZmD27NmYPXs2Zs2aBQDQ19fHsmXL4OPj02zHn5D/asqUKXwvgbRDdASCEPJG8vPzIRAIoK6uDi0tLYlkSnoiW2FhIebNmwd1dXVs27ZNeL2yshIzZ85Ebm4u0z7AfOM4Do2NjZCWlkZ5eTlUVFSY3tx58uRJlJWVwdXVVeT6sWPHsGXLFqSkpDDLJoR0LlQAE0LeGnxNZHv06BF69Oghcq2%2Bvh4ZGRmwsrKS2Dok6caNG/D19cWGDRswZMgQRERE4OTJk9i%2BfTuTm%2BAIIUSSqAAmhLw1%2BJrI1hm5u7vD0NAQc%2BfOhYyMDOrr67F161bk5ORgx44dfC%2BPEELahApgQshb41Vjao2MjCS4ko5vxIgRyM7OFnmy0dDQgJEjRyI7O5vHlRFCSNvRTXCEkLdGa0WuQCCQ8Eo6PkVFRdy5cwc6OjrCa/n5%2Bc2OghBCyNuICmBCyFvj8uXLiIyMRGlpqbArQF1dHQQCAa5evcrz6joWOzs7%2BPr6YtasWdDS0kJRURHi4uJgb2/PNNfX1xdRUVFQVFRkmkMI6dyoACaEvDVCQkKgra2NgQMHIj8/H2ZmZti1axeCgoKYZwsEAqSkpKCwsBD%2B/v7Izs6GhYUF81y%2BzJ8/H126dMHWrVtRXl6O3r17w97eXtiijJWLFy9KrMsHIaTzojPAhJC3xtChQ5GVlYWCggKEh4dj586duHTpEkJCQphO3bt27Rq8vLygo6ODvLw8pKSkYPLkyQgODoaDgwOz3M4oLCwMBQUFsLW1hbq6usgZ5KYR1IQQ0lZUABNC3hqjR49GZmYmnj17hnHjxuGPP/4AABgbG7c6uEEc3NzcYG9vD3t7exgaGiI7OxuZmZmIiIhAWloas1w%2BNTQ04NixYy2OnWYxCa6Jrq5ui9elpKSo0wchRGzoCAQh5K2ho6ODxMREuLi4oFu3bsjNzYWcnByz6WRNbt68ialTpwL43yS00aNHIyAggGkun4KDg3HkyBHo6upCRuZ/fypYf61v3LjB9OMTQghABTAh5C3i7%2B8PX19fmJmZYebMmXBycoK0tDRcXFyY5qqoqOD27dsYOHCg8Nrt27eF45E7ooyMDOzatQuDBw%2BWeHZJSQlSU1NRWFiIXr16wcbGBu%2B%2B%2B67E10EI6bioACaEvDWGDx%2BO33//HbKysnB2dsagQYPw%2BPFjmJmZMc11dXXFnDlz4OPjg/r6eqSlpSEmJgbOzs5Mc/nU2NiIjz76SOK5V65cgaenJ3R0dNC3b19cuXIFsbGxiIuLg4GBgcTXQwjpmOgMMCHkrcBxHPLz80V2AtPS0jBx4kRIS0szz9%2BzZw8SEhJQWFgIDQ0NODs7w9PTE126dGGezYfw8HCoq6vD29tborkeHh4YP348PDw8hNd%2B/PFHpKenIzExUaJrIYR0XFQAE0LavSdPnuDzzz%2BHmpoaNm/eDACoqKiAhYUF9PT0sH37dnTr1o3nVXYsrq6uyMnJgYKCAlRUVEQe%2B%2BWXX5jlGhsb4/Tp0yLnjuvq6jBy5EhcuHCBWS4hpHOhIxCEkHYvJiYGsrKyWLVqlfCaqqoqMjIy4Ovri%2B%2B//x6BgYHM8vnqiMCnadOmYdq0aRLPVVBQQHFxMbS1tYXXiouL0bNnT4mvhRDScdEOMCGk3ZswYQK2bduGfv36NXssNzcXAQEBOHbsGLP85cuXt9oRYdeuXcxyO6PIyEj8%2BeefCAoKQt%2B%2BfXH//n2sW7cOo0aNwpdffsn38gghHQTtABNC2r2KiooWi18AGDRoEMrLy5nm89kRQdKajpi8Cstdb39/fwgEAsydOxd1dXWQl5eHg4MDFixYwCyTENL5UAFMCGn3FBUVUVlZCWVl5WaPPXjwAAoKCkzz%2BeqIwIfXDRRh3QdYXl4eq1evRkhICB4%2BfAg1NTXmmYSQzocKYEJIu2diYoI9e/a0uPOYkJAAfX19pvk2NjaIi4uTeEcEPuzevZvX/Orqauzbtw%2Benp549OgR5s6dCxUVFYSEhEBDQ4PXtRFCOg46A0wIaffu3LkjHEVsbW0NdXV1lJWV4ejRo0hKSkJ8fDz09PSY5fPVEaEzWrx4MXJzc5GcnAw3NzeoqqpCXl4ejx8/RkxMDN/LI4R0EFQAE0LeCjk5OQgODsatW7cgJSUFjuPwwQcfYMWKFTA0NGSaffDgwVYfs7OzY5rd2VhaWuLAgQOQkpKCiYkJMjIyoKSkhFGjRiE7O5vv5RFCOgg6AkEIeSsMHz4cqampyM/Ph0AggLq6OrS0tCSSTUWu5FRXV0NJSQnp6enQ1taGhoYGamtr6RwwIUSsqAAmhLxVtLW1RXrEsuTt7Y3Y2Fi4u7u3WoBRGzTxGjhwIKKjo/H777/DwsICVVVVWL9%2BPT7%2B%2BGO%2Bl0YI6UCoACaEkFYYGBgAeD6djEjGypUrsWrVKigqKmL%2B/Pm4fv06srKysHHjRr6XRgjpQOgMMCGEEEII6VRoB5gQQl6juroaCQkJLY5CjoiI4GlVHUtsbCy8vb1fOYijo46dJoRIHhXAhBDyGkuWLMHFixdhbGwMWVlZvpfTIWVnZ8Pb27vVQRx0ExwhRJzoCAQhhLyGsbEx9u/fL7Gb7zo7juPQ2NgIaWlplJeXQ0VFBdLS0nwvixDSgXThewGEENLeycvL0xQyCblx4wYsLS1x7do1AMD27dsxYcIE3Llzh%2BeVEUI6EtoBJoSQ19i6dSvKysowf/78ZpPgiHi5u7vD0NAQc%2BfOhYyMDOrr67F161bk5ORgx44dfC%2BPENJBUAFMCCGvYWlpiaKiohbPoebm5vKwoo5rxIgRyM7OFvlaNzQ0YOTIkTQJjhAiNnQTHCGEvMbq1av5XkKnoaioiDt37kBHR0d4LT8/Hz169OBxVYSQjoYKYEIIeQ0jI6MWrwsEAgmvpOOzs7ODr68vZs2aBS0tLRQVFSEuLg729vZ8L40Q0oHQEQhCCHmNy5cvIzIyEqWlpcI%2BwHV1dRAIBLh69SrPq%2BtYGhoaEB0djUOHDqG8vBy9e/eGvb09Zs2aRZ0gCCFiQwUwIYS8hqOjI7S1taGkpIT8/HyYmZlh165d8PDwgJeXF9/LI4QQ8v9EBTAhhLzG0KFDkZWVhYKCAoSHh2Pnzp24dOkSQkJCcODAAb6X16E0NDTg2LFjLU7do0lwhBBxoTPAhBDyGj169EDXrl2hra2NW7duAQD09fVRWFjI88o6nuDgYBw5cgS6urqQkfnfnyiaBEcIEScqgAkh5DV0dHSQmJgIFxcXdOvWDbm5uZCTk6OijIGMjAzs2rULgwcP5nsphJAOjApgQgh5DX9/f/j6%2BsLMzAwzZ86Ek5MTpKWl4eLiwvfSOpzGxkZ89NFHfC%2BDENLB0RlgQgj5D549ewZZWVl06dIFly9fxuPHj2FmZsb3sjqc8PBwqKurw9vbm%2B%2BlEEI6MNoBJoSQV%2BA4Dvn5%2BXj33XeF1woKCjBx4kQeV9VxXbt2DTk5OYiJiWk2dvqXX37haVWEkI6GdoAJIaQVT548weeffw41NTVs3rwZAFBRUQELCwvo6elh%2B/bt6NatG8%2Br7FgOHjzY6mN2dnYSXAkhpCOjApgQQlqxZs0aXLp0CevXr4eqqqrwekVFBXx9fWFiYoLAwEAeV0gIIeRNUAFMCCGtmDBhArZt24Z%2B/fo1eyw3NxcBAQE4duwYDyvreJp22F%2BF%2BgATQsSFzgATQkgrKioqWix%2BAWDQoEEoLy%2BX8Io6rqysrFc%2BTi3nCCHiRAUwIYS0QlFREZWVlVBWVm722IMHD6CgoMDDqjqm3bt3870EQkgn0oXvBRBCSHtlYmKCPXv2tPhYQkIC9PX1JbwiQggh4kA7wIQQ0oo5c%2BbA3t4elZWVsLa2hrq6OsrKynD06FEkJSUhPj6e7yUSQgh5A3QTHCGEvEJOTg6Cg4Nx69YtSElJgeM4fPDBB1ixYgUMDQ35Xh4hhJA3QAUwIYT8B/n5%2BRAIBFBXV4eWlhbfyyGEENIGVAATQgghhJBOhW6CI4QQQgghnQoVwIQQQgghpFOhApgQQgghhHQqVAATQgghhJBOhQpgQgghhBDSqVABTAghhBBCOhUqgAkhhBBCSKfyf/8yzh2RCNo0AAAAAElFTkSuQmCC" class="center-img">
</div>
    