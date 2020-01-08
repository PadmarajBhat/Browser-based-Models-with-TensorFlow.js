# Week 1:
* Tensorflow js are written between script tags
* WebGl is used to train on GPU when it comes to building model on browser.
* we are going to use brackets and webserver for chrome for our exercises.
* in the FirstHTML.html
  * a script is written in the html file iteself
    * ```<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest">``` : will interpret the tf js code
  * a asynchronous function is written which takes model as input and call the model.fit to said number of epochs
  * a model is built with one neuron to simulate the linear relation ship between test ```xs``` and ```ys``` values.
  * the asynchronous function is called and post completion predict function is called to get the ml model prediction.
  * now the entire operation happens in background and since alert is given to output the predict output that is only indication that training has completed.
  
* in the iris_classifier.html
 * fitDataset is used to feed the csv dataset into the model.
 * one hot encoding is kind of not scalable code :(
 * how did the copy paste code return different predicted value as that of hand written code ? 
  * Ans: Code compare utility helped to point out the issue. It was spelling mistake in the label encoding. It should have been "setosa" , I had "sentosa".
