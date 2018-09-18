---
published: true
title: >-
  How to Visualize a Decision Tree from a Random Forest in Python Using
  Scikit-Learn
categories:
  - useful tips
  - visualization
---
![](https://cdn-images-1.medium.com/max/2000/1*lAhJt7bvEDxT4DEdd29yCA.jpeg)

## A helpful utility for understanding your model

[Here’s the complete code](https://gist.github.com/WillKoehrsen/ff77f5f308362819805a3defd9495ffd): just copy and paste into a Jupyter Notebook or Python script, replace with your data and run:

<script src="https://gist.github.com/WillKoehrsen/ff77f5f308362819805a3defd9495ffd.js" charset="utf-8"></script>
<center>Code to visualize a decision tree and save as png ([on GitHub here](https://gist.github.com/WillKoehrsen/ff77f5f308362819805a3defd9495ffd)).</center>

The final result is a complete decision tree as an image.

![](https://cdn-images-1.medium.com/max/2000/1*IPLwmH-TJRhEWXW7uaetMw.png)
*Decision Tree for Iris Dataset*

<!--more-->

### Explanation of code

1.  **Create a model train and extract:** we could use a single decision tree, but since I often employ the [random forest](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) for modeling it’s used in this example. (The trees will be slightly different from one another!).

        from sklearn.ensemble import RandomForestClassifier
        model = RandomForestClassifier(n_estimators=10)
        # Train
        model.fit(iris.data, iris.target)
        # Extract single tree
        estimator = model.estimators_[5]

2. **Export Tree as .dot File:** This makes use of the `export_graphviz` function in Scikit-Learn. There are many parameters here that control the look and information displayed. Take a look at [the documentation](http://scikit-learn.org/stable/modules/generated/sklearn.tree.export_graphviz.html) for specifics.

        from sklearn.tree import export_graphviz
        # Export as dot file
        export_graphviz(estimator_limited, 
                        out_file='tree.dot', 
                        feature_names = iris.feature_names,
                        class_names = iris.target_names,
                        rounded = True, proportion = False, 
                        precision = 2, filled = True)

3. **Convert** `**dot**` **to** `**png**` **using a system command**: [running system commands](https://stackoverflow.com/questions/89228/calling-an-external-command-in-python) in Python can be handy for carrying out simple tasks. This requires installation of [graphviz which includes the dot utility](https://graphviz.gitlab.io/download/). For the complete options for conversion, take a look at [the documentation](#%20Convert%20to%20png%20from%20subprocess%20import%20call%20call%28[%27dot%27,%20%27-Tpng%27,%20%27tree.dot%27,%20%27-o%27,%20%27tree.png%27,%20%27-Gdpi=600%27]%29).

        # Convert to png
        from subprocess import call
        call(['dot', '-Tpng', 'tree.dot', '-o', 'tree.png', '-Gdpi=400'])

4. **Visualize**: the best visualizations appear in the Jupyter Notebook. (Equivalently you can use `matplotlib` to show images).

        # Display in jupyter notebook
        from IPython.display import Image
        Image(filename = 'tree.png')

#### Considerations

With a random forest, every tree will be built differently. I use these images to display the reasoning behind a decision tree (and subsequently a random forest) rather than for specific details.

It’s helpful to limit maximum depth in your trees when you have a lot of features. Otherwise, you end up with massive trees, which look impressive, but cannot be interpreted at all! Here’s a full example with 50 features.

![](https://cdn-images-1.medium.com/max/2000/1*hW67kyPZZJ6I_7Z8huwDkg.png)
*Full decision tree from a real problem ([see here](https://www.kaggle.com/willkoehrsen/a-complete-introduction-and-walkthrough)).*

### Conclusions

Machine learning still suffers from a [black box problem](https://www.technologyreview.com/s/604087/the-dark-secret-at-the-heart-of-ai/), and one image is not going to solve the issue! Nonetheless, looking at an individual decision tree shows us this model (and a random forest) is not an unexplainable method, but a sequence of logical questions and answers — much as we would form when making predictions. Feel free to use and adapt this code for your data.

*****

As always, I welcome feedback, constructive criticism, and hearing about your data science projects. I can be reached on Twitter [@koehrsen_will](http://twitter.com/@koehrsen_will)
