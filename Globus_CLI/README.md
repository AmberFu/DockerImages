# The Globus Command Line Interface tool

Globus is a non-profit service for secure, reliable research data management.

#### Globus: https://www.globus.org

#### My dockerhub link: https://hub.docker.com/repository/docker/spashleyfu/globus_cli_wustl

The way building Image

```
➜  WUSTL_GlobusCLI docker build -t spashleyfu/globus_cli_wustl:pigz .
[+] Building 48.1s (20/20) FINISHED                                                                                           
 => [internal] load build definition from Dockerfile                                                                     0.0s
 => => transferring dockerfile: 1.26kB                                                                                   0.0s
 => [internal] load .dockerignore                                                                                        0.0s
 => => transferring context: 2B                                                                                          0.0s
 => [internal] load metadata for docker.io/library/ubuntu:20.04                                                          0.7s
 => CACHED https://bootstrap.pypa.io/get-pip.py                                                                          0.0s
 => [ 1/15] FROM docker.io/library/ubuntu:20.04@sha256:115822d64890aae5cde3c1e85ace4cc97308bb1fd884dac62f4db0a16dbddb36  2.0s
 => => resolve docker.io/library/ubuntu:20.04@sha256:115822d64890aae5cde3c1e85ace4cc97308bb1fd884dac62f4db0a16dbddb36    0.0s
 => => sha256:115822d64890aae5cde3c1e85ace4cc97308bb1fd884dac62f4db0a16dbddb36 1.42kB / 1.42kB                           0.0s
 => => sha256:7b3e30a1f373b0621681f13b92feb928129c1c38977481ee788a793fcae64fb9 529B / 529B                               0.0s
 => => sha256:1a437e363abfa47bfe4b3f5906b7444d12346102d944ebddd537e47a62fc6f52 1.46kB / 1.46kB                           0.0s
 => => sha256:8e5c1b329fe39c318c0d49821b339fb94a215c5dc0a2898c8030b5a4d091bcba 28.57MB / 28.57MB                         0.7s
 => => extracting sha256:8e5c1b329fe39c318c0d49821b339fb94a215c5dc0a2898c8030b5a4d091bcba                                1.2s
 => [ 2/15] RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y     python3-pip     git     vim     30.3s
 => [ 3/15] WORKDIR /opt/                                                                                                0.0s
 => [ 4/15] ADD https://bootstrap.pypa.io/get-pip.py .                                                                   0.0s
 => [ 5/15] RUN python3 get-pip.py                                                                                       3.4s 
 => [ 6/15] RUN echo "GLOBUS_CLI_INSTALL_DIR=$(python3 -c 'import site; print(site.USER_BASE)')/bin"                     0.2s 
 => [ 7/15] RUN echo 'export PATH="'"$(python3 -c 'import site; print(site.USER_BASE)')/bin"':$PATH"' >> "$HOME/.bashrc  0.3s 
 => [ 8/15] RUN pip install --upgrade globus-cli                                                                         2.9s 
 => [ 9/15] RUN pip install virtualenv                                                                                   1.5s 
 => [10/15] RUN git clone https://github.com/globus/automation-examples                                                  0.6s 
 => [11/15] WORKDIR automation-examples                                                                                  0.0s 
 => [12/15] RUN chmod +x cleanup_cache.py cli-sync.sh globus_folder_sync.py share-data.sh share_data.py                  0.2s 
 => [13/15] RUN virtualenv venv                                                                                          0.8s 
 => [14/15] RUN . venv/bin/activate                                                                                      0.3s 
 => [15/15] RUN pip install -r requirements.txt                                                                          1.3s 
 => exporting to image                                                                                                   3.3s
 => => exporting layers                                                                                                  3.3s
 => => writing image sha256:b8f1b050a1b321125b9400989644ab3374eb97c928c918b1ba770ec6feedb58a                             0.0s 
 => => naming to docker.io/spashleyfu/globus_cli_wustl:pigz                                                              0.0s 
                                                                                                                              
Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them                         
```

Vulnerabilities check:

```
➜  WUSTL_GlobusCLI docker scan spashleyfu/globus_cli_wustl:pigz

Testing spashleyfu/globus_cli_wustl:pigz...

...

Package manager:   deb
Project name:      docker-image|spashleyfu/globus_cli_wustl
Docker image:      spashleyfu/globus_cli_wustl:pigz
Platform:          linux/amd64
Base image:        ubuntu:20.04

Tested 253 dependencies for known vulnerabilities, found 41 vulnerabilities.

Base Image    Vulnerabilities  Severity
ubuntu:20.04  14               0 critical, 0 high, 2 medium, 12 low

Recommendations for base image upgrade:

Major upgrades
Base Image     Vulnerabilities  Severity
ubuntu:latest  9                0 critical, 0 high, 2 medium, 7 low


For more free scans that keep your images secure, sign up to Snyk at https://dockr.ly/3ePqVcp

```
