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
  
* in the class exercise:
  * ```The provided Dataset instead generates [object Object]```: is the error if you dont have the map function to it
    ```tfjs@latest:2 Uncaught (in promise) Error: A Dataset iterator for fitDataset() is expected to generate objects of the form `{xs: xVal, ys: yVal}`, where the two values may be `tf.Tensor`, an array of Tensors, or a map of string to Tensor.  The provided Dataset instead generates [object Object]```
    
  * However, if a simple map is provided, it is evaluates to null
   ```const convertedTrainingData = trainingData.map(({xs, ys}) => {
                return { xs : Object.values(xs), ys : Object.values(ys)};
            }).batch(10);```
            
   Leads to :  ```Uncaught (in promise) TypeError: Cannot convert undefined or null to object``` error
      * Root cause: the error indicated null object which hinted the trainingData to be null. Hence the last operation to load the variable was revisited and found there was a spelling mistake in ```columnConfigs```.

# Week 2
* in this week the course focusses on tf.viz and tf.tidy from the tensorflow perspective.
* there is a lab where front end captures the user hand writing and classifies it as respective number.
* there is also an awesome dynamic plot indicating the status of the training.
* to decode the code:
  * there is a load of new concepts from the front end perspective.
    * like angular or may be it is best practice at js to have the class and its method seperate file and later it is imported in the area where it is used.
    * lot of ```var``` are defined first ; kind of global variables declaration. As the data type is var it can be anything in the future.
    * there are also mix of async and regular functions.
      * async functions are called with ```await``` before to it.
         * async function returns **Promise** objects which can have fulfilled, rejected or pending: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261
      * one without async would be called as a regular function call.
* tf-viz : https://github.com/tensorflow/tfjs/tree/master/tfjs-vis
   * it has some components built focussed more on plotting the training outcomes and model (keras) layers.
   * it has maximize and hide button
   * it has capability to have a running plot display for tracking training progress 
      
      
* tf.tidy(): https://js.tensorflow.org/api/0.11.7/#tidy
  * it takes function as argument and deletes all the intermediates variables post execution of the function except return variable.
  * a general practice to avoid memory leak.  
* draw function
  * rawImage.src = canvas.toDataURL('image/png');  // from canvs html tag
  * <img id="canvasimg" style="position:absolute;top:10%;left:52%;width=280;height=280;display:none;">
     * both statements results in saving of the drawn image at html
  * rawImage = document.getElementById('canvasimg');
     * saves the html <img> to a variable.
* 
```var raw = tf.browser.fromPixels(rawImage,1);
    var resized = tf.image.resizeBilinear(raw, [28,28]);
    var tensor = resized.expandDims(0);
    
    var prediction = model.predict(tensor);
    var pIndex = tf.argMax(prediction, 1).dataSync();
```
    
    
    * first raw image is converted to tensor
    * resized to 28x28 ; canvas image will be of 280x280 dimension image and tf model takes 28x28
    * conv2d layer requires 3d input and hence **expandDims** does the magic for us.
    * followed by prediction and argmax for label assignment.
# Week 3
* in this week, the course focusses on using pretrained models in js
* it also talks about converting a model built on python to json format and load the same in js
* it is because the model building is usually done in data center (where the data resides), this week is the focus on pre built model

* 2 pretrained models:
  * toxicity model : identifies the toxicity in a sentence. provides the probability for ( insult, identity attack, obsene etc)
    * ```<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest"></script>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/toxicity"></script>```
        * first script includes the tfjs capability to browser
        * second script includes the toxicity model into the browser scope. However, it has to be loaded later.
        * https://github.com/lmoroney/dlaicourse/blob/master/TensorFlow%20Deployment/Course%201%20-%20TensorFlow-JS/Week%203/Examples/toxicity.html
             * toxicity.load(threshold) : loads the model in the browser scope with predefined threshold. Now this threshold is toxicity model specific and need not have to be there for other models.
             * then ( body) emphasises on the completion of model load .
             * model is the result of load and it is used to *classify* sentence
             * *prediction* is a json format output which is later parsed for displaying the output 
  
  * mobilenet: world famous shrinked image classifer for 1000 labels.
     * again simple CDN includes the model to browser : <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet@1.0.0"> </script> 
     * a static image is loaded too the browser and that is sent to the loaded pretrained model to classify
     
* Saving the model to json:
  * python program would have natural flow ( data ingestion, data wrangling , model building along with hyper parameter tuning)
  * post that, using keras, model.save() is used to save in the local directory
  * ```tensorflowjs_converter --input_format=keras {saved_model_path} ./``` is used to convert the model to json format in the current directory
  * this will create a folder with multiple files in it. This has to be moved to the directly where the html file loads it.
  * you have option of optimization as indicated : https://github.com/tensorflow/tfjs/tree/master/tfjs-converter#step-1-converting-a-tensorflow-savedmodel-tensorflow-hub-module-keras-hdf5-or-tfkeras-savedmodel-to-a-web-friendly-format
  
* loading the json in browser:
  ``` const MODEL_URL = 'https://.../mobilenet/model.json';
      const model = await tf.loadGraphModel(MODEL_URL);
  ```
# Week 4
* last part this week is amazing, there is web cam which collects the data at real time and uses the same for modelling.
* rock paper sizler is given in as an example and in the exercise we are asked to include 2 more sets lizard and spock.
* unfortunately, my pc is letting me install install tensorflow js for some security concerns. However, it is not required here in this webcam based training.
* when I trained with my hand for rock, paper and scissors, it detected succussfully for my wifes hand too. So it is clear that hair, finger size and lines in my hand were not considered for feature to detect the signs !!!!
