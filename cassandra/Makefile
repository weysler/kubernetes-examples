# Copyright 2016 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# build the cassandra image.
VERSION=v15
PROJECT_ID?=exlogy
PROJECT=gcr.io/${PROJECT_ID}
CASSANDRA_VERSION=3.11.7

all: push

build-go:
	go build -a -installsuffix cgo \
		-ldflags "-s -w" \
		-o image/files/cassandra-seed.so -buildmode=c-shared go/main.go

kubernetes-cassandra.jar:
	@echo "Building kubernetes-cassandra.jar"
	docker run -v ${PWD}/java:/usr/src/app maven:3-jdk-8-onbuild-alpine mvn clean install
	cp java/target/kubernetes-cassandra*.jar image/files/kubernetes-cassandra.jar

container:
	@echo "Building ${PROJECT}/cassandra:${VERSION}"
	docker build --pull --build-arg "CASSANDRA_VERSION=${CASSANDRA_VERSION}" -t ${PROJECT}/cassandra:${VERSION} image

container-dev:
	docker build --pull --build-arg "CASSANDRA_VERSION=${CASSANDRA_VERSION}" --build-arg "DEV_CONTAINER=true" -t ${PROJECT}/cassandra:${VERSION}-dev image

build: container

push: build
	docker push ${PROJECT}/cassandra:${VERSION}

.PHONY: all build push
