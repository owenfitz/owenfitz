---
layout: wide_default
---    

# Analysis Report

## Variable Descriptions and Reasonings

For my first risk I chose to monitor Environmental, Social, and Governance (ESG) risks that a company may face. I chose to seperate each portion of this risk into three categories, Environmental risks, Social risks,and Governance risks. For Environmental risks I chose to use the key words: 'environment| environmental' paired with 'sanction | sanctions' with a range of 15. Social risks, the "S" in "ESG", used the keywords 'social|diversity|diverse' paired with 'law|regulation|sanction' with the same range of 15. For the last portion of ESG, I looked at governance in which I included the key words, 'governance|government' with 'regulation|regulate' with the range of 15. I chose these risks because today it has a large impact on whether investors choose to work with your company or not. Many investors choose a company with high ESG as it shows they are investing into their community and the wider range or stakeholders. I would like to see if companies back in 2020 were just as likely to see ESG as an issue as they do today. The measure of the 'Environmental' variable within the data set was a max of 4, the mean being 0.104137, and a standard deviation of 0.383411. I did not see as many 'hits' as I would like but it does show that companies back then did not have their interests on that issue yet. For my 'Social' measures, the statistics are as follows, the max was 3 hits, with a mean of 0.066038, and a standard deviation of 0.286905. Going through each compnent of this risk it seems as though companies did not have their interests on this subject, as it quite well could be a product stemming from the pandemic. For the final portion of the risk, 'Governance', the matches within the data are a max of 22, a mean of 1.260431, standard deviation of 2.000708, the median amount of hits was 1 and at 75%, 2. The 'Governance' portion of the risk I believe produced more hits due to the language I outlined within the keywords.     

The second risk I chose to measure was Competition risks, this was measured using 'increase|increased|greater|more' paired with 'competitors|competition' with the range being 15 as well. I chose to use various synonyms of 'increase' for my first keyword in order to cast a larger net and just used variations of 'competiton' to pair with the first set of keywords. I wanted to measure if any increased competition had a negative impact or any impact at all on the returns for the selected timespan. I figured that if there was copious usage of those words string together there may be a reflecion of the competition in lower returns. The matches I recieved with this risk were an average of 7.949442, a standard deviation of 4.941328, and a max of 33. This risk recieved many hits with a large range, at 25%, 4 hits, median of 7, and at 75% there were 11.  

My final risk I chose to measure was Supply Chain risks companies may face. To measure supply chain issues a company may experience I used 'limited|reduced' for my first set of keywords and then used a range of 15 with 'supplies|supply|suppliers'. I chose the first set to characterize language a company may used to reflect losing resources or shortage of materials. The second set of keywords were chosen soley as variations of 'supply'. I chose to measure supply chain as my final risk because now and looking back into the pandemic there were serious supply chain issues thatb resulted in many shortages of products. I would like to see if any company that mentions issues with supplies or suppliers may have experienced negative returns for the start of the pandemic. This risk did not recieve as many matches as the previous but still produced a good sample, the mean was 1.081645 with a standard deviation of 1.606916 with a max amount of hits of 9. I hope that since not all firms had matches with the keywords, that those who did will possibly have lower returns at the start of the pandemic, thus foreshadowing greater supply chain issues in the future. 


### Examples of matches and non-matches

Using the keywords for my competition risk, 'increase|increased|greater|more' paired with 'competitors|competition', I pasted below an example of a match:

Competition may require us to reduce our prices below our normal selling prices or increase our promotional spending, 

Here is an example of a non-match:

The principal methods of competition in our business include customer service, product offerings, availability, quality, price and store location.



```python

```

## Final Sample: Combined Data from risk measurement and return


```python
import glob
import random
import re
from datetime import datetime
from io import BytesIO
from urllib.request import urlopen
from zipfile import ZipFile

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import pandas_datareader as pdr
import seaborn as sns
from bs4 import BeautifulSoup
from near_regex import NEAR_regex
from sec_edgar_downloader import Downloader
from tqdm import tqdm
```


