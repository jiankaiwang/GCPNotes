# Introduction to Docker (GSP055)

keywords: `docker`

## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1029?parent=catalog

## Quick Notes

### Run a container.

* Run a container, `Hello world, Docker.`.

```sh
$ docker run hello-world
```

Docker can't find an image locally, it find and search the image from a public registry, the Dockerhub, and download it to the local registry. Second, start a container from the image.

* List all images locally.

```sh
$ docker images
```

* List all containers locally.

```sh
# -a: show all containers no matter what its state is
$ docker ps [-a]
```

* Run a container in advanced.

```sh
# docker run --name <container-name> <image-name>
$ docker run --name helloworld hello-world
```

### Build the image.

Next we are going to build an image which is a Node.js application.

```sh
mkdir ./test && cd ./test
```

Next we are going to edit a Dockerfile which is a template for building a Docker image.

```sh
cat > Dockerfile << EOF
# use the node version 6 image as the base image
From node:6

# setup the working directory at /app
WORKDIR /app

# add all content to the directory
ADD . /app

# expose the port number 80 to allow the connection via network
EXPOSE 80

# start the node app with a extry `app.js`
CMD ["node", "app.js"]

# end of file
EOF
```

After you create a Dockerfile, you now have to create the entry script `app.js`.

```sh
cat > app.js << EOF
# include the library for the http connection (it usually uses port 80)
const http = require('http');
const hostname = '0.0.0.0';
const port = 80;

# create a http server
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello world, Node.js on Docker.\n');
});

server.listen(port, hostname, () => {
  console.log("Server is running at http://%s:%s", hostname, port);
});

process.on('SIGINT', function() {
  console.log("Caught interrupt signal and will exit.");
  process.exit();
});

# end of file
EOF
```

Now you can build the image.

```sh
# `.` means you start to build the image from the current directory which contains 
# a Dockerfile

# command protype:
# -t: tag, <name>:<version>
$ docker build -t node:0.1 .
```

Show the images on the local registry.

```sh
$ docker images
```

### Run the built image

You can simply run the built image as the previous way.

```sh
# -p: port mapping <from-host>:<to-container>
# -d: run in the daemon
$ docker run -p 4000:80 --name nodeapp -d node:0.1
```

Try to access the node application.

```sh
$ curl http://localhost:4000
```

### Stop and remove the container

The following commands helps you stop and remove the container.

```sh
$ docker stop nodeapp
```

If you try to remove the container, use the following command.

```sh
$ docker rm nodeapp
```

### Debug the container

No matter what the container state is, in running or stopping, 
you can access the logs.

```sh
# -f: debugging on the running mode
$ docker logs [-f] nodeapp
```

You can access to the container in an interactive mode.

```sh
# -it: start a pseudo-tty in an interactive mode and keep stdin open
$ docker exec -it nodeapp /bin/bash
```

You can inspect the metadata of the container.

```sh
$ docker inspect nodeapp
```

You can format the metadata information in advanced.

```sh
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nodeapp
```

### Publish the image

After you buil the image, you can push it to the registry and clean your developing environment. By default you can push it to the public container registry `Dockerhub` or a customized one, for exmaple, google container registry, `gcr`.

If you are going to push the image to `GCR`, you have to format the image name with tag like the following.

```text
[hostname]/[project-id]/[image]:[tag]

for gcr:

hostname: gcr.io
project-id: <your-project-id>
image: <image-name>
tag: <tag>
```

```sh
# tag the image for pushing to the GCR
docker tag nodeapp:0.1 gcr.io/<project-id>/nodeapp:0.1

# push the image
docker push gcr.io/<project-id>/nodeapp:0.1
```

You can surf the website `https://gcr.io/<project-id>/nodeapp:0.1` to check the pushing. Remove the containers.

```sh
$ docker stop $(docker ps -q)
$ docker rm $(docker ps -aq)
```

### Remove the image

After you push the image to the registry, you can remove the image from the local registry.

```sh
# delete an image by an image
$ docker rmi nodeapp:0.1

# delete all images
$ docker rmi $(docker images -aq)
```

