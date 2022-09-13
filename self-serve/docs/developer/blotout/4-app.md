# Step 4 - Provision the application

In this step we will provision the application on the deployed hardware.

1. All the below operations are to be performed under the terraform organization created in the step 2.
2. Setting up the workspace
    - Create a workspace with name `app` prefixed with the organization's environment type. For e.g. if env is `prod` then the workspace name will be `prod-app`.
    - Choose `/dev/app` or `/prod/app` in the `terraform` github repository for version control with default branch pointing to `master`.
    - Configure the following variables for the workspace
        1. `administrator_email` - ui mail to be whitelisted.
        2. `airflow_job_start_date` - start date of the pipeline For e.g. if pipeline is created on 12th August 2022 the value of the variable will be `2022-08-12 12:00:00` 
        3. `failure_report_email` - email where failure reports are sent (Default: `alert@blotout.io`)
        4. `docker_username` - (sensitive) username for dockerhub 
        5. `docker_password` - (sensitive) password for dockerhub 
        6. `smtp_user` - (sensitive) user to authenticate to smtp server
        7. `smtp_password` - (sensitive) password to authenticate to smtp server
        8. `smtp_mail_from` - mail ID from which mail will be sent
        9. `git_user` - (sensitive) user to authenticate to github
        10. `git_token` - (sensitive) password to authenticate to github
        11. `git_repository_name` - github repo name for airflow (Default: `airflow-dags`)
        12. `git_repository_path` - github repo path in `git_repository_path` repo for airflow (Default: `dags`) 
3. Attach this workspace to the respective variable set of the organization created in Step 2.
4. Run the pipeline.
    - Click on `Actions` and then `Start new run` to start a new run.
    - Below variables are present in the output
        1. `web_application_url` - url for blotout UI
        2. `airflow_url` - url for airflow UI
5. Using the `administrator_email` we can now sign up in URL given by `web_application_url`.

# Links
1. [Terraform organization](../terraform/organization.md)
2. [Workspace in terraform organization](../terraform/workspace.md)
3. [Variable sets](../terraform/variable_sets.md)
4. [Workspace variables](../terraform/workspace_variables.md)
5. [Starting new run](../terraform/action.md#starting-new-run)
6. [Output](../terraform/action.md#output)
7. [Google SSO](../client/sso.md)
8. [Slack](../client/slack.md)