```python
#4
SP_returns_csv = 'output/sp_week_return.csv'
SP_returns = pd.read_csv(SP_returns_csv)
SP_returns.describe()
```

    /Users/owenfitzgerald/Documents/FIN377/anaconda3/lib/python3.9/site-packages/IPython/core/interactiveshell.py:3444: DtypeWarning: Columns (20) have mixed types.Specify dtype option on import or set low_memory=False.
      exec(code_obj, self.user_global_ns, self.user_ns)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CIK</th>
      <th>ESG1</th>
      <th>ESG2</th>
      <th>ESG3</th>
      <th>Comprisk</th>
      <th>SCMrisk</th>
      <th>permno</th>
      <th>date</th>
      <th>prc</th>
      <th>shrout</th>
      <th>vwretd</th>
      <th>R</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.826820e+05</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
      <td>1.826820e+05</td>
      <td>182682.000000</td>
      <td>1.826820e+05</td>
      <td>182682.000000</td>
      <td>182682.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>7.817034e+05</td>
      <td>1.260431</td>
      <td>0.066038</td>
      <td>0.104137</td>
      <td>7.949442</td>
      <td>1.081645</td>
      <td>54558.663968</td>
      <td>2.019392e+07</td>
      <td>135.808215</td>
      <td>5.823751e+05</td>
      <td>0.000737</td>
      <td>-0.121709</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.418076e+05</td>
      <td>2.000708</td>
      <td>0.286905</td>
      <td>0.383411</td>
      <td>4.941328</td>
      <td>1.606916</td>
      <td>29946.323912</td>
      <td>4.586563e+03</td>
      <td>225.318891</td>
      <td>1.021832e+06</td>
      <td>0.017511</td>
      <td>0.090576</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.800000e+03</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>10104.000000</td>
      <td>2.019010e+07</td>
      <td>3.120000</td>
      <td>3.590000e+03</td>
      <td>-0.118168</td>
      <td>-0.610145</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.721000e+04</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>22111.000000</td>
      <td>2.019052e+07</td>
      <td>50.270000</td>
      <td>1.421000e+05</td>
      <td>-0.003516</td>
      <td>-0.159176</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>8.821840e+05</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>60097.000000</td>
      <td>2.019100e+07</td>
      <td>88.140000</td>
      <td>2.841000e+05</td>
      <td>0.001289</td>
      <td>-0.106636</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.136869e+06</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>11.000000</td>
      <td>2.000000</td>
      <td>84373.000000</td>
      <td>2.020022e+07</td>
      <td>150.660000</td>
      <td>5.728190e+05</td>
      <td>0.006696</td>
      <td>-0.064245</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.757898e+06</td>
      <td>22.000000</td>
      <td>3.000000</td>
      <td>4.000000</td>
      <td>33.000000</td>
      <td>9.000000</td>
      <td>93436.000000</td>
      <td>2.020063e+07</td>
      <td>4037.770000</td>
      <td>9.814197e+06</td>
      <td>0.091556</td>
      <td>0.117748</td>
    </tr>
  </tbody>
</table>
</div>



I ran into issues merging my data sets as my second set created to produce the returns for the week looks great however after I merge many unwanted rows are included. This issue led to each firm repeating multiple times and does not skew the data for returns for the specified week. 

Due to the issues I encountered merging the data for the following statistics may be skewed due to the larger data sample. The number of firms included within this data set are 350, the average return during the onset of COVID was -0.121709 with the largest being 0.117748 and the min being -0.610145. Many of the firms, regardless of their sector, did not experience great returns for that week and poses a threat to the analysis between the risks and returns. In order to combat this issue, we will have to look at firms with outlier-type returns in order to substantially prove the two are correlated. 

## ESG Risk scatterplots with regressions



