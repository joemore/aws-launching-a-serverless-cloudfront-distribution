# Launching an AWS/ServerLess CloudFront Distribution 

This is a simple example of how to launch a CloudFront Distribution with an S3 Origin bucket to host files on your website.

1. Install dependencies: `yarn install`

2. Rename the .env.example to .env and fill in the values

```bash
IAM_PROFILE=default
SERVICE_NAME=example-s3-cloudfront
MY_REGION=us-east-1
MY_BUCKET=example-private-bucket
MY_BUCKET_ORIGIN_ID=ExampleS3Origin
MY_DISTRO_COMMENT=Example Distro
# Set the price class to "Use Only North America and Europe" (PriceClass_200 is also europe etc, PriceClass_all is basically everywhere AWS supports)
MY_DISTRO_PRICE_CLASS=PriceClass_100 
# Allowed Origins  Can be either a single * or https://*.example.com or https://www.example.com - Read more here https://docs.aws.amazon.com/AmazonS3/latest/userguide/ManageCorsUsing.html - if you wish to use multiple then edit the serverless.yml file itself
ALLOWED_ORIGINS=*

# optional 1 - using a domain alias (also comment out lines 77-81 of serverless.yml if not using this)
MY_DOMAIN_ALIAS=subdomain.example.com
MY_ACM_CERTIFICATE_ARN=arn:aws:acm:us-east-1:1111111111:certificate/5555555-1111-2222-3333-b82099e490bd # Your SSL Cert found in https://us-east-1.console.aws.amazon.com/acm/home?region=us-east-1

# optional 2 - get route 53 to assign our domain alias automatically (also comment out lines 100-117 of serverless.yml if not using this)
MY_ROOT_DOMAIN=example.com
```

3. Deploy: `sls deploy` 

Hey presto - Your stack is deployed and you can now access your files you upload via the CloudFront URL! 

Next, try uploading some files to your S3 Bucket: 

```bash
aws s3 cp ./test-files/ s3://example-private-bucket/ --recursive --profile default --region us-east-1
```

Testing a fetch with javascript:

```javascript
fetch('https://subdomain.example.com/test.json')
	.then(response => response.text())
	.then(data => console.log(data));
```

If you are getting CORS errors then check your ALLOWED_ORIGINS is either set to *, the domain or a wildcard subdomain such as https://*.example.com you are accessing it from.

For a more detailed write up, take a look at my blog article [AWS - Launching a ServerLess Cloudfront Distribution](https://www.joemore.com/blogs/aws-launching-a-serverless-cloudfront-distribution)