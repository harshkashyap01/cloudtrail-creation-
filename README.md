# cloudtrail-creation

CloudTrail creation project using Terraform in AWS.

---

## 📌 Architecture

```
AWS Account
   │
   ├── CloudTrail (Multi-region)
   │       │
   │       ├── Management Events (All Services)
   │       └── Data Events (S3 Buckets)
   │
   └── S3 Bucket (CloudTrail Logs Storage)
```

---

## 🧠 Project Description

This project sets up AWS CloudTrail using Terraform with:

* Multi-region trail enabled
* Management events for all AWS services
* Data events for S3 buckets
* Secure S3 bucket for storing logs

---

## ⚙️ Bucket Policy

```hcl
resource "aws_s3_bucket_policy" "bucket_policy" {
  bucket = aws_s3_bucket.cloudtrail_bucket.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid = "AWSCloudTrailAclCheck"
        Effect = "Allow"
        Principal = {
          Service = "cloudtrail.amazonaws.com"
        }
        Action = "s3:GetBucketAcl"
        Resource = aws_s3_bucket.cloudtrail_bucket.arn
      },
      {
        Sid = "AWSCloudTrailWrite"
        Effect = "Allow"
        Principal = {
          Service = "cloudtrail.amazonaws.com"
        }
        Action = "s3:PutObject"
        Resource = "${aws_s3_bucket.cloudtrail_bucket.arn}/AWSLogs/*"
        Condition = {
          StringEquals = {
            "s3:x-amz-acl" = "bucket-owner-full-control"
          }
        }
      }
    ]
  })
}
```

---

## 🚀 How to Run

```bash
terraform init
terraform plan
terraform apply
```

---

## 🔍 Output Verification

After deployment:

* Go to AWS Console → CloudTrail
* Ensure logging is enabled
* Check S3 bucket → logs should be stored in:

```
AWSLogs/
```

---

## 👨‍💻 Author

Harsh Kashyap

