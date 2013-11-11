---
author: Anton Stroganov (@aeon)
---

Test Driven Neural Networks with Ruby - [Matthew Kirk](http://modulus7.com/), [@mjkirk](http://twitter.com/mjkirk)	

What are they used for?
- Spam filtering
- Music recommendations

The Challenge: use data to solve problems!
- Ruby has tools to use big data, but...
- The field is huge and confusing...
- Ruby can make it easier for us without having to learn the complex math behind it
- Neural networks to the rescue... the sledge hammer of functional relationships
	- Input layer
		- represent whatever we are inputting in to the model
	- Hidden layer
		- where the magic happens
		- How many neurons should we put in the hidden layer?
		- roughtly 2/3 * input layer count + output count
	- output layer
		- single output that you want as final result
- Neurons....?
	- A neuron just a function that takes two inputs, processes them by weighting them, and outputs a summarized result
		- y = f(w1 * x1 + w2 * x2)
		- a way to represent fuzzy logic
	- Activation functions: normalize input between 0 and 1
		- Sigmoid and Elliott - Learning curve
		- Gaussian - Bell curve
		- Linear - Line
		- Threshold - Yes/No
		- Cosine/Sine - Periodic
	- Since we are looking for a fuzzy logic answer, we want to normalize whatever is coming in to be between 0 and 1
- Training Algorithms
	- Quickprop
	- RProp => Use this
	- Back propagation
	- define how the inputs are weighed
	- They try to find a set of weights that minimize the error over the whole model.
- Example: Google translate autodetecting the language
	- problem: classify arbitrary text into a language
	- But how?
		- Data collection - get a bunch of text in different languages. Bible is one sample corpus we could use.
		- We can process the text in different ways...
			- character count
			- word count
			- stems
			- letter frequency count
		- Looks like character distribution is fairly distinctive between different languages, so it's a good candidate as input
	- TDD Neural net...
		- Write a test
		- Check if test fails
		- Pass/Fail
		- Write production code
		- Run all tests
		- Clean up code
	- Test the seams
		- can't really test the internals...
		- but we can test the expected inputs and results
			- it has proper keys for each vector
			- sums to 1 for all vectors
			- returns characters that is a unique set of characters used
	- Use Occam's razor... the simplest answer is the best... 
		- so if the model is taking a huge amount of time to train, either there is no pattern in the data, or it's not finding it with the current methods
	- Writing the tests helps in two ways
		- Helps you makes sure there is no problem in your input processing layer - in the demo, found out that international UTF-encoded text was not being split/processed as expected
		- Allows you to mess with the settings on the classifier, and compare the error rate changes between runs easily
	- Todo: calculating the confidence interval
		- compare known actual language to predicted language for the known corpus and recording it... 

- https://github.com/hexgnu/language-predictor
- http://modulus7.com/rubyconf

Cool stuff!