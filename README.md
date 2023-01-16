# Cheatsheet

<h2>Table of contents</h2>

* [Utilities](#Utilities)
* [Unix](#Unix)
* [Helm](#Helm)
* [Kubernetes](#Kubernetes)
* [Docker](#Docker)
* [argo-workflows](#argo-workflows)
* [AWS](#AWS)
* [Git](#Git)
* [SVN](#SVN)

<h2>Utilities</h2>

UML diagram builder: https://app.diagrams.net

AWS policy generator: https://awspolicygen.s3.amazonaws.com/policygen.html

Unicode converter: https://www.branah.com/unicode-converter

JSON converter: https://jsontostring.com

Difference checker: https://www.diffchecker.com

Character remover: https://onlinecaseconvert.com/remove-characters-from-text-online.php

Case converter: https://convertcase.net

Epoch time converter: https://www.epochconverter.com

<h2>Unix</h2>

Grant read/write/execute permissions to a file:

```
sudo chmod 775 <target>
```

Change group permission of a file:

```
sudo chown <user>:contractors <target>
```

Kill a process on particular port:

```
sudo kill -9 $(lsof -t -i:<port>)
```

Check what is running on the port:

```
netstat -tulpn | grep <port>
```

Get SSL certificate data:

```
openssl crl2pkcs7 -nocrl -certfile <name> | openssl pkcs7 -print_certs -noout
```

Grep all '.gz' files in directory:

```
find . -name \*.gz -print0 | xargs -0 zgrep "<pattern>"
```

Generate a random string:

```
< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c32
```

Encode a string with Base64:

```
openssl base64 -e <<< "<string>"
```

Decode a string with Base64:

```
openssl base64 -d <<< "<string>"
```

Get your IP address:

```
ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'
```

Inspect HTTP requests to a particular port:

```
nc -l <port>
```

Grab nth line of console output:

```
sed -n 2p
```

<h2>Helm</h2>

Update dependencies:

```
helm dependency update
```

Build dependencies:

```
helm dependency build
```

Generate template:

```
helm template <path-to-chart> -f <values-to-pass> > ~/template.yaml
```

<h2>Kubernetes</h2>

Connect to the cluster:

```
kcon <cluster>
```

Connect to a certain namespace of the cluster:

```
kcon <cluster> <namespace>
```

List resource types:

```
kc get <resource type>
```

Get a specific resource type:

```
kc get <resource type> <name>
```

Delete a resource type:

```
kc delete <resource type> <name>
```

Describe a resource type:

```
kc describe <resource type> <name>
```

Apply a manifest:

```
kc apply -f <path>
```

Enter a specific pod:

```
kc exec --stdin --tty <pod> -- /bin/bash
```


Execute any command (for instance, view file content) inside a pod:

```
kc -n <namespace> exec <pod> -- cat <path>
```

Create an ordinary job from a cronjob (server & client Kubernetes versions should be the same):

```
kc create job --from=cronjob/<cronjob> <name>
```

Check pod's logs:

```
kc logs <pod>
```

Get resource YAML definition:

```
kc get <resource> <name> -o yaml
```

Forward pod's requests to a local port:

```
kc -n <namespace> port-forward <pod> 8080:80
```

Example of selecting Prometheus pods and forwarding their traffic to localhost:

```
kc -n prometheus port-forward $(kc -n prometheus get pods --selector=app=prometheus,component=server --no-headers -o custom-columns=":metadata.name") 9090:9090
```

View the exact yaml of some Kubernetes component:

```
kc get <resource> <name> -o yaml | less
```

Apply the corresponding action to each listed pod:

```
kc -n <namespace> get pods | awk '{print $1}' | grep "<pattern>" | xargs kubectl -n <namespace> <action> pod
```

<h2>AWS</h2>

Synthetize CDK changes:

```
aws2-wrap --profile <profile> cdk synth
```

Deploy CDK changes:

```
aws2-wrap --profile <profile> cdk deploy
```

Destroy CDK changes:

```
aws2-wrap --profile <profile> cdk destroy
```

Login into ECR:

```
aws2-wrap --profile <profile> aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ECR address>
```

Update EKS cluster config:

```
aws eks update-kubeconfig --profile <profile> --region us-east-1 --name <cluster-name>
```

<h2>Docker</h2>

Remove all stopped containers, networks not used by at least one container, all images without at least one container and all build cache:

```
docker system prune -a
```

List all images:

```
docker images
```

Remove an image:

```
docker image rm <name/id>
```

List all containers:

```
docker ps -a
```

Remove a container:

```
docker container rm <name/id>
```

Build an image:

```
docker build (-t <tag>) <path>
```

Push an image:

```
docker push <name/id>
```

Remove the last container:

```
docker ps -a | awk '{print $1}' | sed -n 2p | xargs docker rm
```

<h2>argo-workflows</h2>

Submit a template:

```
argo -n <namespace> template create <path>
```

Submit a workflow:

```
argo -n <namespace> submit <path>
```

<h2>Git</h2>

Remove all unpushed commits:

```
git reset --hard origin
```

Reword last commit message:

```
git commit --amend -m "<message>"
```

Squash all commits on a branch:

```
git checkout <branch>

git reset $(git merge-base master $(git branch --show-current))

git add -A

git commit -m <message>

git push --force
```

Move unpushed commit from one branch to another:

```
git reset HEAD~1

git stash

git checkout <branch>

git stash pop
```

Make a certain branch same as master:

```
git reset --hard master
```

<h2>SVN</h2>

Get latest revision before the new branch was created:

```
svn log --stop-on-copy --verbose --limit 1 -r0:HEAD <branch-link>
```

Define a format a file:

```
svn propset svn:mime-type <format> <path>
```
