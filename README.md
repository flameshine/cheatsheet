# Cheatsheet

<h2>Table of contents</h2>

* [Utilities](#Utilities)
* [Unix](#Unix)
* [Helm](#Helm)
* [Kubernetes](#Kubernetes)
* [Docker](#Docker)
* [AWS](#AWS)
  * [ECR](#ECR)
  * [EKS](#EKS)
  * [S3](#S3)
  * [Secrets](#Secrets)
* [Redis](#Redis)
* [argo-workflows](#argo-workflows)
* [Git](#Git)
* [SVN](#SVN)
* [git-crypt](#git-crypt)
* [Gradle](#Gradle)
* [Maven](#Maven)
* [Python](#Python)

<h2>Utilities</h2>

UML Diagram Builder: https://app.diagrams.net

AWS Policy Generator: https://awspolicygen.s3.amazonaws.com/policygen.html

AWS Pricing Calculator: https://calculator.aws

Unicode Converter: https://www.branah.com/unicode-converter

JSON Converter: https://jsontostring.com

Difference Checker: https://www.diffchecker.com

Epoch Time Converter: https://www.epochconverter.com

Random Number Generator: https://www.random.org

TypeScript Playground: https://www.typescriptlang.org/play

RegEx Shaper: https://regexr.com

Character Remover: https://onlinecaseconvert.com/remove-characters-from-text-online.php

Case Converter: https://convertcase.net

Character Counter: https://charactercalculator.com

<h2>Unix</h2>

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
lsof -i -P | grep "LISTEN" | grep:<port>
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
LC_ALL=C < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c32; echo
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

Find file and replace all occurrences of a string inside:

```
find . -name "<pattern>" -print0 | xargs -0 sed -i "" 's/<old>/<new>/g'
```

Archive applying a passcode:

```
zip -er <archive-name> <archive-target>
```

Add space to the volume:

```
sudo vgdisplay rootvg

sudo growpart /dev/nvme0n1 3

sudo lvextend -L +2G /dev/mapper/rootvg-usrlv

sudo xfs_growfs /usr
```

<h2><a href="https://helm.sh/">Helm</a></h2>

Generate template:

```
helm template <path-to-chart> -f <values-to-pass> > ~/template.yaml
```

<h2><a href="https://kubernetes.io/">Kubernetes</a></h2>

List namespace events:

```
kc -n <namespace> get events
```

Enter a specific pod:

```
kc exec --stdin --tty <pod> -- /bin/bash
```

Execute a command inside a pod:

```
kc -n <namespace> exec <pod> <command>
```

Create an ordinary job from a cronjob:

```
kc create job --from=cronjob/<cronjob> <name>
```

Get resource YAML definition:

```
kc get <resource> <name> -o yaml
```

Forward pod's requests to a local port:

```
kc -n <namespace> port-forward <pod> 8080:80
```

View the exact yaml of some Kubernetes component:

```
kc get <resource> <name> -o yaml | less
```

Apply the corresponding action to each listed pod:

```
kc -n <namespace> get pods | awk '{print $1}' | grep "<pattern>" | xargs kubectl -n <namespace> <action> pod
```

Scale the deployment:

```
kc -n <namespace> scale deployment/<deployment> --replicas=<number of replicas>
```

<h2><a href="https://www.docker.com/">Docker</a></h2>

Remove all stopped containers, networks not used by at least one container, all images without at least one container and all build cache:

```
docker system prune -a
```

Build an image with tag:

```
docker build -t <tag> <path>
```

Remove the last container:

```
docker ps -a | awk '{print $1}' | sed -n 2p | xargs docker rm
```

<h2><a href="https://aws.amazon.com">AWS</a></h2>

<h3><a href="https://aws.amazon.com/en/ecr">ECR</a></h3>

Login into ECR:

```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ECR address>
```

<h3><a href="https://aws.amazon.com/en/eks">EKS</a></h3>

Update EKS cluster config:

```
aws eks update-kubeconfig --profile <profile> --region <region> --name <cluster-name>
```

<h3><a href="https://aws.amazon.com/en/s3">S3</a></h3>

Get the total number of objects in a bucket:

```
aws s3 ls s3://<bucket> --recursive --summarize | grep "Total Objects:"
```

<h3><a href="https://aws.amazon.com/en/secrets-manager">Secrets Manager</a></h3>

Delete a secret immediately:

```
aws secretsmanager delete-secret --secret-id <secret-id> --force-delete-without-recovery
```

<h2><a href="https://redis.io">Redis</a></h2>

Connect to the cluster:

```
redis6-cli -c -h <host> -p <port>
```

Get N items matching a pattern:

```
scan 0 match <pattern> count <count>
```

<h2><a href="https://argoproj.github.io/workflows">argo-workflows</a></h2>

Submit a template:

```
argo -n <namespace> template create <path>
```

Submit a workflow:

```
argo -n <namespace> submit <path>
```

Delete a template:

```
argo -n <namespace> template delete template <name>
```

Delete a workflow:

```
argo -n <namespace> delete workflow <name>
```

<h2><a href="https://git-scm.com">Git</a></h2>

Reset the branch:

```
git reset --hard origin
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

Revert particular file:

```
git checkout HEAD^ <path>
```

Cherry-pick commit from another repository:

```
git remote add other <link>

git fetch other

git cherry-pick <hash>

git remote remove other
```

Squash certain commit:

```
git rebase -i master

pick -> squash (for the commits you're interested in)
```

Split a certain commit:

```
git rebase -i <commit>

pick -> edit (for the commits you're interested in)

git reset HEAD~

git rebase --continue
```

Save changes from a range of commits for a single file into a patch:

```
git diff HEAD~3 HEAD~1 -- path/to/file.ext > changes.patch
```

<h2><a href="https://subversion.apache.org">SVN</a></h2>

Get the latest revision before the new branch was created:

```
svn log --stop-on-copy --verbose --limit 1 -r0:HEAD <branch-link>
```

Define a format a file:

```
svn propset svn:mime-type <format> <path>
```

<h2><a href="https://github.com/AGWA/git-crypt">git-crypt</a></h2>

Instructions: https://github.com/AGWA/git-crypt/tree/master

Lock repository:

```
git-crypt lock
```

Unlock repository:

```
git-crypt unlock <key path>
```

<h2>Gradle</h2>

Build refreshing dependencies:

```
./gradlew build --refresh-dependencies
```

<h2>Maven</h2>

Clean, build and refresh all dependencies:

```
mvn clean install -U
```

Display dependency tree:

```
mvn dependency:tree
```

<h2><a href="https://www.python.org">Python</a></h2>

Start an HTTP server:

```
python3 -m http.server <port>
```
