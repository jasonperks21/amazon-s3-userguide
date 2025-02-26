# Getting started using the AWS CLI and SDK for Java<a name="S3OutpostsGSCLIJava"></a>

With Amazon S3 on Outposts, you can create S3 buckets on your AWS Outposts and easily store and retrieve objects on premises for applications that require local data access, local data processing, and data residency\. S3 on Outposts provides a new storage class, S3 Outposts \(`OUTPOSTS`\), which uses the S3 APIs, and is designed to store data durably and redundantly across multiple devices and servers on your AWS Outposts\. You communicate with your Outpost bucket by using an access point and endpoint connection over a virtual private cloud \(VPC\)\. You can use the same APIs and features on Outpost buckets as you do on Amazon S3 buckets, including access policies, encryption, and tagging\. You can use S3 on Outposts through the AWS Management Console, AWS CLI, AWS SDKs, or REST API\. For more information, see [What is Amazon S3 on Outposts?](S3onOutposts.md)

To get started with S3 on Outposts, you must create a bucket, an access point, and an endpoint\. Then, you can upload objects to your bucket\. The following examples show you how to get started with S3 on Outposts using the AWS CLI and SDK for Java\. To get started using the console, see [Getting started using the AWS Management Console](S3OutpostsGSConsole.md)\.

