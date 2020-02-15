# container-security

Examples of opensource software to quickly scan Dockerfiles and/or docker images in a way that can easily be added to a build pipeline.

# HADOLINT

https://github.com/hadolint/hadolint

```
docker run --rm -i hadolint/hadolint < Dockerfile
```


# TRIVY

https://github.com/aquasecurity/trivy

Scan an accessable image:

```
docker run --rm aquasec/trivy node:alpine
```

Scan an image file:

```
save drew:latest -o drewfoo.tar

docker run -it \
-v $(pwd)/drewfoo.tar:/tmp/drewfoo.tar \
aquasec/trivy --input /tmp/drewfoo.tar 
```

Scan an image without the scanner needing internet access 

```
docker run -d  -p 8080:8080 aquasec/trivy \
server --listen 0.0.0.0:8080

docker run -it \
-v $(pwd)/drewfoo.tar:/tmp/drewfoo.tar aquasec/trivy \
client --remote http://localhost:8080 --input /tmp/drewfoo.tar 
```
Note: You'll need to make the server properly accessable to the client, this example won't work without modification to suit your network.

# DOCKLE

https://github.com/goodwithtech/dockle

```
export DOCKLE_LATEST=$(
 curl --silent "https://api.github.com/repos/goodwithtech/dockle/releases/latest" | \
 grep '"tag_name":' | \
 sed -E 's/.*"v([^"]+)".*/\1/' \
)

docker run --rm \
-v $(pwd)/drewfoo.tar:/tmp/drewfoo.tar \
goodwithtech/dockle:v${DOCKLE_LATEST} --input /tmp/drewfoo.tar 
```
