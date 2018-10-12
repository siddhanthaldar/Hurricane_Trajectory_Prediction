# Hurricane Trajectory Prediction
We propose to develop a web application that takes as input the location from the user
and acquires latitude, longitude, wind speed and pressure corresponding to that location
and predicts if a cyclone might strike that area in the near future.

## Brief Overview
Previous work on hurricane trajectory estimation demonstrates widespread use of Recurrent 
Neural Networks (RNNs) and Long-Short Term Memory(LSTM) models. LSTM provides better memory
retention than RNNs, and are usually preferred over RNNs. However, recent research shows an inclination towards the use of Convolutional Neural Networks(CNNs) for time series modelling. We propose a CNN based time series modelling approach to predict the trajectory of hurricanes. Such work would be beneficial in predicting possible areas which could be struck by hurricanes.

We will be using the Atlantic Hurricane database and the Northeast and North Central Pacific hurricane database provided by the National Hurricane Center (NHC). The mentioned databases contain data obtained at 6 hour intervals. We will be using the following attributes - latitude, longitude, wind speed, pressure. Our methodology is explained in the following sections.

## Methodology
### Grid Modelling

![hurricane](https://user-images.githubusercontent.com/25313941/46867948-555c2400-ce44-11e8-9bd1-bb67e27d03b7.png)

<sub>*Figure : Atlantic Hurricane Points Collected by Unisys Weather*<sub>

We plan to devise a grid model with each data point being assigned to a particular grid block and we will train the neural network to predict the grid block of the hurricane at the next time step given the location at the present time step. The number of grid blocks used in our model could be tuned depending on the amount of hurricane data available. Having a large number of data points would enable us to use a large number of smaller grid blocks, thus permitting more accurate prediction with less error margin.

### Proposed Model

![hurricane_trajectory_pipeline](https://user-images.githubusercontent.com/25313941/46867775-c8b16600-ce43-11e8-8721-a8e5ca92d987.jpg)

We plan to use a Convolutional Neural Network(CNN) based time series prediction model. We arrange the data in a manner suitable for the application of CNN. Say we have a D-dimensional data and want to consider the data corresponding to k time steps at one go. We will use a 2D-array of dimensions kxD, with each row containing a D-dimensional vector corresponding to a particular time step. One thing that must be taken care of is that the data corresponding to successive time steps must be arranged sequentially in the 2D-array. 

After arranging the data in the proposed format, we obtain a feature map of size kxD containing attributes corresponding to k successive time steps. We will apply convolution operations over this feature map followed by a fully connected layer. After the first fully connected layer, the network separates in two branches with both branches utilizing the data from all the neurons of the fully connected layer to predict the x and y coordinates of the grid respectively. Since we plan to predict the positions at multiple time steps at a go, the number of neurons in the last layer will depend on the number of future time steps that we want to predict. We can take a weighted sum of the losses corresponding to each branch for better results. A parameter that can be considered for obtaining the weights for the two losses can we the wind speeds along x and y direction at the previous time step.

#### Advantages of the proposed model
- Since we are predicting multiple time steps at one go, errors per time step do not accumulate. Hence, the final loss is expected to be lower than that for RNNs as in RNN, the error accumulates at every time step.
- The CNN based model is faster as it produces multiple time step predictions at one go.

### Final Step
To convert the grid coordinates back to the latitudes and longitudes, we can follow either of the approaches:
- We can train a simple artificial neural network for conversion of grid coordinates to latitude and longitude.
- We can use a Bayesian neural network for the same conversion.

This would substantially reduce the error involved in conversion of grid coordinates to latitude and longitude.

## Conclusion
Since we plan to use only 4 features for training and testing purposes, the application will be computationally efficient and can be deployed easily as a web app using web services such as Flask, Socket etc. The proposed application will extract the features using satellite data and the user given location and predict if a cyclone might strike a particular area in the near future.



