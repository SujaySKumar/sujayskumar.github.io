---
layout: post
title: Interpreting Machine Learning Models - Part 4
date: '2019-09-15T15:35:06.387009'
author: Sujay S Kumar
tags: 
---

Link to start of the blog series: [Interpretable ML]({% post_url 2019-07-10-interpreting-machine-learning %})

## Type 2 : Model Agnostic Interpretation (Continued)

3. **Feature Importance**
    
    We measure the relative importance of a feature by permuting it's values and observing it's effect on the prediction. If the feature is "important" to the prediction, then the prediction changes drastically when the feature value is changed. Conversely, if the feature is relatively "unimportant", then permuting the value of the feature will have negligible effect on the predicted value. 
    
    *NOTE:* We still assume that the features are not correlated.
    
4. **Global Surrogate**

    Here, we solve a machine learning problem with more machine learning! If the black-box model is too complex to be interpreted, then we train a simple, interpretable model to mimick the bigger, complex model.
    
    This is an area of active research in machine learning, engendered not just by a need for interpretability, but also by a need to reduce model sizes. As the models get more and more complex, they grow in size too and contain millions of parameters. This makes it harder to deploy these models on memory-constrained devices such as phones and IoT devices. Therefore, we develop small ML models which can probe the complex model infinitely. Therefore, the smaller model trains to mimick the bigger model by observing how the prediction changes when the input is changed. These research endeavours have been surprisingly successful.
    The same approach is used in this case, where we train a smaller, interpretable model to mimick the bigger model and hence, we can interpret the outputs.
    
    This smaller model is called as a "surrogate" of the bigger model and more accurately, it is called as "global surrogate" as it mimicks the entire feature space of the bigger model. This is in contrast to "local surrogates" which is explored in the next section, where the surrogate is trained only on a local sub space of the bigger model and is used to interpret a single prediction.
    
5. **Local Surrogate (LIME)**

    Local interpretable model-agnostic explanations (LIME) focuses on training local surrogate models to interpret individual predictions instead of the entire model. This follows a similar principle of *Feature Importance*, where we generate a new dataset by perturbing the given input. The exact steps are outlined below:
    
    - Select the instance for which you want to have an explanation of its black box prediction.
    - Perturb your dataset and get the black box predictions for these new points. Similar to permuting only a single feature in *Feature Importance*, here we perturb the given vector by changing all the features.
    - Weight the new samples according to their proximity to the instance of interest. This is to give higher importance to generated instances which are closer to the instance of interest. This can be done by any similarity or distance metric. LIME uses an exponential smoothing kernel. A smoothing kernel is a function that takes two data instances and returns a proximity measure. The kernel width determines how large the neighborhood is: A small kernel width means that an instance must be very close to influence the local model, a larger kernel width means that instances that are farther away also influence the model. 
    - Train a weighted, interpretable model on the dataset with the variations.
    - Explain the prediction by interpreting the local model.
    
    How do you get the variations of the data? This depends on the type of data, which can be either text, image or tabular data. For text and images, the solution is to turn single words or super-pixels on or off. In the case of tabular data, LIME creates new samples by perturbing each feature individually, drawing from a normal distribution with mean and standard deviation taken from the feature.

    - LIME for Tabular Data:
        - Tabular data is when the training data is in the form a table where each row is a training instance and each column is a feature.
        - The problem here, is how do we generate data close to the instance that we are interested in? Even though LIME uses exponential smoothing function with a kernel width of 0.75 times the square root of the number of columns of the training data, there is no explanation why.
        
    - LIME for Text:
        - Variations of the data are generated differently: Starting from the original text, new texts are created by randomly removing words from the original text. The dataset is represented with binary features for each word. A feature is 1 if the corresponding word is included and 0 if it has been removed.
        
    - LIME for Images:
        - LIME for images works differently than LIME for tabular data and text. Intuitively, it would not make much sense to perturb individual pixels, since many more than one pixel contribute to one class. Randomly changing individual pixels would probably not change the predictions by much. Therefore, variations of the images are created by segmenting the image into “superpixels” and turning superpixels off or on. Superpixels are interconnected pixels with similar colors and can be turned off by replacing each pixel with a user-defined color such as gray. The user can also specify a probability for turning off a superpixel in each permutation.
