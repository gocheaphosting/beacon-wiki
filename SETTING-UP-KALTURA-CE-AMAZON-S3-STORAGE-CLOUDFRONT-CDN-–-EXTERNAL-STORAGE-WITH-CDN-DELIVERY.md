SETTING UP KALTURA CE AMAZON S3 STORAGE CLOUDFRONT CDN – EXTERNAL STORAGE WITH CDN DELIVERY

Kaltura CE Amazon S3 Storage CloudFront CDN Integration

Setting up Amazon S3 and getting security credentials
1. To get your Amazon security credentials (assuming you have an account with amazon AWS), go to this link https://portal.aws.amazon.com/gp/aws/securityCredentials

2. To set up your amazon S3 bucket, go to https://console.aws.amazon.com/s3/home , create a new bucket, and name it.

3. Inside this bucket, create a folder called “kaltura”

4. Select your new bucket on the left side, click Actions and select “Properties”

5. Add more permissions – Authenticated Users – check all boxes.

6. Select the kaltura folder, click properties, go to Permissions.

7. Add more permissions – Everyone – read and download (you can also right click the folder and select “Make Public”)

Setting up Amazon CloudFront CDN
1. Go to https://console.aws.amazon.com/cloudfront/home

2. Create a new “Distribution” of type “Download”, and name it

3. Select your bucket as the origin ID, and decide wether you want logging or not.

4. Copy your CloudFront domain name (example: zdfx73d6dzff32z.cloudfront.net) for later use.

Setting up the Remote Storage Profile in the Admin Console
First, you must enable the necessary configuration options for your partner:

1. Find your partner in the list of partners, click on the right drop down box and select “Configure”

2. Under “Remote Storage Policy”, set Delivery Policy to “Remote Storage Only”

3. Check the “Delete exported storage” checkbox.

4. Under Enable/Disable Features, make sure that “Remote Storage” is checked.

5. Click “Save”.

Next we must configure the Remote Storage Profile. In order to do this, we must click on the partner’s left drop-down box (under “Profiles”) and select “Remote Storage”. You should see the “Remote Storage Profiles” page for your publisher (If you haven’t yet set up any remote storage profiles, the list should be empty).

(Assuming that you have already set up an S3 bucket, and that you have an Access Key ID and a Secret Access Key)

1. Create a new profile by writing your publisher id in the right “Publisher ID” input box and clicking “Create New”.

2. Give a name to your Remote Storage (for example “Amazon S3″)

3. For “Storage URL” type http://{yourbucketname}.s3.amazonaws.com (replace {yourbucketname} with your bucket name on S3)

4. In Storage Base Directory, write “/{yourbucketname}/kaltura” (keep in mind the leading slash, and change yourbucketname to your bucket name)

5. Storage Username – enter your amazon aws api Access Key ID

6. Storage Password – paste your amazon aws api Secret Access Key

7. Under HTTP Delivery Base URL, type “http://{your amazon cloudfront domain}/kaltura” – replace {your amazon cloudfront domain} with the cloudfront domain you created in the previous section).

8. Save the new Remote Storage Profile

Add a crossdomain.xml file
Create a crossdomain.xml file in the root of your S3 bucket

```
<cross-domain-policy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://www.adobe.com/xml/schemas/PolicyFile.xsd">
    <allow-access-from domain="*" to-ports="*" secure="false"/>
    <site-control permitted-cross-domain-policies="all"/>
    <allow-http-request-headers-from domain="*" headers="*"/>
</cross-domain-policy>
```
Final Step – Enable the remote storage profile
1. Click on the dropdown box next to your new storage profile in the Remote Storage Profiles page in Kaltura Admin Console

2. Select “Export Automatically” and then click “OK”

3. You will receive the confirmation that your storage was autoed :)

Test your new configuration
You can go ahead and test your new configuration. Upload a new video in the KMC, let it convert, and wait for it to get distributed. After that, try to play the entry and analyse it in your favorite sniffer. You should see that the movies are being downloaded from your cloudfront CDN, look for flv and mp4 files.