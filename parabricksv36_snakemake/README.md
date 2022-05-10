# How to pull docker images from gcr

Follow https://cloud.google.com/container-registry/docs/advanced-authentication

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

```
