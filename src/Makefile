NAME   := imylomylo/emc2foundation-einsteinium-master
TAG    := $$(curl -s https://api.github.com/repos/emc2foundation/einsteinium/git/refs/heads/master | jq -r '.object.sha')
IMG    := ${NAME}:${TAG}
LATEST := ${NAME}:latest

build:
	docker build --no-cache -t ${IMG} .
	docker tag ${IMG} ${LATEST}

push:
	docker push ${NAME}

login:
	docker log -u ${DOCKER_USER} -p ${DOCKER_PASS}

run:
	docker run -it -e LOCAL_USER_ID=1000 --mount src=${PWD}/../node,target=/home/user/.einsteinium,type=bind ${NAME}

