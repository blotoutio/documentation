# Step 2 - Creating the terraform organization

After we have our client's organization created under the Blotout SSO, we will now create the terraform organization for our client.

1. Create a terraform organization with the client's organization name prefixed with `client-`. For e.g. when organization name is `goldilocks` the terraform organization name for the client will be `client-goldilocks`.
2. Create a variable set with the name of the envrionment. If organization's environment is `dev` then create a variable set `dev-varset`.
    - In the workspaces section of the variable set we can select any of the two options `Apply to all workspaces in this organization` or `Apply to specific workspaces` for now. We will change this option later.
    - Create the following variables which will be used in all the pipelines that will run for all the organization.
        1. `organization_name` - organizaiton name of the client 
        2. `environment` - client's env type selected
        3. `aws_access_key` - (sensitive) access key of the organization. We retrieved this in Step 1.
        4. `aws_secret_key` - (sensitive) secret key of the organization. We retrieved this in Step 1.
        5. `aws_region` - region preferred by client
        6. `blotout_app_release_version` - current stable version of self-serve
        7. `logging_level_com_blot` - (preferrable value: `DEBUG`)

# Links
1. [Terraform organization](../terraform/organization.md)
2. [Workspace in terraform organization](../terraform/workspace.md)
3. [Variable sets](../terraform/variable_sets.md)
4. [Workspace variables](../terraform/workspace_variables.md)
5. [Starting new run](../terraform/action.md#starting-new-run)
6. [Output](../terraform/action.md#output)
7. [Google SSO](../client/sso.md)
8. [Slack](../client/slack.md)