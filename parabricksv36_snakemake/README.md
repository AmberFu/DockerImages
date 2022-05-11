# How to pull docker images from gcr

Because when I try to follow [RIS Doc - Parabricks Quickstart](https://docs.ris.wustl.edu/doc/compute/recipes/tools/parabricks-quickstart.html) to PULL docker image from `gcr.io`, it shows the ERROR as below:

```
➜  parabricksv36_snakemake docker pull gcr.io/ris-registry-shared/parabricks:latest
Error response from daemon: Get "https://gcr.io/v2/": proxyconnect tcp: dial tcp 192.168.65.1:3128: connect: network is unreachable
```

### Resources:

* WUSTL RIS maintain Docker URL: https://console.cloud.google.com/gcr/images/ris-registry-shared/global/parabricks?cloudshell=true
* Web tutorial: [How to a pull Docker Image from GCR in any non-GCP Kubernetes cluster, 2019](https://medium.com/hackernoon/today-i-learned-pull-docker-image-from-gcr-google-container-registry-in-any-non-gcp-kubernetes-5f8298f28969)

### Steps:

Follow https://cloud.google.com/container-registry/docs/advanced-authentication

1. Install `docker`
2. Install `gcloud CLI` on local mechine: https://cloud.google.com/sdk/docs/install-sdk
3. To initialize the gcloud CLI, run `gcloud init` on local terminal
4. Configure authentication - configure Docker to use the Google Cloud CLI to authenticate requests to Container Registry.

    ```
    ➜  Documents gcloud auth configure-docker
    WARNING: Your config file at [/Users/pfu/.docker/config.json] contains these credential helper entries:

    {
      "credHelpers": {
        "asia.gcr.io": "gcloud",
        "eu.gcr.io": "gcloud",
        "gcr.io": "gcloud",
        "marketplace.gcr.io": "gcloud",
        "staging-k8s.gcr.io": "gcloud",
        "us.gcr.io": "gcloud"
      }
    }
    Adding credentials for all GCR repositories.
    WARNING: A long list of credential helpers may cause delays running 'docker build'. We recommend passing the registry name to configure only the registry you are using.
    gcloud credential helpers already registered correctly.
    ```
5. Enabling **Container Registry** service

    Enable the API from Cloud console or with the following gcloud command:
    
    ```
    gcloud services enable containerregistry.googleapis.com
    ```

6. Create Credentials for GCR: 

    * Go to the GCP Console. Select “API & Services” > “Credentials”
    ![Credentials Services](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.20.06%20PM.png)
    
    * From “Credentials” > click “Create credentials” > select “Services Account”
    ![Select Services Account](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.20.25%20PM.png)
    
    * And then, fill the service account detail
    ![service account detail](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.22.35%20PM.png)

    * For the Role, select the "Viewer"
    ![Viewer](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.23.27%20PM.png)

    * click DONE
    ![DONE](https://github.com/AmberFu/DockerImages/blob/main/parabricksv36_snakemake/Screen%20Shot%202022-05-10%20at%205.23.50%20PM.png)




```
➜  parabricksv36_snakemake gcloud auth activate-service-account pull-images-from-gcr@nvidia-parabricks-pipelines-3.iam.gserviceaccount.com --key-file=/Users/pfu/Documents/nvidia-parabricks-pipelines-3-455fe3966c39.json
Activated service account credentials for: [pull-images-from-gcr@nvidia-parabricks-pipelines-3.iam.gserviceaccount.com]


➜  parabricksv36_snakemake cat ~/Documents/nvidia-parabricks-pipelines-3-455fe3966c39.json | docker login -u _json_key --password-stdin https://gcr.io
Login Succeeded

Logging in with your password grants your terminal complete access to your account. 
For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/


➜  parabricksv36_snakemake docker pull gcr.io/ris-registry-shared/parabricks:latest
latest: Pulling from ris-registry-shared/parabricks
...

// Successfully downloaded:
➜  parabricksv36_snakemake docker images
REPOSITORY                              TAG       IMAGE ID       CREATED        SIZE
gcr.io/ris-registry-shared/parabricks   latest    832ca21c5d6f   4 months ago   18GB

```
