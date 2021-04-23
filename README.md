<h2 align="center">Analyzing-The-Discovery-of-Handwashing</h2>

<p align='center'>
  <img width=350 height=400 style="float: left;margin:5px 20px 5px 1px" src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Ignaz_Semmelweis_1860.jpg/1200px-Ignaz_Semmelweis_1860.jpg">
</p>
<br>

![Forks](https://img.shields.io/github/forks/shukkkur/Analyzing-The-Discovery-of-Handwashing.svg)
![Stars](https://img.shields.io/github/stars/shukkkur/Analyzing-The-Discovery-of-Handwashing.svg)
![Watchers](https://img.shields.io/github/watchers/shukkkur/Analyzing-The-Discovery-of-Handwashing.svg)
![Last Commit](https://img.shields.io/github/last-commit/shukkkur/Analyzing-The-Discovery-of-Handwashing.svg) 

<p algin='right'>This is Dr. Ignaz Semmelweis, a Hungarian physician. He is thinking about <em>childbed fever</em> because in the early 1840s at the Vienna General Hospital as many as 10% of the women giving birth die from it. <br> He is thinking about it because he knows the cause of childbed fever: It's the contaminated hands of the doctors delivering the babies. And they won't listen to him and <em>wash their hands</em>!</p>
<p>Let's start by looking at the data that made Semmelweis realize that something was wrong with the procedures at Vienna General Hospital.</p>

```python
yearly = pd.read_csv('datasets/yearly_deaths_by_clinic.csv')
print(yearly)
```

|    | year | births | deaths |  clinic  |
|:--:|:----:|:------:|:------:|:--------:|
|  0 | 1841 | 3036   | 237    | clinic 1 |
|  1 | 1842 | 3287   | 518    | clinic 1 |
|  2 | 1843 | 3060   | 274    | clinic 1 |
|  3 | 1844 | 3157   | 260    | clinic 1 | 
|  4 | 1845 | 3492   | 241    | clinic 1 |
|  5 | 1846 | 4010   | 459    | clinic 1 |
|  6 | 1841 | 2442   | 86     | clinic 2 |
|  7 | 1842 | 2659   | 202    | clinic 2 |
|  8 | 1843 | 2739   | 164    | clinic 2 |
|  9 | 1844 | 2956   | 68     | clinic 2 |
| 10 | 1845 | 3241   | 66     | clinic 2 |
| 11 | 1846 | 3754   | 105    | clinic 2 | 

<p>
The table above shows the number of women giving birth at the two clinics at the Vienna General Hospital for the years 1841 to 1846. You'll notice that giving birth was very dangerous; an alarming number of women died as the result of childbirth, most of them from childbed fever.
</p>

<h3>1. The alarming number of deaths</h3>

```python
yearly['proportion_deaths'] = yearly.deaths / yearly.births

clinic_1 = yearly[yearly.clinic == 'clinic 1']
clinic_2 = yearly[yearly.clinic == 'clinic 2']

display(clinic_2)
```
<p>Let's zoom in on the proportion of deaths at Clinic 1.</p>

|    | year | births | deaths |  clinic  | proportion_deaths |
|:--:|:----:|:------:|:------:|:--------:|:-----------------:|
|  6 | 1841 | 2442   | 86     | clinic 2 | 0.035217          |
|  7 | 1842 | 2659   | 202    | clinic 2 | 0.075968          |
|  8 | 1843 | 2739   | 164    | clinic 2 | 0.059876          |
|  9 | 1844 | 2956   | 68     | clinic 2 | 0.023004          |
| 10 | 1845 | 3241   | 66     | clinic 2 | 0.020364          | 
| 11 | 1846 | 3754   | 105    | clinic 2 | 0.027970          |

<h3>2. Death at the clinics</h3>
<p>If we now plot the proportion of deaths at both Clinic 1 and Clinic 2 we'll see a curious pattern…</p>

```python
ax = clinic_1.plot(x='year', y='proportion_deaths', label='Clinic 1')
clinic_2.plot(x='year', y='proportion_deaths', label='Clinic 2', ax=ax)

plt.ylabel("Proportion deaths")
plt.show()
```
<p align="center">
  <img src="https://github.com/shukkkur/Analyzing-The-Discovery-of-Handwashing/blob/5f6383d85e37b1ce04e9522473065c0084fefd5f/datasets/img1.png" />
</p>

<p>The only difference between the clinics was that many medical students served at Clinic 1, while mostly midwife students served at Clinic 2. While the midwives only tended to the women giving birth, the medical students also spent time in the autopsy rooms examining corpses. </p>
<p>Semmelweis started to suspect that something on the corpses spread from the hands of the medical students, caused childbed fever. So in a desperate attempt to stop the high mortality rates, he decreed: <em>Wash your hands!</em> This was an unorthodox and controversial request, nobody in Vienna knew about bacteria at this point in time. </p>

```python
monthly = pd.read_csv('datasets/monthly_deaths.csv', parse_dates=['date'])

# Calculate proportion of deaths per no. births
monthly["proportion_deaths"] = monthly.deaths/monthly.births

print(monthly.head())
```

|   |    date    | births | deaths | proportion_deaths |
|:-:|:----------:|:------:|:------:|:-----------------:|
| 0 | 1841-01-01 | 254    | 37     | 0.145669          |
| 1 | 1841-02-01 | 239    | 18     | 0.075314          |
| 2 | 1841-03-01 | 277    | 12     | 0.043321          |
| 3 | 1841-04-01 | 255    | 4      | 0.015686          |
| 4 | 1841-05-01 | 255    | 2      | 0.007843          |

<h3>3. The effect of handwashing</h3>

<p>Starting from the summer of 1847 the proportion of deaths is drastically reduced and, yes, this was when Semmelweis made handwashing obligatory.<br>The effect of handwashing is made even more clear if we highlight this in the graph. </p>

 Date when handwashing was made mandatory
handwashing_start = pd.to_datetime('1847-06-01')

```python
before_washing = monthly[monthly.date < handwashing_start]
after_washing = monthly[monthly.date >= handwashing_start]

ax = before_washing.plot(x='date', 
              y='proportion_deaths', label='Before Washing')
after_washing.plot(x='date',y='proportion_deaths', label='After Washing', ax=ax)
```

<p align='center'>
  <img style="float: left;margin:5px 20px 5px 1px" src="https://github.com/shukkkur/Analyzing-The-Discovery-of-Handwashing/blob/4b3f002c0a63035531351ddd2274c88987a9512a/datasets/img2.png">
</p>

<h3>4. More handwashing, fewer deaths? </h3>

<p><Again, the graph shows that handwashing had a huge effect. How much did it reduce the monthly proportion of deaths on average?/p>
 
```python
before_proportion = before_washing.proportion_deaths
after_proportion = after_washing.proportion_deaths
mean_diff =  after_proportion.mean() - before_proportion.mean()

print(mean_diff)
```
<p><code>-0.08395660751183336</code><br>It reduced the proportion of deaths by around 8 percentage points! </p>

<h3>5. A Bootstrap analysis of Semmelweis handwashing data</h3>
<p>To get a feeling for the uncertainty around how much handwashing reduces mortalities we could look at a confidence interval (here calculated using the bootstrap method).</p>

```python
# A bootstrap analysis of the reduction of deaths due to handwashing
boot_mean_diff = []
for i in range(3000):
    boot_before = before_proportion.sample(replace=True,n=len(before_proportion))
    boot_after = after_proportion.sample(replace=True,n=len(after_proportion))
    boot_mean_diff.append(boot_after.mean()-boot_before.mean())

# Calculating a 95% confidence interval from boot_mean_diff 
confidence_interval = pd.Series(boot_mean_diff).quantile([0.025, 0.975] )
confidence_interval
```
<p><code>0.025   -0.101156</code><br>
  <code>0.975   -0.066840</code><br>
</p>
<p>So handwashing reduced the proportion of deaths by between 6.7 and 10 percentage points, according to a 95% confidence interval. All in all, it would seem that Semmelweis had solid evidence that handwashing was a simple but highly effective procedure that could save many lives.</p>

<h3> Conclusion </h3>

```python
# The data Semmelweis collected points to that:
doctors_should_wash_their_hands = True
```

