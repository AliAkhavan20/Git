# Add the gitlab-runner user to the docker group:
```
sudo usermod -aG docker gitlab-runner
```

## Verify that gitlab-runner has access to Docker:
```
sudo -u gitlab-runner -H docker info
```
