# AWS Lambda [To be continued]

Use CLI, rather than installing it locally I will use docker image:
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-docker.html

````shell script
docker run --rm -it amazon/aws-cli help
````


And will follow this tuto:
https://docs.aws.amazon.com/lambda/latest/dg/services-apigateway-tutorial.html (not the template)

````shell script
➤ docker run -v "lambda:/lambda" --entrypoint pwd --rm -it amazon/aws-cli                             vagrant@archlinux
/aws

````

But error

````shell script
➤ docker run -v "lambda:/aws/lambda/function.zip" --rm -it amazon/aws-cli lambda create-function --function-name LambdaFunctionOverHttps \
  --zip-file "fileb:///aws/lambda/function.zip" --handler index.handler --runtime nodejs12.x \
  --role arn:aws:iam::123456789012:role/service-role/lambda-apigateway-role

Error parsing parameter '--zip-file': Unable to load paramfile fileb:///aws/lambda/function.zip: [Errno 21] Is a directory: '/aws/lambda/function.zip'
````

Idea: use Lambda based on docker:
- https://azure.microsoft.com/fr-fr/free/serverless/
- https://hackernoon.com/how-did-i-hack-aws-lambda-to-run-docker-containers-7184dc47c09b

We can run lambda inside docker: https://stackoverflow.com/questions/50670190/is-it-possible-to-run-docker-image-dockerfile-on-aws-lambda

For me: https://www.serverless.com/blog/why-we-switched-from-docker-to-serverless

