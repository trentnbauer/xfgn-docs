# ❔ Deploy new Container Stack

## Required Knowledge

* How to create Docker Compose files
* [authentication-access-and-accounts.md](../policies/authentication-access-and-accounts.md "mention")
* [creation-and-managment-of-servers-or-services.md](../policies/creation-and-managment-of-servers-or-services.md "mention")
* [external-access-to-systems.md](../policies/external-access-to-systems.md "mention")
* [monitoring-and-alerting.md](../policies/monitoring-and-alerting.md "mention")
* [management-of-documentation.md](../policies/management-of-documentation.md "mention")

## Process

### Create Compose file in Github



### Deploy Stack

#### What server?

Pick a server that is relevant to the usecase. If the app relates to Plex, the container should be installed on the same host as the Plex server.

If the app relates to Pterodactyl, it should be installed on the same server as either the panel or wings.

#### Deploy Stack



### ENV file required?

If an ENV file is required,

1. SSH into [Portainer host](../service-overviews/portainer-and-gitops.md#portainer-1)
2.  Run the below command to CD into the Portainer files

    ```bash
    cd /var/snap/docker/common/var-lib-docker/volumes/portainer/_data/env
    ```
3.  Run the below command (correct the file name) to create a new ENV file,

    ```bash
    nano myapp.env
    ```
4. Paste the contents of the ENV file and save it
5.  Update the Docker Compose file to reference the env file per below

    ```yaml
        env_file:
          - $ENV
    ```
6.  Update the stack and set the ENV variable as below (correct the file name)

    <pre><code><strong>/data/env/myapp.env
    </strong></code></pre>
7. Save and stop the stack
8. Delete and related docker volumes
9. Pull and deploy the stack
