# headless-s3-photo-site

This project allows one to create a photo sharing site hosted on AWS S-3.

The site is 'headless' because it does not require a back-end application server.

The site use the AWS JavaScript API for S-3 to store and retrieve images.
The site is built on AngularJS and Bootstrap.


Example site:  <TODO add example link >

USAGE

AWS setup.

1.  Obtain an AWS Account.  This does require a payment device.  Their is a beginning free tier after which you will
be charged for storage and network usage.  For a personal site hosting on S-3 should cost less $1 /month.

2.  Create an S-3 bucket in your AWS account.  Following the instructions here:  
http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html

3.  Enable static website hosting for your AWS bucket:  
http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html.  When enabling static hosting,
you will need to set the options for:

Index Document as "index.html"

and 

Error Document as "error.html"

4.  Allow public read access to the bucket you created.  Grant access to "Everyone" for operation "List".
5.  Create a folder in the bucket to hold your images.  
6.  Add a bucket policy to allow images to be retrieved from the above folder.  You do this by adding a "bucket policy" to the AWS bucket.  The bucket
policy will look like:

{
	"Version": "2008-10-17",
	"Statement": [
		{
			"Sid": "AllowPublicRead",
			"Effect": "Allow",
			"Principal": {
				"AWS": "*"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::<BUCKET_NAME>/<IMAGE_FOLDER>"
		}
	]
}

substitute in th ename of the bucket and folder created in step 5 for BUCKET_NAME and IMAGE_FOLDER

7.  Upload the files "index.html" and "error.html" to your S-3 bucket.  Under Properties -> Permissions for the bucket, add "Everyone" and check permissions "Open/Download".

8.  Set the CORS policy for the bucket.  Click on "Edit CORS configuration" for your bucket and paste in the following:

<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>PUT</AllowedMethod>
        <AllowedMethod>POST</AllowedMethod>
        <AllowedMethod>DELETE</AllowedMethod>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>


CONFIGURATION

1.  Your website needs to be customized by creating configuration for the settings of your AWS account.  This requires some modification of JavaScript files, but no actual coding.








