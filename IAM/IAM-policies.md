## Types of IAM Policies

AWS supports six types of policies: identity-based policies, resource-based policies, IAM permissions boundaries, 
AWS Organizations service control policies (SCPs), access control lists (ACLs), and session policies. All of these 
polices are evaluated before a request is either allowed or denied. 

### Identity-based

- Identity-based policies grant permissions to an identity (users, groups to which users belong, or roles)
- They are either managed or inline:

  - **Managed:** An AWS managed policy is a standalone policy that is created and administered by AWS. Standalone policy means that the policy has its own Amazon Resource Name (ARN) that includes the policy name.
 
    The following diagram illustrates AWS managed policies. The diagram shows three AWS managed policies: [**AdministratorAccess**](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AdministratorAccess.html),
    [**PowerUserAccess**](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/PowerUserAccess.html), and [**AWSCloudTrailReadOnlyAccess**](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudTrail_ReadOnlyAccess.html).
    Notice that a single AWS managed policy can be attached to principal entities in different AWS accounts, and to different principal entities in a single AWS account. 

   ![Alt text](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/policies-aws-managed-policies.diagram.png)

  - Policies can also be **Customer managed** - These are policies that you create and manage in your AWS account.

    This type of policy provides more precise control than AWS managed policies and can also be attached to multiple users, groups, and roles.

    The following diagram illustrates customer managed policies. Each policy is an entity in IAM with its own Amazon Resource Name (ARN) that includes the policy name. Notice that the same policy can be attached to multiple principal entities—for example,
    the same DynamoDB-books-app policy is attached to two different IAM roles.
    
   ![Alt text](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/policies-customer-managed-policies.diagram.png)

   - **Inline:**  Policies that you add directly to a single user, group, or role. Inline policies maintain a strict one-to-one relationship between a policy and an identity.

     They are deleted when you delete the identity.

     The following diagram illustrates inline policies. Each policy is an inherent part of the user, group, or role. Notice that two roles include the same policy (the DynamoDB-books-app policy), but they are not sharing a single policy.
     Each role has its own copy of the policy.
   
   ![Alt text](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/policies-inline-policies.diagram.png)

### Resource-based

- These are inline policies that are attached to AWS resources.
- The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies.
- Resource-based policies grant permissions to the principal that is specified in the policy; hence, the principal policy element is required.
- Grants permission to principals or accounts (same or different accounts)

  For example, the resource-based policy below is attached to an Amazon S3 bucket. According to the policy, only the IAM user carlossalzar can access this bucket.

   ![Alt text](https://osamaoraclecom.files.wordpress.com/2021/08/2021-08-15_18-12-51-1.png)

### Permissions boundaries

- A permissions boundary sets the maximum permissions that an identity-based policy can grant to an IAM entity.
- The entity can perform only the actions that are allowed by both its identity-based policies and its permissions boundaries.
- *Resource-based policies that specify the user or role as the principal are not limited by the permissions boundary.*

  For example, assume that one of your IAM users should be allowed to manage only Amazon S3, Amazon CloudWatch, and Amazon EC2.
  To enforce this rule, you can use the customer-managed policy enclosed in the square to set the permissions boundary for the user.
  Then, add the condition block below to the IAM user’s policy. The user can never perform operations in any other service, including IAM, even if it has a permissions policy that allows it.

   ![Alt text](https://osamaoraclecom.files.wordpress.com/2021/08/2021-08-15_18-12-51-2.png)

### Service control policies (SCPs)

- AWS Organizations service control policies (SCPs) is a service for grouping and centrally managing AWS accounts.
- If you enable all features in an organization, then you can apply SCPs to any or all of your accounts.
- SCPs specify the maximum permissions for an account, or a group of accounts, called an organizational unit (OU).
- Restricts permissions for entities in an AWS account, including AWS account root users
- SCPs alone are not sufficient in granting permissions to the accounts in your organization.
- No permissions are granted by an SCP - The administrator must still attach identity-based or resource-based policies to IAM users or roles, or to the resources in your accounts to actually grant permissions.
- SCP policies may appear to be identical to IAM policies at first glance. Both have access control and adhere to principles such as the least privilege access system.
  However, their respective scopes and applications are vastly different. Here are some of the most significant differences between the two systems.
- AWS SCP control policies are applied at the account level, and a user within a specific account can define an IO policy within the account root user’s SCP limits.

  For example, here is a sample SCP policy that restricts access to Amazon EC2 actions:

```shell
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAccessToEC2Actions",
            "Effect": "Deny",
            "Action": "ec2:*",
            "Resource": "*"
        }
    ]
}
```

  On the other hand, IAM policies are applied at the user or group level and define permissions for individual users or groups within an account. 
  The following is an example of an IAM user policy that grants access to Amazon S3 buckets:

```shell
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::examplebucket/*"
        }
    ]
}
```
  In this example, the IAM policy grants permission to read and write objects in an S3 bucket named “examplebucket”. 
  This policy can be applied to specific other users and roles or groups in an AWS account.

### Access control lists (ACLs)

- Access control lists (ACLs) are service policies that allow you to control which principals in another account can access a resource.
- ACLs cannot be used to control access for a principal within the same account.
- ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document format.
- Amazon S3, AWS WAF, and Amazon VPC are examples of services that support ACLs.

  When you create a bucket or an object, Amazon S3 creates a default ACL that grants the resource owner full control over the resource.

  This is shown in the following sample bucket ACL (the default object ACL has the same structure):


```shell
<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>*** Owner-Canonical-User-ID ***</ID>
    <DisplayName>owner-display-name</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
               xsi:type="Canonical User">
        <ID>*** Owner-Canonical-User-ID ***</ID>
        <DisplayName>display-name</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy> 
```

### Session policies

   - AWS Session policy is a limiting AWS policy, which limits the maximum permission for a particular AWS session (assumed role session or user federated session).
   - The permissions for a session are the intersection of the identity-based policies for the IAM entity (user or role) used to create the session and the session policies.
   - Permissions can also come from a resource-based policy. An explicit deny in any of these policies overrides the allow.
   - It is very similar to Service Control Policy (SCP) in the way that instead of granting or revoking permissions, it is used to limit the total permissions. But the way it differs from SCP is that instead of applying these maximum permissions to an account or OU, Session policy applies the maximum permissions to an assumed session.
   - For example, imagine you are assuming a role or federating a user (creating temporary credentials for a user), you are creating a temporary session from those identities and you will be given an opportunity to supply Session policies to restrict permissions in those specific time-bound sessions.

     The policy follows the same JSON structure as other policy types with standard elements.

```shell
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:ListBucket",
    "Resource": "arn:aws:s3:::example_bucket"
  }
}
```




