

aws cloudformation create-stack --stack-name demo-vpc --template-body file://demo-vpc.yaml

aws cloudformation create-stack --stack-name demo-webserver --template-body file://demo-webserver.yaml --parameters file://params-webserver.json --capabilities CAPABILITY_IAM
