aws-lambda-gulp

## ðæ¦è¦

AWS Lambda ãããDynamoDB ã«æ¸ãè¾¼ã¿ããããµã³ãã«ã§ãã
Gulpã§ZIPãã¦ããã­ã¤ãã¦ã¾ãã


## ð¬ä½¿ãæ¹

### Labmda ãZIPã«çºãã
```
gulp dist
```

### aws cli ãèµ·å
```
docker-compose up -d
docker-compose exec awscli /bin/bash
```

### Labmda ãAWS ã«ããã­ã¤
```
POLICY=$(cat<< EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
)
# ã­ã¼ã«ãä½æãã
aws iam create-role \
  --role-name lambda-dynamodb-role \
  --assume-role-policy-document "${POLICY}"

â» DynamoDBã¸ã®ã¢ã¯ã»ã¹æ¨©éã¯CLIã ã¨ã¨ã©ã¼ã«ãªãã½ãã®ã§å¾ã»ã©ã³ã³ã½ã¼ã«ç»é¢ããä»ä¸ãã

# å®è¡çµæã CloudWatch Log ã¨ãã¦ä¿å­ããã
aws iam attach-role-policy \
  --role-name lambda-dynamodb-role \
  --policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess

# Lambdaé¢æ°ãä½æãã
aws lambda create-function \
  --function-name lambda-dynamodb-read-function \
  --zip-file fileb://dist/read-fde29740-3b0a-11ec-856f-514ae6ed300d.zip \
  --handler index.handler \
  --runtime nodejs14.x \
  --role "arn:aws:iam::004796740041:role/lambda-dynamodb-role"
aws lambda create-function \
  --function-name lambda-dynamodb-write-function \
  --zip-file fileb://dist/write-fde29740-3b0a-11ec-856f-514ae6ed300d.zip \
  --handler index.handler \
  --runtime nodejs14.x \
  --role "arn:aws:iam::004796740041:role/lambda-dynamodb-role"

# Lambdaé¢æ°ãå®è¡ãã¦ã¿ã
aws lambda invoke \
  --function-name lambda-dynamodb-write-function \
  --log-type Tail \
  --payload '{"email":"taro@test.com", "first_name":"å¤ªé", "last_name":"ãã¹ã", "age":40}' \
  outputfile.txt

  
aws lambda invoke \
  --function-name lambda-dynamodb-read-function \
  --log-type Tail \
  --payload '{"email":"taro@test.com"}' \
  outputfile.txt
```


## ð« Licence

[MIT](https://github.com/isystk/aws-lambda-gulp/blob/master/LICENSE)

## ð Author

[isystk](https://github.com/isystk)
