# Step 6 - Hosting the CDN

In this step we will provision hosting of CDN under the client's domain.

1. All the below operations are to be performed under the terraform organization created in the step 2.
2. Setting up the workspace
    - Create a workspace with name `cdn` prefixed with the organization's environment type. For e.g. if env is `prod` then the workspace name will be `prod-cdn`.
    - Choose `/dev/cdn` or `/prod/cdn` in the `terraform` github repository for version control with default branch pointing to `master`.
    - Configure the following variables for the workspace
        1. `cdn_domains` - (HCL) client's domain on which CDN is hosted. Replace `<domain>` with the client's domain. if domain is `cdn.goldilocks.com` then the variable will be
        ```
        {
            "goldilocks.com" = {
              endpoint            = "cdn.goldilocks.com"
              dns_map_ack_success = false
            }
        }
        ```
        2. `sdk_version` - version of SDK to host
3. Attach this workspace to the respective variable set of the organization created in Step 2.
4. Run the pipeline.
    - Click on `Actions` and then `Start new run` to start a new run.
    - Below variables are present in the output
        1. `cdn_domains_mappings` - DNS authentication for CDN domains
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
        2. `cdn_cloudfront_mappings` - DNS authentication to SDK domains. For e.g.
        ```
        [
          {
            "cloudfront_endpoint": "xxx.cloudfront.net",
            "domain ": "<domain>",
            "type": "CNAME"
          }
        ]
        ```

5. The above pipeline creates a SSL/TLS certificate under the client's domain. This certificate needs to be verified via the DNS strategy.
    - Client needs to create a CNAME record with the above `record_name` and `record_value` given in the variable output `cdn_domains_mapping` as name and target respectively.
    - Proxy status for the CNAME record should be kept off.
    - Create another CNAME record for client's domain, where the name is `domain` and target is `cloudfront_endpoint` as represented in the `cdn_cloudfront_mappings`. This step points the client's domain to the cloudfront endpoint.
6. Edit `dns_map_ack_success` in `cdn_domains` to `true` once client has confirmed the DNS records are in place. Taking the above example - 
    ```
    {
        "goldilocks.com" = {
          endpoint            = "cdn.goldilocks.com"
          dns_map_ack_success = true
        }
    }
    ```
7. Click on `Actions` and then `Start new run` to start a new run.
8. To verify the changes, hit the `/index.js` endpoint over client's endpoint. This will give your SDK hosted on CDN.