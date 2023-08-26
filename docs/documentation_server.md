# Documentation hosting

### requirements

Install docker and docker compose as instructed on the docker website

### Running the documentation server
To run a docker container with this documentation use the following command:

`sudo docker run --name=dev_docs -d -p 8000:8000 -v ${PWD}:/docs titom73/mkdocs`

*note that this image only supports amd64 architecture*