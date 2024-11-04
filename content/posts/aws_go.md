+++
title = "Beginner's Guide to Managing Files in Amazon S3 with Go"
date = "2024-11-04"
author = "Ukeje Goodness"
description = "This article assumes that you’re an AWS beginner and walks you through the processes of using S3 for
storage from your Go apps."
[taxonomies]
tags = ["Golang", "development", "Go", "API", "Backend", "Postgres", "Docker", "DevOps"]
+++
---




Amazon Simple Storage Service (S3) has become the industry standard for web-based file storage. It is designed for up to
99.999999999% durability and security. It also has flexible storage classes, data management, and analytics features.

Are you building the server-side app for your project in Go and need a storage option? This article assumes that you’re
an AWS beginner and walks you through the processes of using S3 for storage from your Go apps.

## Getting Started with AWS and Go

Setting up an AWS S3 bucket with the right permissions and parameters can be daunting. You’ll have to create a bucket,
create an IAM user, issue permissions to the IAM user to execute operations on the bucket, and set up the access keys in
your environment.

First, create an Amazon AWS account and sign in if you don’t have one yet. Then, search for S3 like this.

![S3](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/846nlq6n6twslc7gdb8o.png)

Now, Click on “Create a bucket.” You’ll be prompted to configure the bucket according to your project’s specifications.

![Create Bucket](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dubsmbm8v76530l95llz.png)

Now that you’ve created a bucket, the next step is to set up an IAM user to whom you’ll issue permission to access the
S3 bucket.

Head to the security credentials section of your profile and create a user.

![Create User](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bedgpv9e7agdvmmvea18.png)

Then, create an access key for the user and retrieve the access and secret keys.

![CloudBox IAM](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k2objf4thev9p9mt1j2b.png)

On your computer, in the root directory, create a `.aws` folder and a file named `credentials` with no extension; then
add the keys like this:

```toml
[default]
aws_access_key_id = <aws_access_key_id here>
aws_secret_access_key = <aws_secret_access_key here>
```

Now, Issue these permissions to the user to allow S3 uploads.

![Permission Policies](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0epj35zjai6qwlarnepk.png)

Finally, initialize a Go project and install the AWS Go SDK.

```bash
go mod tidy
go get github.com/aws/aws-sdk-go
```

You’re all set up, and now, you can start uploading, downloading and managing files with AWS S3 Buckets.

## Uploading Files to an S3 Bucket

First, you must import the necessary packages from the AWS SDK package.

Add these modules to the top of your `main.go` file or whichever file you’re using:

```go
package main

import (
	"bytes"
	"fmt"
	"io"
	"os"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)
```

To upload a file, you must create a new session, open a file, and use the session instance to upload the file.

```go
func UploadFile(bucket, region, filePath, key, acl string) error {
	sess, err := session.NewSession(&aws.Config{
		Region: aws.String(region),
	})
	if err != nil {
		return fmt.Errorf("error creating session: %w", err)
	}
	file, err := os.Open(filePath)
	if err != nil {
		return fmt.Errorf("error opening file: %w", err)
	}
	defer file.Close()

	var buf bytes.Buffer
	if _, err := io.Copy(&buf, file); err != nil {
		return fmt.Errorf("error reading file: %w", err)
	}

	_, err = s3.New(sess).PutObject(&s3.PutObjectInput{
		Bucket: aws.String(bucket), 
		Key:    aws.String(key),          
		Body:   bytes.NewReader(buf.Bytes()), 
		ACL: aws.String(acl),     
	})
	if err != nil {
		return fmt.Errorf("error uploading file: %w", err)
	}

	fmt.Println("File uploaded successfully:", filePath, "to key:", key)
	return nil
}
```

The `UploadFile` function takes in the bucket name, AWS region, the file key and ACL (access controls list) and uploads
the specified file to S3 with the specified key.

