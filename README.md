TimeFork For Stock Market Data
====

**TimeFork** is a technique for interactive prediction of time-series data. To showcase this technique, we developed a stock market analytics tool (StockFork) using data from the Yahoo Finance API. With TimeFork, you can predict the future of stocks through an interactive dialogue with the interface (through 3 steps).

1. The interface first provides prediction suggestions for each stock based on its past trends (**temporal predictions**).
2. Based on these suggestions and other stock market data, an analyst can interact to make his/her own prediction for one or more stocks.
3. Following the analyst's interactions for specific stocks, the interface recalculates the predictions for other stocks (**conditional predictions**). In essence, this step answers "what if" questions that an analyst can have. For example, what if Tesla increase by 5% over the next ten days. The dialogue goes back to Step 1 or 2 after. 

The analyst can go through these steps iteratively till he/she has enough information to make a decision regarding investment in the stocks.


## Repository Content 

In this repository, we provide two main things for developers and researchers interested in contributing to this project. 

1. The "main" branch contains our implementation of the stock market analytics tool, StockFork, including its server and client components (described below).

2. The "user-study" branch contains the implementation including the code and data specifically used for a user study conducted to understand if TimeFork is successful improving the predictions and when/how it does do. 

Beyond the implementations, **our repository also offers sample stock market data in [public/data/](https://github.com/karthikbadam/TimeFork/tree/master/public/data) folder and the corresponding trained neural network files (containing fitted weights) in [public/data/train/](https://github.com/karthikbadam/TimeFork/tree/master/public/data/train).**


## User-study Branch

For implementations, data, and training files specific to our user study, take a look into the [**"user-study" branch**](https://github.com/karthikbadam/TimeFork/tree/user-study).

## Build Process

To deploy this application, you must have [node.js](https://nodejs.org/en/) and [npm](https://www.npmjs.com/) installed.

1. Run `npm install` to install dependencies.
2. Run `node app.js` to start the application at port 3000. Now, you can try it out by opening `localhost:3000` in your browser.


## Server-Side Components

To support the TimeFork technique for stock market prediction, we used neural network models for temporal and conditional predictions. More specifically, 

1. The **temporal predictions** (predictions based on past performance of a stock) are generated using a **multilayer perceptron** (one model per stock) trained on the past 6 [relative](https://en.wikipedia.org/wiki/Relative_change_and_difference) stock price changes (input) to predict the future change. This model had five layers: input layer of 6 neurons, hidden layers of sizes (50, 60, 70), and an output layer of one neuron, with Sigmoid activation function. The models are trained on historical stock market data using the standard backpropagation algorithm. Alternative predictions for each temporal prediction are generated by changing one or more input values by a 10% fixed margin. This model is implemented using [Brain](https://www.npmjs.com/package/brain).

2. The **conditional predictions** (predictions for a stock based on a trend for other stocks) are generated using a **self-organizing map** trained on all co-occurrences in the dataset (e.g., Apple and Tesla increased by a 5\% and 3\% relative change respectively on a particular day). The neurons in the model capture these co-occurrence patterns and are clustered. So, we can easily look up the SOM neurons to find the ones matching a particular prediction from the analyst and generate conditional predictions for others (e.g., if the analyst says Apple might increase by 5\% in the next 3 days, what will happen to Tesla?). This model is implemented using [ML-SOM](https://www.npmjs.com/package/ml-som).

These models are trained on the sample stock market data provided. The server-side components train these models, which are further stored in [JSON format](http://json-schema.org/) on [public/data/train/](https://github.com/karthikbadam/TimeFork/tree/master/public/data/train).

## Client Interface

The client interface provides overview and detail representations for stock market data built using [D3](http://d3js.org/). It also accesses the trained neural networks (recreated from the JSON files) and provides prediction suggestions while supporting user interaction. 

![StockFork](https://raw.githubusercontent.com/karthikbadam/TimeFork/master/public/images/stockfork.png)


## How to use

[Link to the video](https://dl.dropboxusercontent.com/u/110166980/TimeFork-demo.mp4)

To learn more, [visit the wiki](https://github.com/karthikbadam/TimeFork/wiki).

