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

![Survivor Plots](/assets/img/Survivor_charts.jpg)
These plots are telling us several things. Firstly, the survival rate was very low - around 35%. The second plot shows the age distribution vs the survival rate. The distributions shows that the ages between 60-80 had lower survival rates. This chart is made with the scatter plot in matplotlib and setting the alpha to a low value to get the translucency effect.
{% highlight ruby %}
plt.subplot2grid((2,3), (0,1))
plt.scatter(x=titanic_df.Survived, y=titanic_df.Age, alpha=0.1)
plt.title("Survivor Vs Age")
{% endhighlight %}

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

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
[Kaggle]: https://www.kaggle.com/c/titanic