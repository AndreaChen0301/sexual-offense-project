# **Demographical Features and Sexual Offense in U.S.**

Inequality is always a hot topic in anywhere of the world, espeically in the U.S.. However, every time when people talk about inequality of the minority, they talk about race, mostly black, while ignoring a large portion of population who has been mistreated for decades--females. They endure inequality and social stereotype since in young ages. Sexual directive prints on baby onesies, stereotypical career advice for females, unequal opportunities  in academics, unreasonable limitations for females in workplaces, etc.. They exist everywhere; I can say no girl could say that she has not been mistreated in her entire life. 

Not everyone can name a person around them who are victim or offender of sexual offense, but it's easy to find a real-life example, or to say, criminal case. Notorious rapist Jeffery Eiberstain and Warren S. Jeffs of Keep Sweet: Pray and Obey; I can already name two just in US. Sexual offense is the worst form of malice agianst women. This project is tend to analyze the victim and offender profiles of sexual offenses and dig deeper into the demographical factors that might correlate with sexual offense, specifically in 2021.

### As the very beginning, it's important to see the trend of sexual offense cases in US in previous years, from 2013-2019.

![past year data](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/past%20rape%20data.png?raw=true)

### Observe a General Trend: Number of Rape Cases from 2013-2019 in US:

note: After 2019, rape is categorized as sexual offense under violent crime. They share the same definition.

![13-19 trend](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_6_1.png?raw=true)

The revised definition of rape includes both female and male victims (rape), sodomy, and sextual assult with an object, where are legacy definition only includes female victims (rape). The legacy rape rate increased 15.4% from 2013 to 2019.

### Feature Comparison Choropleth:

Taking only one of the demographical features--spending of policing correction per capita, compare it with the sexual offense rate of each state. The choropleth of the United State responding to sexual offense rate and policing correction spending per capita respectively in 2021. It gives a general idea of which states have higher or lower sexual offense rate, and what relationship might it has with policing spending.

![heatmap for sexual offense](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/heatmap1.png?raw=true)
![heat map for police](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/heatmap2.png?raw=true)

## **Victims Profile**

Getting closer to the victims of sexual offense, who are they? The age and sex distribution are included in order to find the out who is more likely be targeted.

### Victims Sex Distribution

![sex bar](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_57_1.png?raw=true)
![sex pie](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_57_2.png?raw=ture)

It's evident that victims are overwhelmingly females, made up 87.4% of all victims. Now we know the sex distribution. What about age? Are the young ladies more likely to be the target? Or the immatured, vulnerable underaged girls? 

### Victim Age Distribution

![age density](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_60_0.png?raw=true)

5 years for each age category. Left skewed curve shows that majority of the victims are in young ages, mostly between 11-15 who are vulnerable underaged teen and even children.

## **Offender Profile**

After the victims proflie, who are the offenders? What's their sex distribution, age distribution? Will these two parameters have the same pattern for victims and offenders? Besides sex and age, relationship with the victim and the offender drug+alcohol use are also included. Do the offenders know the victims? Or they are total strangers. For alcohol is always used as an justification for wrong doing, are the offenders sober when they commit crime?

### Offender Sex Distribution vs Victims Sex Distribution

![offender sex distribution](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_65_1.png?raw=true)

The result shows an opposite trend. The female victims outnumber the male victims while male offenders outnumber the female victims.

### Offender Age Distribution

![offenser age dis](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_68_1.png?raw=true)

Offender age distribution is also left skewed but smoother. Most of the offenders are teenagers and physically capable adults who are between 11-35.

### Relationship With Victims

![relationships](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_71_1.png?raw=true)

"Family member and other" category consists victims related to all offenders and victims related to at least one offenders. They together made up 80.5% of the cases, indicating most of the offenses are done by acquaintance. 

### Offender Use of Drug or Alcohol

![alcohol drug use](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_74_1.png?raw=true)

Nearly 90% of the sexual offense is commited when offenders are sober, free from any use of alcohol or drugs.

Notes:  All the data used in previous analysis are from FBI Crime Data Explorer


## **State-level Femographical Features**

The purpose of the project is to find out whether the demographic features will affect sexual offense rate. What are those features? I have decided to include 8 features: police spending, primary and secondary school teacher average salary, poverty rate, GDP per capita, education spending per pupil, drug overdose mortality rate, binge drinking rate, and bachelor degree attainment rate. Due to the avaliability, the valid data are only included from 2016 to 2019. The NAs, rows for the features missing data from 2020 due to COVID-19, are dropped.

### The below is a glance of the first few rows of the complete data of 8 features for 51 states (including Washington D.C.) from 2016-2019.

![data glance](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/full%20data.png?raw=true)

### To find out whether the demographic features will affect sexual offense rate, I fit the RandomForestRegressor model to the model data to chack the importance of each feature. The model data has everything expect for state and year variable.

![model data](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/model%20data.png?raw=true)

### Table and plot of r^2 score for each of the 8 features (importance):

![importance table](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/model%20importance.png?raw=true)
![importance plot](https://github.com/AndreaChen0301/sexual-offense-project/blob/main/project/data/images/output_87_1.png?raw=true)

It is clear that police correction spend per capita has a correlation with sexual offense rate of the state. The education, poverty, and other devient behavior features that I also expected to see a correlation with sexual assult rate, however, are not important to the model prediction.

Note: Data are from UCR crime database, Bureau of Justice Statistics and US Census Bureau, National Center for Education Statistics, kaggle https://www.kaggle.com/code/marshuu/poverty-rate-in-the-us-animation/input, Center for Disease Control and Prevention, National Survey on Drug Use and Health.

