# Static Website Hosting using S3, and CloudFront

- In this project, I host a static Travel Blog Website, using AWS S3, and AWS CloudFront.

- The website is a simple static website, with HTML, CSS, and JavaScript files.

## The steps to host the website are as follows:

1. Configure S3 Bucket:

- Create an S3 bucket.
- Go to "Permission" tab, uncheck the box "Block all Public Access". Then, click on "Bucket Policy" and add the following policy:

```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::BUCKET_NAME/*"
        }
    ]
}
```

- Replace "BUCKET_NAME" with your bucket name.
- Go to "Properties" tab, click on "Static website hosting", and select "Use this bucket to host a website". Then, add "index.html" as the index document.
- Upload the website files to the bucket. Here, I am using AWS CLI commands to upload website files to S3 Bucket:
  ```bash
   aws s3api put-object --bucket my-bucket-website-travel --key index.html --body index.html --content-type text/html --profile cloud-fundamentals
   # Notice that: the --content-type text/html is a must. Otherwise, the index.html file will be downloaded instead of being displayed in the browser, because the default content-type is binary/octet-stream.
  ```

```bash
aws s3 cp img/ s3://my-bucket-website-travel/img/ --recursive --profile cloud-fundamentals
aws s3 cp css/ s3://my-bucket-website-travel/css/ --recursive --profile cloud-fundamentals
aws s3 cp vendor/ s3://my-bucket-website-travel/vendor/ --recursive --profile cloud-fundamentals
```

2. Configure CloudFront:

- Create a CloudFront distribution.

- In the "Origin Domain", select the <bucket-name>.s3-website-region.amazonaws.com (which is the S3 bucket website endpoint).

- Notice that: if we upload the updated files to the S3 bucket, the changes will not be reflected immediately in the CloudFront distribution (because the CloudFront's cache is not invalidated automatically). To fix this, we can create an invalidation in the CloudFront distribution (the Invalidations can not be deleted once created)

3. Access the website:

- Via S3 Static Website Hosting: http://my-bucket-website-travel.s3-website.us-east-2.amazonaws.com/
- Via CloudFront: https://du4k3zldq8ptp.cloudfront.net/
