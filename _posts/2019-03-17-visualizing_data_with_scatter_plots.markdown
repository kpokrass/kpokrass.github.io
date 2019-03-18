---
layout: post
title:      "Visualizing Data with Scatter Plots"
date:       2019-03-17 20:36:06 -0400
permalink:  visualizing_data_with_scatter_plots
---


**What is it?**

A scatter plot is a two-dimensional graphing technique that uses markers to indicate the intersection of two variables on a cartesian coordinate system where one variable is assigned to the x-axis and another variable is assigned to the y-axis. 

**When to use it?**

Scatter plots are best used to examine the relationship or pattern between two variables. Therefore, scatter plots can be used early on in the exploratory analysis of a data set to assess the correlation between the variables within the data set. In essence, the distribution of the markers not only display *if* there is a relationship, but also *what kind* of relationship exists between the variables.

**How to interpret it?**

Traditionally, a scatter plot is organized where the independent variable is assigned to the x-axis and the dependent variable is assigned to the y-axis. As suggested earlier, the distribution of the markers displays the relationship between the variables. This relationship can be further evaluated in terms of direction, shape, and strength. 

The direction of the relationship between the variables can be either positive or negative. If the relationship is positive, larger values of one variable are associated with larger values of the other variable. Conversely, if larger values of one variable are associated with smaller values of the other variable, the relationship would be described as negative between the two variables.

The shape of the relationship and be determined by inspecting the pattern of the distribution of the markers. If the markers cluster in such a way that they begin to resemble a line, the relationship can be described as linear. A linear relationship between two variables indicates that the variables change at roughly the same rate. If the markers cluster in such a way that they begin to resemble a curve, the relationship can be described as curvilinear. A curvilinear relationship between two variables indicates that one of the variables does not change at a constant rate.

A formal assessment of the strength of a relationship between two variables requires some math to calculate the correlation coefficient, but a crude assessment can be accomplished by visually inspecting how tightly the markers cluster around their relational shape (i.e. linear or curvilinear). The tighter the markers conform to the relational shape, the stronger the relationship.

**Basic Scatter Plot**

The block below outlines the code for creating a basic scatter plot. It is utilizing the seaborn library as it allows for increased customization from matplotlib alone. Click the [link](http://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot) to view seaborn’s documentation for scatter plots.

```
# Import necessary libraries
import seaborn as sns
import matplotlib.pyplot as plt

# Set background style of graph
sns.set(style=’whitegrid’)

# Declare figure size
figure = plt.figure(figsize=[width, height])

# Declare graph type, x and y variables by their column names, and name of data set containing the data
sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’)

# Set the title of the graph
plt.title(‘Title of Graph’)

# Set the title for the x-axis
plt.xlabel(‘Label of Independent Variable’)

# Set the title for the y-axis
plt.ylabel(‘Label of Dependent Variable’)

# Display the graph
plt.show()
```

**Modifications**

Changing the color of the markers by passing the color keyword. Change the translucence of the markers by passing the alpha keyword (range from 0 to 1, where numbers closer to 1 indicate greater opacity). Change the style of the markers by passing the marker keyword:

```
sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’, color=’color’, alpha=0.6, marker=”+”)
```

Group the markers by a third (typically categorical) variable by passing the hue keyword. This will assign colors to the markers based on their categorical value. The style of the hue colors can be customized by declaring a color map and passing the palette keyword. It will also be helpful to display a legend to denote what color goes with each categorical value. Seaborn will include a legend in the output if the hue keyword is passed, but it can also be accomplished by including ‘plt.legend()’ in the code block or passing the ‘legend’ keyword:

```
sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’, hue=’third_variable’, legend=’full’)

plt.legend()
```

With custom colors (view seaborn’s tutorial for choosing color palettes [here](https://seaborn.pydata.org/tutorial/color_palettes.html)).

```
cmap = sns.diverging_palette(16, 226, n=12)

sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’, hue=’third_variable’, palette=cmap)
```

An effect similar to hue can be accomplished by passing the style keyword for the third variable. This will differentiate the markers by shape instead of colors:

```
sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’, style=’third_variable’)
```

Differentiate the markers by size relative to a third (typically continuous) variable by passing the size keyword. It may also be helpful to set hue and size to the same variable to make it easier to interpret. Again, a legend is necessary to denote what the sizes of the markers mean:

```
sns.scatterplot(x=’independent_variable’, y=’dependent_variable’, data=’data_set’, size=’third_variable’)
```

To emphasize a trend between multiple variables, it may be helpful to arrange two plots side-by-side. For this, it will be necessary to include ‘plt.subplot(rows, columns, position)’ in the code block:

```
# Left figure
plt.subplot(1,2,1)
sns.scatterplot(x=’independent_variable_1’, y=’dependent_variable’, data=’data_set’)
plt.title('Title of Left Graph')
plt.xlabel('Left Graph x-axis')
plt.ylabel('Left graph y-axis')

# Right figure
plt.subplot(1,2,2)
sns.scatterplot(x=’independent_variable_2’, y=’dependent_variable’, data=’data_set’)
plt.title('Title of Right Graph')
plt.xlabel('Right Graph x-axis')
plt.ylabel('Right graph y-axis')
```

A scatter matrix allows the user to view scatter plots for every variable within a data set displayed in matrix form where each row and each column corresponds to a different variable. Every variable within the data set will be included in the matrix by passing just the data set. Or, specific variables can be selected by passing them as a list into the vars keyword. The data in the matrix can also be grouped by hue or marker style. Additionally, the distribution density of each variable is displayed along the diagonal of the matrix. This documentation for seaborn’s scatter matrix can be viewed [here](https://seaborn.pydata.org/generated/seaborn.pairplot.html):

```
sns.pairplot(data_set, vars=[‘variable_1’, ‘variable_2’, ‘variable_3’, ‘variable_4’])
```

**Strengths/Weaknesses**

Strengths
* Easy to see the relationship between two variables
* Range of the data and suspicious outliers can be identified

Weaknesses
* The number of variables that can be included in the graph is limited -- Best for two variables, but more may be added in a restricted capacity
* Only continuous variables should be used -- Categorical variables can be included by utilizing different color/style markers
* Can fail to display the patterns in larger data sets as too many markers on a plot will resemble an amorphous blob


**Sources**

https://www.westga.edu/academics/research/vrc/assets/docs/scatterplots_and_correlation_notes.pdf

https://chartio.com/learn/dashboards-and-charts/what-is-a-scatter-plot/

