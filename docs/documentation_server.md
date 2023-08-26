# Docker documentation

## requirements

Install docker and docker compose as instructed on the docker website

##
To run a docker container with this documentation use the following command
`sudo docker run --name=dev_docs -d -p 8000:8000 -v ${PWD}:/docs titom73/mkdocs`