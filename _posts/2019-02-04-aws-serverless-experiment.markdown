---
layout: post
title:  "AWS Serverless Experiment"
date:   2018-10-25 16:21:56
categories: experiment aws serverless
---

# What?
`Serverless computing is a cloud-computing execution model in which the cloud provider runs the server, and dynamically manages the allocation of machine resources.`
https://en.wikipedia.org/wiki/Serverless_computing

I would like to try to build a toy project on a serverless platform to see how it all glues together.

I will experiment first on AWS, as it is the longest and from everythign I have read the most mature serverless platform.  It also has a nice free tier for me to play with.

# Why?
Serverless promises a lot.  Lets see what it feels like.

# How?

I would like to connect AWS Lambda, API Gateway, Amazon Aurora Serverless and Cognito together to create a example application.

Setting up a golang lambda function to respond via a api gateway request is very easy.  I managed to do everything though the web UI of aws.  

Here is the example hello world example taken from the AWS documentation.

{% highlight go %}
package main

import (
	"errors"
	"log"

	"github.com/aws/aws-lambda-go/events"
	"github.com/aws/aws-lambda-go/lambda"
)

var (
	// ErrNameNotProvided is thrown when a name is not provided
	ErrNameNotProvided = errors.New("no name was provided in the HTTP body")
)

// Handler is your Lambda function handler
// It uses Amazon API Gateway request/responses provided by the aws-lambda-go/events package,
// However you could use other event sources (S3, Kinesis etc), or JSON-decoded primitive types such as 'string'.
func Handler(request events.APIGatewayProxyRequest) (events.APIGatewayProxyResponse, error) {

	// stdout and stderr are sent to AWS CloudWatch Logs
	log.Printf("Processing Lambda request %s\n", request.RequestContext.RequestID)

	// If no name is provided in the HTTP request body, throw an error
	if len(request.Body) < 1 {
		return events.APIGatewayProxyResponse{}, ErrNameNotProvided
	}

	return events.APIGatewayProxyResponse{
		Body:       "Hello " + request.Body,
		StatusCode: 200,
	}, nil
}

func main() {
	lambda.Start(Handler)
}
{% endhighlight %}

Once I had done it through the UI, I wanted to then automate the process of updating the function via the the aws cli.

### Build the function
{% highlight bash %}
GOOS=linux go build -o bin/main github.com/keithballdotnet/aws-serverless/functions
{% endhighlight %}

### Zip the function
{% highlight bash %}
zip bin/deployment.zip bin/main
{% endhighlight %}

### Update the function
{% highlight bash %}
aws lambda update-function-code --function-name helloWorld --zip-file fileb://./bin/deployment.zip --region eu-central-1
{% endhighlight %}






