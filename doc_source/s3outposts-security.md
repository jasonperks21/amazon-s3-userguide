# Security in S3 on Outposts<a name="s3outposts-security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from data centers and network architectures that are built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/)\. To learn about the compliance programs that apply to Amazon S3 on Outposts, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using S3 on Outposts\. The following topics show you how to configure S3 on Outposts to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your S3 on Outposts resources\. 

## Data encryption in S3 on Outposts<a name="s3-outposts-data-encryption"></a>

By default, all data stored in Amazon S3 on Outposts is encrypted using server\-side encryption with Amazon S3 managed encryption keys \(SSE\-S3\)\. You can optionally use server\-side encryption with customer\-provided encryption keys \(SSE\-C\) by specifying an encryption key as part of your object API requests\. Server\-side encryption encrypts only the object data, not the object metadata\.

For more information about server\-side encryption with customer\-provided keys \(SSE\-C\), see [Protecting data using server\-side encryption with customer\-provided encryption keys \(SSE\-C\)](ServerSideEncryptionCustomerKeys.md)\.