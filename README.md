# Dokku Example Go 

Example of Deployment of Golang project via Dokku

## On server machine:

- Install dokku:
``` 
http://dokku.viewdocs.io/dokku/

```
- Go to browser on and put public ip of server. And add the public ssh key.

- Create dokku project
```
MYPROJECT="dokku-example" // same name must be specified on Procfile
PUBLIC_IP="172.12.12.111" // server IP
dokku apps:create $MYPROJECT
dokku config:set $MYPROJECT TZ="America/Sao_Paulo" // change timezone
dokku config:set $MYPROJECT PLATFORM=$MYPROJECT // not necessary

``` 

- Set .env variables // api keys etc
```
    dokku config:set $MYPROJECT MYTOKEN="23dfgd423424dfgdfgdfg2344" MYTOKEN2="22n4i234283uhg844"
```

## Locally:

- Project must have "go.mod" file.
- And "Procfile" with same dokku project name as specified on server machine: 

- Project folder must be inside: 
$GOPATH/src/

- Install dependencies
```
# Init go.mod
GO111MODULE=on go mod init

# Download dependencies
GO111MODULE=on go mod download

# Create vendor folder
GO111MODULE=on go mod vendor

```
- Add public key to remote machine:
``` 

cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/mypemfile.pem ubuntu@$PUBLIC_IP "sudo sshcommand acl-add dokku dokku"

```
- Add dokku remote to project:
```
git remote add dokku dokku@$PUBLIC_IP:$MYPROJECT
```

## Work on changes
- Commit and push to dokku remote:
```
git push dokku master
```
## Check log if build was successful
## App will be running by default on port 80 of the server public IP:
```
    {publicip}/hello/:yourname}
```


### After app is running. Useful commands:
- Check logs for that project
```
    dokku logs $MYPROJECT --tail
```

- Check project configs
```
    dokku config $MYPROJECT
```

- Check all apps running on Dokku
```
    dokku ps:report
```

- Restart app
```
    dokku ps:restart $MYPROJECT
```

