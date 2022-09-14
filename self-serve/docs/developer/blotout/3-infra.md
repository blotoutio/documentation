# Step 3 - Provision the hardware infrastructure

In this step we will provision the hardware infrastructure required by the organization.

1. All the below operations are to be performed under the terraform organization created in the step 2.
2. Setting up the workspace
    - Create a workspace with name `infra` prefixed with the organization's environment type. For e.g. if env is `prod` then the workspace name will be `prod-infra`.
    - Choose `/dev/infra` or `/prod/infra` in the `terraform` github repository for version control with default branch pointing to `master`.
    - Configure the following variables for the workspace
        1. `apps_default_hosted_api_user_service_key` - (sensitive) Blotout user service key of DNS provider
        2. `apps_default_hosted_api_token` - (sensitive) Blotout token of DNS provider
        3. `apps_default_hosted_zone_id` - (sensitive) Blotout Zone ID
3. Attach this workspace to the respective variable set of the organization created in Step 2.
4. Run the pipeline.
    - Click on `Actions` and then `Start new run` to start a new run.
    - Below variables are present in the output
        1. `account_id` - account ID of the client
        2. `loadbalancer` - aws loadbalancer that the UI will point to
        3. `organization_name` - this will consists of the client's preferred orgnization name and the env type separated by `_`. It is the name of the EKS cluster and other resources in AWS. For e.g. if the env is `dev` and organization name is `goldilocks` then output will be `goldilocks_dev`
        4. `region` - client's preferred region
    - From above only `loadbalancer` is useful for client. Rest other are less useful.

# Links
- [Workspace in terraform organization](../terraform/workspace.md)
- [Workspace variables](../terraform/workspace_variables.md)
- [Starting new run](../terraform/action.md#starting-new-run)
- [Output](../terraform/action.md#output)