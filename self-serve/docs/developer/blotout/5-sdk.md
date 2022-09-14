# Step 5 - Provision the SDK

In this step we will provision the SDK under the client's domain.

1. All the below operations are to be performed under the terraform organization created in the step 2.
2. Setting up the workspace
    - Create a workspace with name `sdk` prefixed with the organization's environment type. For e.g. if env is `prod` then the workspace name will be `prod-sdk`.
    - Choose `/dev/sdk` or `/prod/sdk` in the `terraform` github repository for version control with default branch pointing to `master`.
    - Configure the following variables for the workspace
        1. `sdk_domains` - (HCL) client's domain where SDK is hosted. Replace `<domain>` with the client's domain.
        ```
        [{
            endpoint = "<domain>"
            dns_map_ack_success = false
        }]
        ```
3. Attach this workspace to the respective variable set of the organization created in Step 2.
4. Run the pipeline.
    - Click on `Actions` and then `Start new run` to start a new run.
    - Below variables are present in the output
        1. `loadbalancer` - loadbalancer for client's domain
        2. `sdk_domains_mapping` - DNS authentication to SDK domains. For e.g.
        ```
        [
          {
            "endpoint": "<domain>",
            "record_name": "xxx",
            "record_value": "xxx",
            "type": "CNAME"
          }
        ]
        ```

5. The above pipeline creates a SSL/TLS certificate under the client's domain. This certificate needs to be verified via the DNS strategy.
    - Client needs to create a CNAME record with the above `record_name` and `record_value` as name and target respectively.
    - Proxy status for the CNAME record should be kept off.
    - Create another CNAME record for client's domain, where the name is `endpoint` and target is `loadbalancer`. This step points the client's domain to the application deployed under the loadbalancer pointed by `loadbalancer`
6. Edit `dns_map_ack_success` in `sdk_domains` to `true` once client has confirmed the DNS records are in place.
    ```
    [{
        endpoint = "<domain>"
        dns_map_ack_success = true
    }]
    ```
7. Click on `Actions` and then `Start new run` to start a new run.
8. To verify the changes, hit the `/sdk/v1/geo/city` endpoint over client's domain `<domain>`. This will give your geo location.

# Links
- [Workspace in terraform organization](../terraform/workspace.md)
- [Workspace variables](../terraform/workspace_variables.md)
- [Starting new run](../terraform/action.md#starting-new-run)
- [Output](../terraform/action.md#output)