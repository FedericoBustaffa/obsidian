---
id: double_descent
aliases: []
tags:
  - master
  - machine_learning
---

# Double Descent

A relatively new phenomenon discovered is called **double descent** and it
occurs when the model is **overparametrized**, in other words when the model has
more weights/units than the number of training examples.

This phenomenon seems to go in contrast with what the basics of ML teaches:
usually a very complex and flexible model (w.r.t to the task) is bound to
overfitting.

![Double Descent|500](/files/double_descent.png)

As is possible to see in the plot, until the model doesn't reach the **interpolation
threshold**, everything is as expected: training error keeps decreasing while
test error decreases until it reaches a minimum and then starts growing again.

The test error grows until the interpolation threshold that, more precisely, is
the point where the model is complex enough to perfectly fit the training data.
For example a polynomial regressor trained with 3 points can for sure fit
perfectly those points with a 2-degree polynomial (3 free parameters).

![Degree 2](/files/regression_example_deg2.png)

![Degree 7](/files/regression_example_deg7.png)

If the number of free parameters increases, the model maintains the perfect
interpolation of the training set, but now the solution given by gradient
descent seems to be _"regularized"_ in some sense.

![Degree 16](/files/regression_example_deg16.png)

In other words the model continues to fit training data but with an higher
degree polynomial with smaller weights.

## References

- [[deep_learning]]
- [[bias_variance]]
- [WelchLabs YouTube video](https://www.youtube.com/watch?v=z64a7USuGX0&t=1808s)
