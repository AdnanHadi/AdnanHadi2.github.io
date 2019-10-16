---
layout: post
title:  "Titanic - Predicting the odds of Survival"
date:   2019-10-04 21:30:41 -0400
categories: jekyll update
---
![Titanic](/assets/img/Titanic.jpg)


# Introduction
RMS Titanic was a British passenger liner that met an unfortunate fate in 1912 after striking an iceberg during her maiden voyage. It had more that 2,000 passengers and more than two thirds lost their lives. This topic of who survived and what factors contributed to their survival became the theme of a [Kaggle competition][Kaggle]. Kaggle released a dataset for Titanic passengers with various informtaion about them such as Age, Gender, lodging class, etc. In this article we discuss how Machine Learning can be used to predict the survival of passengers.

# Tools required
For this competition I used the following tools:
- Anaconda for running Jupyter Notebooks
- Python 3 
- Visualization libraries: Matplotlib, Seaborn
- Data Analytics libraries: Pandas, Numpy
- ML libraries: SciKit Learn

# Data Exploration
The data provided by Kaggle is split into train and test data. As a best practice, we keep the test set aside and don't look at it. But in this case, the test set is actually the submission set and not the set used to tune the model.

A quick peek at the data reveals the following:
- Data available: 891 passengers
- features available: 10
- missing data in feature: **age** , **cabin**, **embarked**
- Age: min=0.42, max=80 yrs

And the 10 features that are available are:
- **survival**	Survival	    0 = No, 1 = Yes
- **pclass**	    Ticket class	1 = 1st, 2 = 2nd, 3 = 3rd
- **sex**	        Sex	
- **Age**	        Age in years	
- **sibsp**	    # of siblings / spouses aboard the Titanic	
- **parch**	    # of parents / children aboard the Titanic	
- **ticket**	    Ticket number	
- **fare**	    Passenger fare	
- **cabin**	    Cabin number	
- **embarked**	Port of Embarkation	C = Cherbourg, Q = Queenstown, S = Southampton

The first feature **survivial** is actually our output value or label. The other 9 features are our predictors. We can better understand the data with some visualizations.

# Data Visualization
Using the Matplotlib library we can plot how different features are correlated with the Survival rate.

![Survivor Plots](/assets/img/Survivor_charts.JPG)

These plots are telling us several things. Firstly, the survival rate was very low - around 35%. The second plot shows the age distribution vs the survival rate. The distributions shows that the ages between 60-80 had lower survival rates. This chart is made with the scatter plot in matplotlib and setting the alpha to a low value to get the translucency effect.
{% highlight ruby %}
plt.subplot2grid((2,3), (0,1))
plt.scatter(x=titanic_df.Survived, y=titanic_df.Age, alpha=0.1)
plt.title("Survivor Vs Age")
{% endhighlight %}
<center><h4><i>55% travelled in 3rd Class</i></h4></center>

From the Class Percentage chart we clearly see that the majority population was travelling in 3rd class and mostly embarked from Southampton. The KDE or Kernel Density Estimate plot is particularly useful to view the distribution of one feature with the other. In this case we can see that most 3rd class passengers were young and most 1st class passengers were in the mid 30's.

The code for KDE is as follows:

{% highlight ruby %}
# Class KDE (Kernel Desnisty Estimate)
plt.subplot2grid((2,3),(1,0),colspan=2)
for x in [1,2,3]:
    titanic_df.Age[titanic_df.Pclass==x].plot(kind="kde")
plt.title("Class/Age Kernel Density Estimate")
plt.legend(("1st","2nd","3rd"))
{% endhighlight %}

It is also interesting to see how the survivor rate differed with gender. Similar to the above plots we can generate survivor vs gender plots as shown below:
![Gender Plots](/assets/img/Gender_charts.JPG)

The plots show that `60%` of the passengers were Male and only `25%` of them survived. Whereas `40%` of the passengers that are female, `70%` survived. 

The Female Survivor % can be plotted with a bar chart as follows:
{% highlight ruby %}
# Female survivors
plt.subplot2grid((3,3),(1,1))
titanic_df.Survived[titanic_df.Sex=="female"].value_counts(normalize=True).sort_index().plot(kind="bar",alpha=0.5, color=female_col)
plt.title('Female Survivor %')
{% endhighlight %}

<center><h4><i>70% of the Female Survived</i></h4></center>

Once again we plotted the KDE, this time to see the relation between survival and class. We can see that majority of the deceased were from 3rd Class and conversely majority of the survivors were from 1st Class.

We can also use `Matplotlib` to plot a pie-chart of the Survivors by Class.

{% highlight ruby %}
# Data to plot
labels = '2', '1', '3'
total = titanic_surv['Pclass'].count()
surv_class = titanic_surv.groupby('Pclass')['Pclass'].count()
sizes = [surv_class[2], surv_class[1], surv_class[3]]
colors = ['gold', 'yellowgreen', 'lightcoral']
explode = (0.01, 0.1, 0.01)  # explode 1st slice

# Plot
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
autopct='%1.1f%%', shadow=True, startangle=140)
plt.title('Titanic Survivors by Ticket Class')
plt.axis('equal')
plt.show()
{% endhighlight %}

![Survivor Pie](/assets/img/Survivor_pie.JPG)


[Kaggle]: https://www.kaggle.com/c/titanic