**Topics**
+ [Step one: Create a bucket](#S3OutpostsGSCreateBucket)
+ [Step two: Create an access point](#S3OutpostsGSCreateAccessPoint)
+ [Step three: Create an endpoint](#S3OutpostsGSCreateEndpoint)
+ [Step four: Upload an object to an S3 on Outposts bucket](#S3OutpostsUploadObjects)

## Step one: Create a bucket<a name="S3OutpostsGSCreateBucket"></a>

The following AWS CLI and SDK for Java examples show you how to create an S3 on Outposts bucket\.

------
#### [ AWS CLI ]

**Example**  
The following example creates an S3 on Outposts bucket \(`s3-outposts:CreateBucket`\) using the AWS CLI\.   

```
aws s3control create-bucket --bucket example-outpost-bucket --outpost-id op-01ac5d28a6a232904
```

------
#### [ SDK for Java ]

**Example**  
The following example creates an S3 on Outposts bucket \(`s3-outposts:CreateBucket`\) using the SDK for Java\.   

```
import com.amazonaws.services.s3control.model.*;

public String createBucket(String bucketName) {

    CreateBucketRequest reqCreateBucket = new CreateBucketRequest()
            .withBucket(bucketName)
            .withOutpostId(OutpostId)
            .withCreateBucketConfiguration(new CreateBucketConfiguration());

    CreateBucketResult respCreateBucket = s3ControlClient.createBucket(reqCreateBucket);
    System.out.printf("CreateBucket Response: %s%n", respCreateBucket.toString());

    return respCreateBucket.getBucketArn();

}
```

------

## Step two: Create an access point<a name="S3OutpostsGSCreateAccessPoint"></a>

To access your Amazon S3 on Outposts, you must create and configure an access point\. These examples how you how to create an endpoint using the AWS CLI and the SDK for Java\.

Access points simplify managing data access at scale for shared datasets in Amazon S3\. Access points are named network endpoints that are attached to buckets that you can use to perform Amazon S3 object operations, such as `GetObject` and `PutObject`\. With S3 on Outposts, you must use access points to access any object in an Outposts bucket\. Access points support only virtual\-host\-style addressing\.

------
#### [ AWS CLI ]

**Example**  
The following AWS CLI example creates an access point for an Outposts bucket\.  

```
aws s3control create-access-point --account-id 123456789012 --name example-Outposts-Access-Point --bucket "arn:aws:s3-outposts:region:123456789012:outpost/op-01ac5d28a6a232904/bucket/example-outpost-bucket" --vpc-configuration VpcId=example-vpc-12345
```

------
#### [ SDK for Java ]

**Example**  
The following SDK for Java example creates an access point for an Outposts bucket\.  

```
import com.amazonaws.services.s3control.model.*;

public String createAccessPoint(String bucketArn, String accessPointName) {

    CreateAccessPointRequest reqCreateAP = new CreateAccessPointRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn)
            .withName(accessPointName)
            .withVpcConfiguration(new VpcConfiguration().withVpcId("vpc-12345"));

    CreateAccessPointResult respCreateAP = s3ControlClient.createAccessPoint(reqCreateAP);
    System.out.printf("CreateAccessPoint Response: %s%n", respCreateAP.toString());

    return respCreateAP.getAccessPointArn();

}
```

------

## Step three: Create an endpoint<a name="S3OutpostsGSCreateEndpoint"></a>

To route requests to an S3 on Outposts access point, you must create and configure an S3 on Outposts endpoint\. Each virtual private cloud \(VPC\) on your Outpost can have one associated endpoint\. For more information about endpoint quotas, see [ S3 on Outposts network requirements](S3OnOutpostsRestrictionsLimitations.md#S3OnOutpostsConnectivityRestrictions)\. You must create these endpoints to be able to access your Outposts buckets and perform object operations\. For more information, see [Endpoints](S3OutpostsWorkingBuckets.md#S3OutpostsEP)\.

These examples show you how to create an endpoint using the AWS CLI and the SDK for Java\. For more information about permissions required to create and manage endpoints, see [Permissions for S3 on Outposts endpoints](S3OutpostsIAM.md#S3OutpostsEndpointPermissions)\.

------
#### [ AWS CLI ]

**Example**  
The following AWS CLI example creates an endpoint for an Outpost using the AWS VPC resource access type\. The VPC is derived from the subnet\.  

```
aws s3outposts create-endpoint --outpost-id op-01ac5d28a6a232904 --subnet-id subnet-8c7a57c5 --security-group-id sg-ab19e0d1
```
The following AWS CLI example creates an endpoint for an Outpost using the customer\-owned IP address pool \(CoIP pool\) access type\.  

```
aws s3outposts create-endpoint --outpost-id op-01ac5d28a6a232904 --subnet-id subnet-8c7a57c5 --security-group-id sg-ab19e0d1 --access-type CustomerOwnedIp --customer-owned-ipv4-pool ipv4pool-coip-12345678901234567
```

------
#### [ SDK for Java ]

**Example**  
The following SDK for Java example creates an endpoint for an Outpost\.  

```
import com.amazonaws.services.s3outposts.AmazonS3Outposts;
import com.amazonaws.services.s3outposts.AmazonS3OutpostsClientBuilder;
import com.amazonaws.services.s3outposts.model.CreateEndpointRequest;
import com.amazonaws.services.s3outposts.model.CreateEndpointResult;

public void createEndpoint() {
    AmazonS3Outposts s3OutpostsClient = AmazonS3OutpostsClientBuilder
                .standard().build();
                
    CreateEndpointRequest createEndpointRequest = new CreateEndpointRequest()
                .withOutpostId("op-0d79779cef3c30a40")
                .withSubnetId("subnet-8c7a57c5")
                .withSecurityGroupId("sg-ab19e0d1")		
                .withAccessType("CustomerOwnedIp")   
                .withCustomerOwnedIpv4Pool("ipv4pool-coip-12345678901234567");
    // Use .withAccessType and .withCustomerOwnedIpv4Pool only when the access type is 
    // customer-owned IP address pool (CoIP pool) 
    CreateEndpointResult createEndpointResult = s3OutpostsClient.createEndpoint(createEndpointRequest);
    System.out.println("Endpoint is created and its ARN is " + createEndpointResult.getEndpointArn());
}
```

------

## Step four: Upload an object to an S3 on Outposts bucket<a name="S3OutpostsUploadObjects"></a>

Objects are the fundamental entities stored in S3 on Outposts\. Every object is contained in a bucket\. You must use access points to access any object in an Outpost bucket\. When you specify the bucket for object operations, you use the access point Amazon Resource Name \(ARN\), which includes the AWS Region code for the Region the Outpost is homed to, AWS account ID, Outpost ID, and access point name\. The following example shows the ARN format for S3 on Outposts access points in object operations:

```
arn:aws:s3-outposts:region:account-id:outpost/outpost-id/accesspoint/accesspoint-name
```

With S3 on Outposts, object data is always stored on the Outpost\. When AWS installs an Outpost rack, your data stays local to your Outpost to meet data residency requirements\. Your objects never leave your Outpost and are not in an AWS Region\. Because the AWS Management Console is hosted in\-Region, you can't use the console to upload or manage objects in your Outpost\. However, you can use the REST API, AWS CLI, and AWS SDKs to upload and manage your objects through your access points\.

The following AWS Command Line Interface \(AWS CLI\) and AWS SDK for Java examples show you how to upload an object to an S3 on Outposts bucket using an access point\.

------
#### [ AWS CLI ]

**Example**  
The following example puts an object named `sample-object.xml` into an S3 on Outposts bucket \(`s3-outposts:PutObject`\) using the AWS CLI\. The example includes the object key and access point ARN \(with the AWS account ID, Outposts ID, and access point name\):  

```
aws s3api put-object --bucket arn:aws:s3-outposts:Region:123456789012:outpost/op-01ac5d28a6a232904/accesspoint/example-Outposts-Access-Point --key sample-object.xml --body sample-object.xml
```

------
#### [ SDK for Java ]

**Example**  
The following example puts an object into an S3 on Outposts bucket using the SDK for Java\. For more information, see [Uploading objects](upload-objects.md)\.  

```
import com.amazonaws.AmazonServiceException;
import com.amazonaws.SdkClientException;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.s3.model.ObjectMetadata;
import com.amazonaws.services.s3.model.PutObjectRequest;

import java.io.File;

public class PutObject {
    public static void main(String[] args) {
        String accessPointArn = "*** access point ARN ***";
        String stringObjKeyName = "*** String object key name ***";
        String fileObjKeyName = "*** File object key name ***";
        String fileName = "*** Path to file to upload ***";

        try {
            // This code expects that you have AWS credentials set up per:
            // https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html
            AmazonS3 s3Client = AmazonS3ClientBuilder.standard()
                    .enableUseArnRegion()
                    .build();

            // Upload a text string as a new object.
            s3Client.putObject(accessPointArn, stringObjKeyName, "Uploaded String Object");

            // Upload a file as a new object with ContentType and title specified.
            PutObjectRequest request = new PutObjectRequest(accessPointArn, fileObjKeyName, new File(fileName));
            ObjectMetadata metadata = new ObjectMetadata();
            metadata.setContentType("plain/text");
            metadata.addUserMetadata("title", "someTitle");
            request.setMetadata(metadata);
            s3Client.putObject(request);
        } catch (AmazonServiceException e) {
            // The call was transmitted successfully, but Amazon S3 couldn't process
            // it, so it returned an error response.
            e.printStackTrace();
        } catch (SdkClientException e) {
            // Amazon S3 couldn't be contacted for a response, or the client
            // couldn't parse the response from Amazon S3.
            e.printStackTrace();
        }
    }
}
```

------