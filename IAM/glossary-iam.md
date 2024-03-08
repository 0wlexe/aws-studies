# Glossary for AWS IAM Policies

### Role

- An IAM identity that you can create in your account that has specific permissions.
- An IAM role has some similarities to an IAM user:
  - Roles and users are both AWS identities with permissions policies that determine what the identity can and cannot do in AWS.
  - However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it.
  - A role does not have standard long-term credentials such as a password or access keys associated with it. Instead, when you assume a role, it provides you with temporary security credentials for your role session.

- Roles can be used by the following:
  - An IAM user in the same AWS account as the role
  - An IAM user in a different AWS account than the role
  - A web service offered by AWS such as Amazon Elastic Compute Cloud (Amazon EC2)
  - An external user authenticated by an external identity provider (IdP) service that is compatible with SAML 2.0 or OpenID Connect, or a custom-built identity broker.

### AWS service role

- A service role is an IAM role that a service assumes to perform actions on your behalf. An IAM administrator can create, modify, and delete a service role from within IAM.

### AWS service role for an EC2 instance

- A special type of service role that an application running on an Amazon EC2 instance can assume to perform actions in your account. This role is assigned to the EC2 instance when it is launched. Applications running on that instance can retrieve temporary security credentials and perform actions that the role allows. For details about using a service role for an EC2 instance, see Using an IAM role to grant permissions to applications running on Amazon EC2 instances.

### AWS service-linked role

- A service-linked role is a type of service role that is linked to an AWS service. The service can assume the role to perform an action on your behalf. Service-linked roles appear in your AWS account and are owned by the service. An IAM administrator can view, but not edit the permissions for service-linked roles.
    Note
- If you are already using a service when it begins supporting service-linked roles, you might receive an email announcing a new role in your account. In this case, the service automatically created the service-linked role in your account. You don't need to take any action to support this role, and you should not manually delete it. 


### Role chaining 

- Is when you use a role to assume a second role through the AWS CLI or API. For example, RoleA has permission to assume RoleB. You can enable User1 to assume RoleA by using their long-term user credentials in the AssumeRole API operation. This returns RoleA short-term credentials. With role chaining, you can use RoleA's short-term credentials to enable User1 to assume RoleB.
- When you assume a role, you can pass a session tag and set the tag as transitive. Transitive session tags are passed to all subsequent sessions in a role chain.
- Role chaining limits your AWS CLI or AWS API role session to a maximum of one hour. When you use the AssumeRole API operation to assume a role, you can specify the duration of your role session with the DurationSeconds parameter. You can specify a parameter value of up to 43200 seconds (12 hours), depending on the maximum session duration setting for your role. However, if you assume a role using role chaining and provide a DurationSeconds parameter value greater than one hour, the operation fails.
- AWS does not treat using roles to grant permissions to applications that run on EC2 instances as role chaining.

### Delegation

- The granting of permissions to someone to allow access to resources that you control.
- Delegation involves setting up a trust between two accounts. The first is the account that owns the resource (the trusting account). The second is the account that contains the users that need to access the resource (the trusted account). The trusted and trusting accounts can be any of the following:
  - The same account.
  - Separate accounts that are both under your organization's control.
  - Two accounts owned by different organizations.

- To delegate permission to access a resource, you create an IAM role in the trusting account that has two policies attached. The permissions policy grants the user of the role the needed permissions to carry out the intended tasks on the resource. The trust policy specifies which trusted account members are allowed to assume the role.

### Federation

- The creation of a trust relationship between an external identity provider and AWS.
- Users can sign in to a web identity provider, such as Login with Amazon, Facebook, Google, or any IdP that is compatible with OpenID Connect (OIDC).
- When you use OIDC and SAML 2.0 to configure a trust relationship between these external identity providers and AWS, the user is assigned to an IAM role. The user also receives temporary credentials that allow the user to access your AWS resources.

### Federated user

- Instead of creating an IAM user, you can use existing identities from AWS Directory Service, your enterprise user directory, or a web identity provider.
- These are known as federated users. AWS assigns a role to a federated user when access is requested through an identity provider.

### Trust policy

- A role trust policy is a required resource-based policy that is attached to a role in IAM. The principals that you can specify in the trust policy include users, roles, accounts, and services.
- A JSON policy document in which you define the principals that you trust to assume the role.

### Permissions policy

- A permissions document in JSON format in which you define what actions and resources the role can use. The document is written according to the rules of the IAM policy language.

### Permissions boundary

- An advanced feature in which you use policies to limit the maximum permissions that an identity-based policy can grant to a role.
- You cannot apply a permissions boundary to a service-linked role. 

### Principal

- An entity in AWS that can perform actions and access resources. A principal can be an AWS account root user, an IAM user, or a role. You can grant permissions to access a resource in one of two ways:
  - You can attach a permissions policy to a user (directly, or indirectly through a group) or to a role.
  - For those services that support resource-based policies, you can identify the principal in the Principal element of a policy attached to the resourc
- If you reference an AWS account as principal, it generally means any principal defined within that account.

### Role for cross-account access

- A role that grants access to resources in one account to a trusted principal in a different account.
