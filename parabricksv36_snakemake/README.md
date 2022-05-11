# How to pull docker images from gcr

Because when I try to follow [RIS Doc - Parabricks Quickstart](https://docs.ris.wustl.edu/doc/compute/recipes/tools/parabricks-quickstart.html) to PULL docker image from `gcr.io`, it shows the ERROR as below:

```
➜  parabricksv36_snakemake docker pull gcr.io/ris-registry-shared/parabricks:latest
Error response from daemon: Get "https://gcr.io/v2/": proxyconnect tcp: dial tcp 192.168.65.1:3128: connect: network is unreachable
```

### Resources:

* WUSTL RIS maintain Docker URL: https://console.cloud.google.com/gcr/images/ris-registry-shared/global/parabricks?cloudshell=true
* Dockerhub: https://hub.docker.com/r/spashleyfu/parabricksv36_snakemakev76

### Steps:

Follow [How to a pull Docker Image from GCR in any non-GCP Kubernetes cluster, 2019](https://medium.com/hackernoon/today-i-learned-pull-docker-image-from-gcr-google-container-registry-in-any-non-gcp-kubernetes-5f8298f28969)

1. Install `docker`
2. Install `gcloud CLI` on local mechine: https://cloud.google.com/sdk/docs/install-sdk
3. To initialize the gcloud CLI, run `gcloud init` on local terminal
4. Create Credentials for GCR: 

    * Go to the GCP Console. Select “API & Services” > “Credentials”
    ![Credentials Services](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.20.06%20PM.png)
    
    * From “Credentials” > click “Create credentials” > select “Services Account”
    ![Select Services Account](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.20.25%20PM.png)
    
    * And then, fill the service account detail
    ![service account detail](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.22.35%20PM.png)

    * For the Role, select the "Viewer"
    ![Viewer](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.23.27%20PM.png)

    * Click DONE
    ![DONE](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.23.50%20PM.png)
    
    * Choose "Key" Tab to create a new key: it will automatically download "KEY-FILE"
    ![Create a new key](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-11%20at%2011.56.06%20AM.png)

5. Follow GCP Doc - Authentication methods: https://cloud.google.com/container-registry/docs/advanced-authentication

    * [Using `gcloud` credential helper](gcloud credential helper)
    ```
    // Make sure Login first: 
    $ gcloud auth login
    
    // View existing service accounts: Get "ACCOUNT"
    $ gcloud iam service-accounts list
    
    // Configure authentication with service account credentials:
    $ gcloud auth activate-service-account {ACCOUNT} --key-file={KEY-FILE}
    
    // Configure Docker with the following command:
    $ gcloud auth configure-docker
    ```

    * Then, follow [JSON key file](https://cloud.google.com/container-registry/docs/advanced-authentication#json-key) section
    
    > Use the service account key as your password to authenticate with Docker
    > 
    
    ```
    // Authenticate Docker
    $ cat keyfile.json | docker login -u _json_key --password-stdin https://gcr.io
    ```

> Actual Running LOGs (see ~/note.md on local Mac)
