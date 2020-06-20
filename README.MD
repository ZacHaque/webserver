aws cloudformation create-stack --stack-name mywebserver --template-body file://webserver.yaml --parameters file://params-webserver.json --capabilities CAPABILITY_IAM


aws cloudformation create-stack --stack-name vpc --template-body file://demo-vpc.yaml


 --parameters file://params-webserver.json --capabilities CAPABILITY_IAM