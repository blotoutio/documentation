# Step 1 - Creating AWS organiztion under Blotout SSO

The first step is to create the organization under Blotout SSO. For this, a pipeline is already created and we just need to trigger it with right variables.

1. A terraform organization with name `blotout-admin` is already created. This organiztaion is responsible for all the admin task which fall under the `Blotout Infrastructure` strategy.
2. Setting up client workspace in `blotout-admin` organiztion. Let's assume the name of the organization as ***goldilocks*** for the entire deployment

    - Create a workspace with name of the client's organization. The workspace should be named as the organization name i.e. `goldilocks`.
    - Choose `/admin` in the `terraform` github repository for version control with default branch pointing to `master`.
    - Configure the following variables for the workspace
        1. `organization_name` - preferred name of the client
        2. `organization_mail` - mail ID of the client
        3. `organization_region` - chosen region of the client
        4. `environment` - env type chosen by the client
        5. `iam_user` - name of the user through which we will create the resource in the client account. (default: `blotout-k8s`)
        6. `role_name` - name of the role with restrictive permission that will get created in the client account and will be attached to `iam_user` user (default: `self-serve-role`)
3. A variable set is already created in the `blotout-admin` terraform organization with name of `admin-varset`. We need to attach the client organization `goldilocks` if `Apply to specific workspaces` option is selected in the `Workspaces` section of the variable set. This variable set consists of 4 variables. 
    - `aws_access_key` - (secret) Access key of our blotout parent account. 
    - `aws_secret_key` - (secret) Secret key of our blotout parent account.
    - `aws_region` - AWS region (default: `us-east-1`)
    - `public_key` - public key to encrypt the `iam_user` credentials

    **Note:** - These variables are set with their static values. Changing this is ***not recommended***.

4. Run the pipeline. Once the pipeline successfully finishes we will get the following variables as output
    - `account_arn` - arn of the new client's account
    - `account_id` - account ID
    - `iam_user_details` - (sensitive) credentials of the `iam_user` encrypted by `public_key`
5. If we go to [Blotout SSO](https://blotout.awsapps.com/start/) we will be able to see the organization.
6. To obtain the credentials we need to first obtain the token and the workspace ID. 
    - Token is used to authenticate to the terraform cloud. To obtain Token 
        1. click on your profile in terraform cloud
        2. click on ***User settings***
        3. click on `Tokens`
        4. click on `Create an API token`. 
    - Workspace ID is the unique ID of the workspace.
        1. go inside `blotout-admin` terraform organization.
        2. Enter your workspace (here: `goldilocks`)
        3. Just below the name of the workspace you will find your workspace ID starting with `ws`.
7. To obtain the access key and secret key, you must have the private key corresponding to the public key used in the `public_key` to encrypt the output.

    ```
    # token and workspace ID here
    export TOKEN="xxx"
    export WORKSPACE_ID="xxx"

    # for access key
    export ACCESS_KEY=$(curl --silent --header "Authorization: Bearer ${TOKEN}" --header "Content-Type: application/vnd.api+json" https://app.terraform.io/$(curl --silent --header "Authorization: Bearer ${TOKEN}" --header "Content-Type: application/vnd.api+json" https://app.terraform.io/api/v2/workspaces/${WORKSPACE_ID}/current-state-version-outputs | jq -r '.data[] | select(.attributes.name=="iam_user_details") | .links.self') | jq '.data.attributes.value["access-key"]' --raw-output)

    # for secret key (you must have the private key loaded in gpg)
    export SECRET_KEY=$(curl --silent --header "Authorization: Bearer ${TOKEN}" --header "Content-Type: application/vnd.api+json" https://app.terraform.io/$(curl --silent --header "Authorization: Bearer ${TOKEN}" --header "Content-Type: application/vnd.api+json" https://app.terraform.io/api/v2/workspaces/${WORKSPACE_ID}/current-state-version-outputs | jq -r '.data[] | select(.attributes.name=="iam_user_details") | .links.self') | jq '.data.attributes.value["secret-key"]' --raw-output | base64 -d | gpg --decrypt)
    ```
8. You have your keys in the environment variable `ACCESS_KEY` and `SECRET_KEY`.

# Links
- [Terraform organization](../terraform/organization.md)
- [Workspace in terraform organization](../terraform/workspace.md)
- [Variable sets](../terraform/variable_sets.md)
- [Workspace variables](../terraform/workspace_variables.md)
- [Starting new run](../terraform/action.md#starting-new-run)
- [Output](../terraform/action.md#output)