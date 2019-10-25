---
layout: post
title:      "Neural Networks – Optimizers"
date:       2019-10-25 14:54:26 -0400
permalink:  neural_networks_optimizers
---

<p align="center">
<img src="https://www.bsi.bund.de/SharedDocs/Bilder/DE/BSI/Themen/Cyber-Sicherheit/SiSyPHuS_Win10.jpg?__blob=normal&v=1">
</p>

The main goal of any machine learning model is to learn the classification/regression task from the data it was given. For models such as decision trees or random forests, “learning” happens in the form of finding the best way to successively split the data such that information gain is maximized (entropy is minimized) at each split. Neural networks, on the other hand, use an algorithm known as gradient descent to minimize the difference (loss) between the predicted and ground-truth values. As performing gradient descent involves calculating the partial derivative of the cost function with respect to each parameter in that function, it is a very computationally expensive process. This computational expense is also compounded by the massive amount of data neural networks need to achieve peak performance. Thus, many algorithms have been developed to improve upon the standard (stochastic) gradient descent such as Momentum, Root Mean Square Propagation (RMSProp), and Adaptive Moment Estimation (Adam). 

In order to understand what optimizers might work best for a particular deep learning task, it is essential first to understand where optimizers fit into the neural network and how they participate in learning. Optimizers affect cost calculations and thereby affect how the weights and bias values are updated during backpropagation. Let’s briefly discuss back propagation before we dive into the specific optimizers.

## Back Propagation

At this point the data has been fed forward through the network and it has generated a prediction. The loss (_L(pred, true)_) is calculated for each prediction based on how far off it is from its ground-truth value and then all of the loss measurements are averaged together to form a cost function (_J(w,b)_) for the entire network. The derivative of the cost function is calculated by taking the partial derivative with respect to each of its variables (_w_ and _b_). The value of each variable’s derivative is used to update the weight and bias values throughout the network by multiplying the derivative by a learning rate and then subtracting this value from the current weight/bias value (i.e. _updated w_ = _current w_ - (_alpha_ x  _dw_)). 

This is where the magic of gradient descent and its optimizers come into play. If the value of (_alpha_ x  _dw_) is negative, then the value of _w_ will be updated in the positive direction. Whereas, if the value of (_alpha_ x  _dw_) is positive, the new value of _w_ will be updated in the negative direction. The values of the weights/bias are updated in a direction opposite that of their derivative is because the cost function is convex (think U-shaped parabola). Since the derivative is the slope of the tangent line to the function and because we want to find the values for _w_ (and _b_, but for the sake of explanation we will just focus on _w_ for this example) that minimizes this cost function, we update their values based on what would bring them closer to the cost function minimum. Thus, if the derivative is positive, the _w_ value is moving the function away from the minimum and it needs to be shifted in the negative direction, and vice versa.

What was just described above is stochastic gradient descent and it is effective at finding the minimum of the cost function given sufficient time. However, it is slow and can affect the efficiency of model production and deployment. The optimizers mentioned at the beginning of this discussion help speed convergence (finding the minimum of the cost function) along to address the efficiency issue of stochastic gradient descent.

## Optimizers

### Momentum