The `session.NewSession` function creates a new AWS session and [`s3.New`](http://s3.New) creates a new S3 session. The
`PutObject` function takes in a reference to the `PutObjectInput` with the file Bucket, Key, file body, and ACL.

```go
func main() {
	bucket := "cloudboxbucket"  
	region := "eu-north-1"      
	filePath := "Makefile"     
	key := "Makefile"           
	acl := "private"         

	if err := UploadFile(bucket, region, filePath, key, acl); err != nil {
		fmt.Println("Error uploading file:", err)
	}
}
```

When I called the `UploadFile` function with the parameters, here’s proof that the file was uploaded to my S3 bucket.

![Upload Files](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kpfr306qoz245b62l4fg.png)

You can always browse more details of files by clicking the file name on the AWS S3 console.

## Downloading Files from an S3 Bucket

Now, you can attempt to retrieve the file with the key you’ve specified.

```go
func DownloadFile(bucket, region, key, destPath string) error {
	sess, err := session.NewSession(&aws.Config{
		Region: aws.String(region),
	})
	if err != nil {
		return fmt.Errorf("error creating session: %w", err)
	}

	s3Client := s3.New(sess)

	output, err := s3Client.GetObject(&s3.GetObjectInput{
		Bucket: aws.String(bucket),
		Key:    aws.String(key),
	})
	if err != nil {
		return fmt.Errorf("error downloading file: %w", err)
	}
	defer output.Body.Close()

	destFile, err := os.Create(destPath)
	if err != nil {
		return fmt.Errorf("error creating destination file: %w", err)
	}
	defer destFile.Close()

	if _, err := io.Copy(destFile, output.Body); err != nil {
		return fmt.Errorf("error saving file: %w", err)
	}

	fmt.Println("File downloaded successfully:", key, "to", destPath)
	return nil
}
```

After creating the client instance, the `GetObject` function receives the file parameters in the `&s3.GetObjectInput`
instance. You can now copy the file stream to your preferred destination with `io.Copy` function.

```go
func main() {
	bucket := "cloudboxbucket"  
	region := "eu-north-1"      
	key := "Makefile"          
	destPath := "internal/database/DownloadedMakefile"

	if err := DownloadFile(bucket, region, key, destPath); err != nil {
		fmt.Println("Error downloading file:", err)
	}
}
```

The file should be downloaded to your specified destination path when executing a program with the `DownloadFile`
function.

![Download File](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/anmg596kmx47tlsd0a3e.png)

You’ll need to specify the key in the key in the destination path as well.

## Deleting Files from S3 Buckets

To delete a file from an S3 bucket, you’ll use the AWS SDK’s `DeleteObject` function to remove the file. You must
specify the S3 bucket name and the file key you want to delete.

```go
func DeleteFile(bucket, region, key string) error {
	sess, err := session.NewSession(&aws.Config{
		Region: aws.String(region),
	})
	if err != nil {
		return fmt.Errorf("error creating session: %w", err)
	}

	s3Client := s3.New(sess)

	_, err = s3Client.DeleteObject(&s3.DeleteObjectInput{
		Bucket: aws.String(bucket),
		Key:    aws.String(key),
	})
	if err != nil {
		return fmt.Errorf("error deleting file: %w", err)
	}

	err = s3Client.WaitUntilObjectNotExists(&s3.HeadObjectInput{
		Bucket: aws.String(bucket),
		Key:    aws.String(key),
	})
	if err != nil {
		return fmt.Errorf("error waiting for file deletion: %w", err)
	}

	fmt.Println("File deleted successfully:", key)
	return nil
}
```

The `DeleteFile` function starts by creating a session, like in the upload and download examples. The
`s3Client.DeleteObject` is called with a `DeleteObjectInput` struct, where you’ll specify the Bucket and Key parameters.

You can use the `WaitUntilObjectNotExists` function to confirm that the file doesn’t exist.

```go
func main() {
	bucket := "cloudboxbucket"   
	region := "eu-north-1"       
	key := "Makefile"           

	if err := DeleteFile(bucket, region, key); err != nil {
		fmt.Println("Error deleting file:", err)
	}
}
```

When executing the `DeleteFile` function, you can see that the Makefile no longer exists in my S3 bucket.

![Delete File](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dy7vxstdammklnjj89mz.png)

## Conclusion

You’ve learned how to set up an S3 bucket for an IAM user (it also works for root) and upload, download, and delete
files from S3 buckets.

There’s so much more functionality you can do with AWS S3 Buckets. I hope this article has provided the foundation for
you to go further, depending on your project’s use case.