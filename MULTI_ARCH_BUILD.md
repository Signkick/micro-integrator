# How To run a Multi Architecture Build for WSO2 MI

In root of the project, run
```
mvn clean install -DskipTests -Dmaven.test.skip=true -Ddocker.skip=false
```

Navigate to the output of the build specific to building the Docker image. The image is actually also already been built by the MVN plugin, but we want to build it ourselves using buildx:
```
cd distribution/target/docker

# Create a multi-arch builder
docker buildx create --name multiarch

#Use the new builder image
docker buildx use multiarch

# Initiate the new builder
docker buildx inspect --bootstrap

# Initiate the multi architecture build, and push to ECR
docker buildx build --platform "linux/amd64,linux/arm64" -f ../../src/docker-distribution/centos/Dockerfile --build-arg MICROESB_VERSION=4.1.0-SNAPSHOT --tag 010771964665.dkr.ecr.eu-west-1.amazonaws.com/signkick/micro-integrator:latest --push .
```