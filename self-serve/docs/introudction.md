# Introduction

The Cloud deployment for Self-serve takes two ways which totally resides in two different envrionments.

1. Client infrastructure
2. Blotout Infrastructure

You need to decide **one** path from the below two.

## Client Infrastructure
In this all the infrastructure related resources are deployed in the client's cloud provider. For e.g. If client already has an account in the AWS cloud then it can use that account to deploy the self-serve resource in that account itself.

## Blotout infrastructure
In this Blotout deploys the entire infra in a new client account under the management of the parent Blotout account. For e.g. If the client wishes to outsource the entire infrastructure to Blotout (AWS) then it will have an account which will be under the Blotout Management Account.

# Pre-requisities
There are few pre-requisites that you need to have two start the process of Self-serve deployment

### Common requirements
1. Organization name - Preferred name of the organization
2. Preferred region - These regions are AWS specific. Chose one of the following [regions](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Regions)
3. Envrionment type - (prod/production , dev/qa/test/testing/sandbox)
4. UI mail ID - Preferred mail ID which will be helpful for the client to login. If the client has opted for Google login then the value for this will be `devops@blotout.io`
5. SDK domain - Domain for SDK
6. CDN domain - Domain to host CDN

### Client infrastructure
1. AWS access key
2. AWS secret key
3. Giving the following access to the user with above access key and secret key

### Blotout infrastructure
1. Email ID - Your preferred mail ID to provision an AWS account. 

**Note:** This mail ID should have never been used to create an AWS account before.

# Deployment