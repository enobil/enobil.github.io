---
layout: post
title:  "AWS Systems Design Options For User Role Management"
date:   2022-06-01 14:00:00 +0300
categories: jekyll update
---

## Introduction

There are several design options to manage application level user roles in a platform running on AWS.  
Aiming to outline each option and compare.  

## Option 1: Maintaining Roles In AWS Cognito

In this type of options, the role data will be maintained within Cognito. 

# Pros
1. Allows stricter access control on the user role data (compared to application database) (more secure)
1. User role data stays in the Cognito / dedicated user management services (privacy)
1. Management functionalities are offloaded to Cognito (separation of concerns, maintainability)
1. Regulation / security review friendly, since otherwise application layer can have gaps to expose vulnerabilities

# Cons

# How it will be managed

AWS console / AWS SDK / AWS CLI  
or  
Application frontend -> application backend -> AWS SDK  

## Option 1.1: Using AWS Cognito Groups (Proposed)

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-user-groups.html  

The idea is maintaining one user group for each role.  
In terms of implementation, the group info is included in the JWT token as in [here](https://stackoverflow.com/a/43789307).  

# Pros

1. Allows group level operations  
    1. Allows attaching IAM policies to group level
1. (Looks like more idiomatic usage of Cognito about the user roles (to check further))

# Cons

# Related Quotas
1. Max number of group memberships per user: 100 (assuming <10 roles, highly sufficient)
1. Max number of groups per user pool: 10K (assuming <10 roles, highly sufficient)
1. SDK
    1. AdminAddUserToGroup -> 25 TPS (max 1500 requests/minute, 90K req/hour)
    1. AdminRemoveUserFromGroup -> 25 TPS (max 1500 requests/minute, 90K req/hour)
    1. AdminListGroupsForUser -> 50 TPS (max 3000 requests/minute, 180K req/hour)
    1. ListUsersInGroup -> 30 TPS (max 1800 requests/minute, 108K req/hour)
    1. GetGroup -> 20 TPS (max 1200 requests/minute, 72K req/hour)
    1. ListGroups -> 20 TPS (max 1200 requests/minute, 72K req/hour)
    1. CreateGroup -> 15 TPS (max 900 requests/minute, 54K req/hour)
    1. DeleteGroup -> 15 TPS (max 900 requests/minute, 54K req/hour)
    1. UpdateGroup -> 15 TPS (max 900 requests/minute, 54K req/hour)


## Option 1.2: Using AWS Cognito User Attributes

The idea is setting a custom attribute to maintain list of roles assigned to the user.  

https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-manage-user-accounts.html  

# Pros

# Cons

# Related Quotas
1. Custom attributes per user pool: 50 (1 needed as a roleList attribute)
1. Attribute payload: 2KB (<256B needed assuming <10 roles)
1. SDK
    1. AdminUpdateUserAttributes -> 25 TPS (max 1500 requests/minute, 90K req/hour)

## Option 2: Maintaining Roles In External Application DB

The idea is maintaining e.g. a table that maps user ids (e.g. email) to the role definitions.  

# Pros
1. Trivial to implement
1. Higher throughput (although wouldn't be needed most likely)

# Cons
1. Any bug will impact the security, better to offload as much responsibility to upstream AWS services as possible
1. User management responsibility will be spread between Cognito and the application layer instead of single responsibility

# Related Quotas
1. For DynamoDB, hard limit would be 40K RCU/sec, e.g. >100K TPS (although highly unnecessary)

# How it will be managed

Application frontend -> application backend -> DB