```python
ESGret1 = sns.regplot(x="ESG3", y = "R", data= SP_returns).set(title = "Environment Risks and Return (F,M,A 2020)", xlabel = "Environmental Risk", ylabel = "Returns")
plt.legend()
```

    No handles with labels found to put in legend.





    <matplotlib.legend.Legend at 0x7f7cfeb615e0>




    
!images/[png](output_11_2.png)
    


After analysis of the scatterplot with the regression, it appears as though companies with more hits associated with the keys words revolving environmental regulations experienced worse returns than those with less or no matches. As the regression is downward sloping, it is very telling that there may be some correlation between mentioning environmental regulations and having worse returns. Those companies that write more about environmental regulations may be in the energy sector where limitations on production may hurt their returns.  


```python
ESGret2 = sns.regplot(x="ESG2", y = "R", data= SP_returns).set(title = "Social Risks and Return (F,M,A 2020)", xlabel = "Social Risk", ylabel = "Returns")
plt.legend()
```

    No handles with labels found to put in legend.





    <matplotlib.legend.Legend at 0x7f7cfe919be0>




    
!images/[png](output_13_2.png)
    


In this case with Social risk, it appears as though there may be little to no correlation between having the keywords, 'social|diversity|diverse' paired with 'law|regulation|sanction', mentioned and having worse returns. I believe with most of these ESG risks, there may be no reflection in the returns due to the time we chose and the circumstances of the situation.


```python
ESGret3 = sns.regplot(x="ESG1", y = "R", data= SP_returns).set(title = "Governance Risks and Return (F,M,A 2020)", xlabel = "Governance Risk", ylabel = "Returns")
plt.legend()
```

    No handles with labels found to put in legend.





    <matplotlib.legend.Legend at 0x7f7cfe956b80>




    
!images/[png](output_15_2.png)
    


Analyzing the scatterplot and regression for governance risk, the regression line is on a positive slope, indicating that the companies that mentioned the keywords more experienced better returns. However, this regression slope is not extreme and is only a gradual rise. As stated previously, I hypothesize that the reason for my ESG risks to not be as much of a factor as it is today is due to the time of the sample. Many investors and companies started using ESG as a factor during the pandemic as many individuals' social awareness increased over the pandemic resulting in the support of companies who invest back into their stakeholders and community.

## Competition Risk scatterplot with regression


```python
CompRet = sns.regplot(x="Comprisk", y = "R", data= SP_returns).set(title = "Competition Risks and Return (F,M,A 2020)", xlabel = "Competition Risk", ylabel = "Returns")
plt.legend()
```

    No handles with labels found to put in legend.





    <matplotlib.legend.Legend at 0x7f7cfba06460>




    
!images/[png](output_18_2.png)
    


The scatterplot and regression line show that much of the correlation, or lack there of, between competition risk and the returns is almost flat but slightly downward sloping. Much of the data for returns is distributed quite evenly when the company has between 0-15 matches. After 15 matches, the few companies remaining have returns similar to the rest of the sample, however, there are a few outliers past 15 matches that have returns that are below average. This could be seen as correlation between experiencing more competition risk and having lower returns.

## Supply Chain Risk scatterplot with regresson


```python
SCMRet = sns.regplot(x="SCMrisk", y = "R", data= SP_returns).set(title = "Supply Chain Risks and Return (F,M,A 2020)", xlabel = "Supply Chain Risk", ylabel = "Returns")
plt.legend()
```

    No handles with labels found to put in legend.





    <matplotlib.legend.Legend at 0x7f7cfeac8b80>




    
!images/[png](output_21_2.png)
    


The scatterplot and regression line for Supply Chain risks and returns shows a negative slope. This means that the companies who had more matches associated with supply risks experienced worse returns than those who did not. The two companies with the largest amount of matches have much different returns, one of the companies is performing similar to how the regression predicted, the other is performing much better with a positive return for the week. This could mean that in this time-frame the correlation between supply chain risks and having lower returns is non-existent, however, some of the other companies who had high match counts experienced returns much worse than the rest. Larger time-span would be needed in order to accurately decide whether the two are correlated.


```python

```
