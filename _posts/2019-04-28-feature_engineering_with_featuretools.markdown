---
layout: post
title:      "Feature Engineering with FeatureTools"
date:       2019-04-29 02:01:20 +0000
permalink:  feature_engineering_with_featuretools
---


As data scientists, we are told time and time again that a model is only as good as the data it is given. In short, garbage in results in garbage out. But what makes data “good?” Whether it is tabular, a collection of images, text documents, or in any other form, data is simply the starting point of the project. It is neither inherently good nor bad. It just either exists or it does not exist. And let’s all be honest with ourselves for a minute, based on the explosion of data center construction and the formation of numerous companies (and occupations) surrounding data management, the data exists. You may just have to be a little creative to find it.

The creativity referenced here is the art of feature engineering. Feature engineering is a process where in the data scientist uses the features already present in the data to create new features by intelligently combining or decomposing the original features to better describe the dataset and its trends for a particular machine learning algorithm. Since all supervised machine learning algorithms generate predictive models based on the data they are given, feature engineering can be thought of as “teaching” the algorithm in a style that is more informative than what the raw data alone can present.

> "Much of the success of machine learning is actually success in engineering features that a learner can understand." - Scott Locklin ([Neglected machine learning ideas](https://scottlocklin.wordpress.com/2014/07/22/neglected-machine-learning-ideas/))
> 

With this “learner” analogy, we can begin to understand the importance of feature engineering and formulate a general answer to our “what makes data good” question posed at the beginning of this discussion. Good data is whatever data leads to the generation of a good model. Thus, by transforming the original data into better (i.e. more informative) data via feature engineering, there will be a positive influence on the predictive capability of a model because the machine learning algorithms are better able to learn the data in its “engineered” form.

> "Coming up with features is difficult, time-consuming, and requires expert knowledge. Applied machine learning is basically feature engineering." - Andrew Ng ([Machine Learning and AI via Brain Simulations](https://forum.stanford.edu/events/2011/2011slides/plenary/2011plenaryNg.pdf))
> 

However, the goal of “improving model performance” is too broad to truly understand feature engineering. At its core, this process is attempting to pinpoint key information and illuminate trends in the raw data by incorporating domain knowledge. Including knowledge of the field and understanding the scope of the project is what allows for the intelligent combination of the features from the original dataset and is what ultimately leads to higher quality models. 

Now that we have a firm understanding of what a data scientist is attempting to accomplish with feature engineering, let’s look at some methods that can be used to create new features.

1. Indicator variables can be used to define a threshold, group, or event before the data is fed to the machine learning algorithm to help it “focus” on key information.
2. Feature interactions can be created by using any of the four basic mathematical operations between two or more of the existing features.
3. Altering how the data in a feature is represented, rather than creating an entirely new feature, can offer a more informative presentation of the data to the algorithm.

These examples may seem frustratingly vague, however more explicit examples would do a disservice to the art of feature engineering. This process needs to be individually tailored to each project and thus there is no one right or wrong way to go about engineering features. It requires time, effort, and a sound understanding of what the project is attempting to answer or solve. One of the top ranked Kaggle data scientists was quoted after a recent competition win as saying “The algorithms we used are very standard for Kagglers. […] We spent most of our efforts in feature engineering ([Xavier Conort](http://blog.kaggle.com/2013/04/10/qa-with-xavier-conort/))."

Because feature engineering is such an important, yet time consuming, step in the data science process, it is reasonable to assume that some very smart individuals would attempt to automate the process. But as this discussion has already stated, the features need to be combined or altered in an intelligent way in order to have a positive effect on the quality of the model. For this we will turn our discussion to FeatureTools, an open source library for automated feature engineering of relational datasets (The official documentation can be [found here](https://docs.featuretools.com/index.html). FeatureTools utilized deep feature synthesis (DFS), which is an algorithm that sequentially applies mathematical functions along the relationships within the data to generate features. The details of how this algorithm works are beyond the scope of this discussion, but the scholarly article providing an in-depth analysis of its development can be [found here](http://www.jmaxkanter.com/static/papers/DSAA_DSM_2015.pdf).

To use FeatureTools, each table in the database needs to be represented as an “entity.” From there, each entity needs to be assigned to an “entity set” where the relationship between each entity in the entity set is defined.

For example, let’s say we have three tables of data; Table1, Table2, Table3. Each of these tables must have a unique index (id_1, id_2, id_3) that allows for the joins (in FeatureTools these are called relationships) with the other tables in the entity set. The code block below outlines this process.

```
# Create the entity set
Entity_set = ft.Entity_set(id=’name_of_set’)

# Add the tables (as dataframes) to the entity set
Entity_set.entity_from_dataframe(entity_id = ‘name_of_entity’,
				          Dataframe = df_name,
				          Index = ‘index_col’)
```

So for our example data:

```
# Create the entity set
Entity_set = ft.Entity_set(id=’example’)

# Add the tables (as dataframes) to the entity set
Entity_set.entity_from_dataframe(entity_id = ‘entity_1’,
				          Dataframe = Table1,
				          Index = ‘id_1’)
Entity_set.entity_from_dataframe(entity_id = ‘entity_2’,
				          Dataframe = Table2,
				          Index = ‘id_2’)
Entity_set.entity_from_dataframe(entity_id = ‘entity_3’,
				          Dataframe = Table3,
				          Index = ‘id_3’)
```

The output should resemble something similar to the entity set summary shown below.

```
Entityset: example
  Entities:
    entity_1 [Rows: 27, Columns: 4]
    entity_2 [Rows: 27, Columns: 4]
    entity_3 [Rows: 27, Columns: 4]
  Relationships:
    No relationships
```

Notice how the entity set summary states that there are no relationships between the tables (now called entities within the FeatureTools entity set). These relationships need to be added manually. The syntax of a FeatureTools relationship is parent, column to join on, child, column to join on. The ‘parent’ to ‘child’ relationship is one-to-many. The following code block generalizes this process.

```
# Create the relationships
relate = ft.Relationship(entity_set['parent_entity']['id_1'], entity_set['child_entity']['id_2'])
es.add_relationship(relate)
```

Specific to our example:

```
# Create the relationships
relate1 = ft.Relationship(entity_set['entity_1']['id_1'], entity_set['entity_2']['id_2'])
es.add_relationship(relate1)

relate2 = ft.Relationship(entity_set['entity_3']['id_3'], entity_set['entity_2']['id_2'])
es.add_relationship(relate2)

# View the entity set summary
Entity_set



Entityset: example
  Entities:
    entity_1 [Rows: 27, Columns: 4]
    entity_2 [Rows: 27, Columns: 4]
    entity_3 [Rows: 27, Columns: 4]
  Relationships:
    entity_2.id_2 -> entity_1.id_1
    entity_2.id_2 -> entity_3.id_3

```

The entity set is now ready to undergo deep feature synthesis using the FeatureTool primitives. There are two types of built-in feature primitives; aggregations and transformations. Aggregations combine the data in the tables to get values such as min, mean, max, and count. Transformations change the data in the tables based on a function. You can use `ft.primitives.list_primitives() ` to get a list of all the available primitives, their type, and a short description of what they calculate. For our example, we will not specify particular primitives, but the code block below will outline how to perform deep feature synthesis with specifications.

```
# Deep Feature Synthesis
feature_matrix, feature_names = ft.dfs(entityset=entity_set,
                                       target_entity='entity_1',
		            agg_primitives = list_of_aggs,
		            trans_primitives = list_of_trans,
		            ignore_variables = dict_to_ignore,
                                       max_depth = 2, 
                                       verbose = True, 
                                       n_jobs = -1)
```

There are a few important things to note from the boilerplate code above. First, the target_entity parameter specifies which entity to append the synthesized features. This entity, along with the synthesized features will be returned by the feature matrix. Second, the ignore_variables parameter allows you to exclude certain variables from deep feature synthesis. The entry for this parameter should be in the form of a dictionary, where the keys are the entity_id and the values are the column names from each entity you wish to exclude. Lastly, setting verbose equal to true returns a summary of the synthesis process with accompanying progress bar and time estimates.

A list of all of the synthesized features can be returned with the following code:

```
all_features = [str(x.get_name()) for x in feature_names]
```

### Conclusion

In summary, feature engineering is a crucial step in the data science process as it leads to better model performance. FeatureTools attempts to automate this time_consuming and energy_expensive step by using an algorithm to generate features in a fraction of the time. While this package is undoubtedly powerful, it is not a magic bullet and in no way is a substitute for domain knowledge and thoughtful feature engineering. FeatureTools will create mathematical combinations of features you would not have thought of yourself, but there is no way to know if these synthesized features will affect model performance. Additionally, the sheer number of synthesized features will rapidly increase your dimensionality and make it almost impossible to test all of the features within a model (especially for larger datasets). Since FeatureTools is fairly new to the data science field (open-sourced in 2017), it will be interesting to track the evolution of its utilization and updates.


