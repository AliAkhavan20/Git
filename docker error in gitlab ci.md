# Docker error running gitlab ci
## if you had trouble running gitlab runner you might want to consider below command

> pat attention to the specific error:
> dial tcp: lookup docker on 8.8.8.8:53: no such host <ERROR>

### volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
### Adding docker.sock volume to config.toml appears to work.  Dont think this is correct approach though more of a workaround
#### open config.toml first
```
nano /etc/gitlab-runner/config.toml
```
#### edit this line : 
```
privileged = true
```