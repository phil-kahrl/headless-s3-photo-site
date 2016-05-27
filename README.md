# headless-s3-photo-site

This project allows one to create a photo sharing site hosted on AWS S-3.

The site is 'headless' because it does not require a back-end application server.  Perhaps a better word would be 'serverless'
because the website does not require a backend server.

The site use the AWS JavaScript API for S-3 to store and retrieve images.
The site is built on AngularJS and Bootstrap.


Example site:  http://pkahrl-headless-s3.s3-website-us-west-2.amazonaws.com

USAGE - BRIEF SUMMARY

Create and configure an AWS bucket.  Modify and upload to the files "index.html" and "error.html" to the bucket.  Create a folder in the bucket to hold your photos.  Update the website by adding or editing photos in the folder.


USAGE - DETAILS

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
			"Resource": "arn:aws:s3:::<BUCKET_NAME>/<IMAGE_FOLDER>/*"
		}
	]
}

substitute in th ename of the bucket and folder created in step 5 for BUCKET_NAME and IMAGE_FOLDER

7.  Modify the index.html file according to the insturction in "CONFIGURATION".  Upload the files "index.html" and "error.html" to your S-3 bucket.  Under Properties -> Permissions for the bucket, add "Everyone" and check permissions "Open/Download".

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

2.  Clone this project from GitHub and open "index.html" in your favorite text editor.

See the following JavaScript variables at the top file:

  BASE_URL - From the AWS console for your bucket use the URL provided in "Properties -> Static Website Hosting -> endpoint"
  BUCKET_NAME - The name of your S-3 bucket.
  PREFIX - The name of the folder in the S-3 bucket where your photos will be kept.
  SITE_TITLE - This will be the title in the of the website.
  SITE_BANNER - The banner text for the site.
  WELCOME_MSG - The welcome message for the site.

3.  Upload the modified index.html file to to your bucket.  Make sure you have read permissions for "Everyone" for "Open/Download" on the file after you upload it.

4.  You can now add new photos to your site by uploading to the folder for images.  The navigation for photos will be based on the last modified time for each file.
