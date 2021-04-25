## Grant Loan Analysis

In this week's challenge, the client would like to have a more concise calculation of grant applicants' approval status. That is, determining a trend between the successful grant applications for future applicants' better success. Let's take a look at what we have.

# The Data
First, let's take a look at the actionable data points we can crunch through. I've taken the liberty of removing the tax number and names of the charities for their own privacy.

![Original Variables](img_link)

It would appear we have a number of variables to hash out in respect to our target variable: Success. The first category that stands out to me is application type, as there are only 17 types out off the 30k+ examples here. Conversely, Classificiation may have some weight to the outcome of each grant, but it may be too varied to make proper judgments with our computing power. The asked amount for each grant is nearly independent of each, though we should preserve this data range as it could indicate a trend. I personally think the most important factors are as such: Application type, affiliation, use case, organization, and ask amount. While this may shy away from the income statistics of each, I am willing to bet that the substance of the grant's use is more important than the company that intends to use it. So the unneeeded data points are special considerations, and classification.

# Modeling
In order to hone in on the best traits of a grant proposal, I'm going to feed our data into a neural network. This network will analyze the traits of each proposal and, ideally, be able to predict the success of a grant based on these criteria.
After boiling down the variables, these are the columns that the neural network will be pitting against each other:

![Useful Indicators](img_link)

I will limit these factors later on, but first I'd like to crunch the numbers writ large.

![NN control](img_link)

As a sort of "control" on the neural networks that will be crunching the data, I've set up this sequential series above. 80 nodes in the first layer utilizing a tanh activation function. 30 nodes in the following layer, but utilizing relu activation to condense the data. Finally, sigmoid activation on our output layer, as we are only testing for a binary output: successful or not.

![Control Performance](img_link)

After running this test the network could only achieve about 74% accuracy. We're looking for some better performance so I'm going to jump up the scale of the network.

![Trial1 Structure](img_link)

In this iteration I added another layer after the 30node relu, condensing the learning data to 10 nodes through another relu activation. Also, even though this is another variable, I took out the ask amount column from our data in hopes to have less noise in the features. The results were as follows:

![Trial1 Performance](img_link)

Oddly enough, the performance was worse than our control. I fear the scale of the grant amount was actually a helpful learning tool for the network, so I'm going to add it back in. However, next test will take the Classification of each grant taken out. Perhaps this datapoint is adding unneeded noise to the overall model.

Because this is enough of a change to the test, I'm going to preserve network structure of the first trial.

![Trial2 Structure](img_link)

And with the classification flag taken out, here is the second trial's performance.

![Trial2 Performance](img_link)

It seems I am going in the wrong direction. Now we have only 73% accuracy from our network. In order to get more scientific about the trial and error, I'm going to see if Keras can figure out the best structure for me, but without taking data points out.

![KT Calculation](img_link)

Ok, while the accuracy score is not quite up to snuff, I'm going to chalk that up to not enough epochs cycled through to learn properly. So I'm going to scale up this "best performing" version and see what we get.

![Trial3 Structure](img_link)

And after 100 epochs we get this score:

![Trial3 Performance](img_link)

Not quite what we were looking for, but certainly on the right track. It seems having multiple layers helped the computing time, and brought our accuracy score up by more than half a percent. While that's not a particularly high number, I understand that the data we're working with here is not that straightforward, and has many outside factors to the success of each grant.

# Summary
To close, perhaps a much higher-scaled model is in order for the proper classification of these grants. I would recommend much denser hidden layers (perhaps back toward the 80-node threshhold of my first trial) but I would also recommend applying the same activation to all layers. 
Then again, this may be one of those datasets that has too much variation to yield a highly accurate prediction model. Nonprofit grants exist in an industry that is nearly entirely dictated by human decision-making, which skews the ability of a neural network to understand those decisions by a huge factor. 