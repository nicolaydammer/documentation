# Documentation hosting

### requirements

* Docker
* Docker-Compose

### Running the documentation server

To run a docker container with this documentation use the following command:

`sudo docker run --name=dev_docs -d -p 8000:8000 -v ${PWD}:/docs titom73/mkdocs`

*note that this image only supports amd64 architecture*