Adding [momentum to stochastic gradient descent](https://towardsdatascience.com/stochastic-gradient-descent-with-momentum-a84097641a5d) helps the algorithm converge faster because it utilizes a moving average of the previous gradient calculations to update the _w_ and _b_ values. With this method, the path towards convergence is more direct because the vertical oscillations have been reduced by the exponential weighted averages of the gradients.

The equation for applying momentum to stochastic gradient descent is shown below. It looks very similar to the equation for stochastic gradient descent, except that the exponentially weighted average of the gradient (_V_) is multiplied by the learning rate (_alpha_) instead of the derivative of _w_ (or _b_, but again for the sake of explanation we will just focus on _w_). The _beta_ parameter can be thought of as how many previous gradients are being included in the moving average. A larger _beta_ indicates a greater number of previous gradients are included in the moving average, where a smaller _beta_ indicates fewer gradients are included. The best way to determine _beta_ for a project is through iterative training, but a value of 0.9 is a good place to start.

<p align="center">
<img src="https://raw.githubusercontent.com/kpokrass/blog_images/master/momentum_eq.png" width="300">
</p>

### RMSProp

The RMSProp optimizer is very similar to adding momentum to stochastic gradient descent in that it reduces the vertical oscillations on the path to convergence. However, it does this by using the square of the current gradient instead of just the gradient. This is advantageous when using mini-batches (subgroups of the full training dataset) to train the network as is forces the gradient used in updates to be similar for adjacent mini-batches. 

The equations for implementing RMSProp are shown below. Again, _beta_ can be thought of as how many previous gradients to include in the same manner as with momentum (0.9 is still a good starting value). _Epsilon_ is a correction parameter to ensure that we do not divide by zero when updating the weight and bias values. This parameter is usually set to 1e-9.

<p align="center">
<img src="https://raw.githubusercontent.com/kpokrass/blog_images/master/momentum_eq.png" width="300">
</p>

### Adam

The Adam optimizer is essentially a combination of momentum and RMSProp. Thus, it has the efficiency advantages of momentum and the mini-batch performance increases seen with RMSProp. Because momentum and RMSProp reduce the vertical oscillations during gradient descent, the learning rate (_alpha_) may be able to be increased to speed up convergence. This combination of increased efficiency and performance is one of the main reasons the Adam optimizer is one of the most popular optimization algorithms used in the field of deep learning.

The equations to implement Adam are shown below. The _beta_ values for the momentum-like portion and the RMSProp-like portion are different and the developers of the Adam optimizer suggest 0.9 and 0.999, respectively, as starting values. The _epsilon_ value in the denominator of the RMSProp-like portion of the equation serves the same purpose as it does in the straight forward application of RMSProp and therefore should still be set to 1e-9. It is also important to note that when the Adam optimizer is used, the exponentially weighted average terms (_V_ and _S_) undergo a bias correction calculation to minimize the effect of the randomly initialized values of _V_ and _S_. These values are divided by 1 - _beta_^t (t being equal to the time step; number of gradients in the exponentially weighted moving average).

<p align="center">
<img src="https://raw.githubusercontent.com/kpokrass/blog_images/master/adam_eq.png" width="450">
</p>

## Summary

This post focused on how neural networks learn during back propagation and the optimization algorithms to make this learning more efficient. While the optimizers included in this post do not constitute an exhaustive list, the ones highlighted offer a discussion of the a few of the most popular algorithms used in the field of deep learning. Adding momentum to stochastic gradient descent accelerates convergence by reducing the magnitude of the oscillations in the vertical direction. RMSProp also reduces the magnitude of the vertical oscillations, but it does so by normalizing the magnitude of the step size, which makes this optimizer better when used with mini-batch training. Lastly, the Adam optimizer combines the benefits of momentum and RMSProp, making it a go-to choice for most neural network developers. As always, there is no way to truly know which optimizer will work best for a specific project besides trial and error, but the optimizers discussed in this post should provide an informed starting point for any deep learning project. 

### Additional Resources

[Cost Functions and Gradient Descent](https://towardsdatascience.com/machine-learning-fundamentals-via-linear-regression-41a5d11f5220)

[Momentum](https://engmrk.com/gradient-descent-with-momentum/)

[RMSProp](https://towardsdatascience.com/understanding-rmsprop-faster-neural-network-learning-62e116fcf29a)

[Video Lecture for RMSProp](https://www.coursera.org/lecture/deep-neural-network/rmsprop-BhJlm)

[Video Lecture for Adam Optimizer](https://www.coursera.org/lecture/deep-neural-network/adam-optimization-algorithm-w9VCZ)

[Why Correct for Bias in Adam Optimizer](https://stats.stackexchange.com/questions/232741/why-is-it-important-to-include-a-bias-correction-term-for-the-adam-optimizer-for)

