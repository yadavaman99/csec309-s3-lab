Title: Lab Assignment 01 - AWS S3 Secure Website Hosting
Student Details: Aman Yadav, 2301410010
1. S3 Bucket Website URL: http://aman-yadav-17-05-2005.s3-website.ap-south-1.amazonaws.com/
2. Implementation Steps: Create S3 Bucket
Sign in to the AWS Console > S3 > Create bucket
Used a unique name (aman-yadav-17-05-2005), choose region mumbai ap-south-1, keep defaults, and create the bucket.
Upload two HTML files:
index.html (homepage with your name/ID)
error.html (user-friendly 404 page).
Enable Static Hosting
In bucket Properties > Static website hosting, enable it.
Set, Index document = index.html
Error document = error.html
Save and note the endpoint
Configure Public Access
In Permissions > Block public access, disable "Block all public access."
Add a bucket policy (in bucket-policy.json) to allow public read.
Test the Website
Visiting the endpoint should now load index.html.
Accessing a non-existent path should show error.html.
4. Bucket Policy:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::aman-yadav-17-05-2005/*"
        }
    ]
}
5. IAM Policy:
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListBucket",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::aman-yadav-17-05-2005"
        },
        {
            "Sid": "AllowObjectActions",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::aman-yadav-17-05-2005/*"
        }
    ]
}
Create a custom IAM policy that allows only listing the bucket and putting, getting, or deleting objects—but strictly within your specific S3 bucket. This adheres to least-privilege principles. 
Create the IAM user (s3-website-manager) with console access in the IAM dashboard. Attach the custom policy directly, ensuring they only manage the website bucket.
Download the credentials CSV after creation and store it securely this avoids using root access.
6. CloudTrail Configuration & Logs:
Set up a CloudTrail trail named (S3-Website-Activity-Log) and direct logs into a new, separate S3 bucket—this avoids infinite logging loops. 
Enable both management and object-level (data) events, ensuring reads/writes (like GetObject, PutObject, DeleteObject) are logged. 
<img width="1362" height="555" alt="Screenshot 2025-08-17 202516" src="https://github.com/user-attachments/assets/cd961a05-ac10-4c12-b35f-96b3b3bd0ea1" />
<img width="1365" height="450" alt="Screenshot 2025-08-17 202531" src="https://github.com/user-attachments/assets/7d990282-9e67-42ab-85cb-64c82eb39af2" />
7. Security Validation:
<img width="1365" height="719" alt="Screenshot 2025-08-17 181614" src="https://github.com/user-attachments/assets/553a4b05-6d3a-4bfb-a82b-91ad941156bb" />
The S3 static website endpoint only supports HTTP, which is insecure. To enable HTTPS, we need to place Amazon CloudFront in front of the S3 bucket. CloudFront provides free SSL/TLS certificates (via ACM) and serves the content securely over HTTPS. Therefore, the correct AWS practice is to use CloudFront with the S3 bucket as its origin
8. Learning Outcomes: I learned how to apply the principle of least privilege by creating a dedicated IAM user with only S3-specific permissions, and how to use CloudTrail to audit and trace every action on my bucket for enhanced security and visibility.
