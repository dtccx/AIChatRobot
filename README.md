# AIChatRobot
Current implementing, will not update codes until summer for personal private reasons.
Work in Cloud Computing Group.
NYU Tandon school Computer Science Program


# website enter:
http://bot-website-bucket.s3-website-us-east-1.amazonaws.com


## first one step project:
design simple dialog and front-end web:
with simple lambda function

![](img/image1.png)

### Below are several key points. 
1. Some implementations of the application features are slightly different from what TA recommended in the announcement. Firstly, I use implicit grant instead of authorization code grant. Also, to work around the fact that cognito only allows https callback, I actually created an api gateway http proxy to route the request back to the s3 bucket. 

2. Currently our application does not have sign out feature. Also, the user has to sign in first to use the chatbot.

3. The main client side logic is in index.html and js/index.js. Also, I added a lot of comments in js/index.js to help reader understand our code. Please pay extra attention to the comments since it is really helpful. 
### Function implemented
1.when you greet(morning or hi), it responses with "hi" back;

2.when you ask, it responses with simple answer;

3.when you say something it doesn't understand, it will responsed error;


## Second step:
design a Dining Concierge chatbot using Amazon Lex  
![](img/image2.png)
### design a Dining Concierge chatbot using Amazon Lex. 
1.	Build a Dining Concierge chatbot using Amazon Lex.  

	a.	Create a new bot using the Amazon Lex service. Read up the documentation on all things Lex, for more information: https://docs.aws.amazon.com/lex/latest/dg/getting-started.html  
	b.	Create a Lambda function (LF1) and use it as a code hook for Lex, which essentially entails the invocation of your Lambda before Lex responds to any of your requests -- this gives you the chance to manipulate and validate parameters as well as format the bot’s responses. More documentation on Lambda code hooks at the following link: https://docs.aws.amazon.com/lex/latest/dg/using-lambda.html  
	c.	Bot Requirements:  
		i.	Implement at least the following three intents:  
		> ●	GreetingIntent  
		> ●	ThankYouIntent  
		> ●	DiningSuggestionsIntent  
		ii.The implementation of an intent entails its setup in Amazon Lex as well as handling its response in the Lambda function code hook.  
		●	Example: for the GreetingIntent you need to   
			1. create the intent in Lex,  
			2. train and test the intent in the Lex console,   
			3. implement the handler for the GreetingIntent in the Lambda code hook, such that when you receive a request for the GreetingIntent you compose a response such as “Hi there, how can I help?”  
		iii.	For the DiningSuggestionsIntent, you need to collect at least the following pieces of information from the user, through conversation:  
		●	Location  
		●	Cuisine  
		●	Dining Time  
		●	Number of people  
		●	Phone number  
2.	Build a suggestions module, that is decoupled from the Lex chatbot.  
	a.	During the fulfillment step (https://docs.aws.amazon.com/lex/latest/dg/API_FulfillmentActivity.html) of the DiningSuggestionsIntent, push the information collected from the user (location, cuisine, etc.) to an SQS queue. More on SQS queues here: https://aws.amazon.com/sqs/  
	b.	Create a new Lambda function (LF2) that acts as a queue worker. Whenever it is invoked it   
		1. pulls a message from the SQS queue,  
		2. gets restaurant suggestions based on its parameters using one or more APIs such as Yelp or Google Places,   
		3. formats them and   
		4. sends them over text message to the phone number included in the SQS message, using SNS (https://docs.aws.amazon.com/sns/latest/dg/SMSMessages.html).  
	c.	Set up a CloudWatch event trigger that runs every minute and invokes the Lambda function as a result: https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/RunLambdaSchedule.html. This automates the queue worker Lambda to poll and process suggestion requests on its own.  
3.	Integrate the Lex chatbot into your chat API from Assignment 1.  
	a.	Use the AWS SDK to call your Lex chatbot from the Lambda function that you created in Assignment 1.  
	b.	When the API receives a request, you should   
		1. extract the text message from the API request,  
		2. send it to your Lex chatbot,   
		3. wait for the response,  
		4. send back the response from Lex as the API response.  
	c.	If you did everything correctly, you should be able to leverage the frontend from Assignment 1, with no additional modifications.  
	
## Tricky:

How to validate the slot value:

It's hard to get the slot name which users are currently texting.

for example:

"where do you live in" " New York"

How to get to know slot[city] is current one.
	




