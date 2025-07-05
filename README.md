# Beach-Wave-Conditions -S3 Static Website Hosting

This project documents the step-by-step migration of a static website displaying beach wave conditions (tide strength reports) from a local server to **Amazon S3** using static website hosting.
## Goal

Host a static website on Amazon S3 so users can access beach tide reports globally through a public URL.
## Project Structure
beach-wave-site/
├── index.html
├── styles.css
├── main.js
├── target-file.csv
└── text.html

**Step 1:** Plan the Migration:
Before starting, ask:
What files are being moved?
--> Static website: index.html, styles.css, main.js, etc.
--> Camera logs: images, videos, or metadata files
How will you organize them in the cloud?
--> S3 folder structure

**Step 2:** Create the S3 Bucket
--> create the bucket from the AWS Console or via CLI **aws s3 mb s3://website-bucket-c153f9bo**
This is where the name change happens — from your local directory (like /var/www/html/) to an S3 bucket :s3://website-bucket-c153f9bo/

**Step 3:** Move the Static Website
aws s3 cp /var/www/html/ s3://website-bucket-c153f9bo/ --recursive
**/var/www/html/ → local path
s3://website-bucket-c153f9bo/ → destination folder in S3
--recursive → copies all files and subfolders**
Now your static website files are in:
s3://website-bucket-c153f9bo/index.html
![migrated waves website on amazon s3](https://github.com/user-attachments/assets/6c2a5290-a93c-4ff7-aae7-68a7e59fe648)

**Step 4:** Enable Static Website Hosting
**S3 buckets are not websites by default — they're just storage.
Enabling static website hosting does 3 things:**
**"Function -->	Reason"
Allows S3 to act like a web server -->	So you can serve .html, .css, .js like a real site.
Lets you define index.html -->	So root URL knows what to show.
Lets you define error.html -->	So 404 pages are user-friendly.**

In AWS Console → Go to the bucket
Click on "Properties"
Scroll to Static website hosting
Enable it and set:
Index document: index.html
Error document: error.html
You’ll get a public URL like:
**http://website-bucket-c153f9bo.s3-website-us-east-1.amazonaws.com**
![ENABLED STATIC webhosting in s3 buckets](https://github.com/user-attachments/assets/35d367fa-fd6c-45ed-90c8-ee0c9014ee6d)

**Step 5:** Configure Permissions
Add a bucket policy like this:

**{
  "Version": "2012-10-17",
  "Id":"StaticWebPolicy"
  "Statement": [
  {
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::website-bucket-c153f9bo/*"
  }
  ]
}**
![permissions](https://github.com/user-attachments/assets/a8048a2f-41b6-4452-b4b1-35f7f1f82351)


Click the url:http://website-bucket-c153f9bo.s3-website-us-east-1.amazonaws.com
Now static website is available on cloud:

![waves webpae](https://github.com/user-attachments/assets/f2d81772-f036-48c6-8e58-5624a2f103ac)
