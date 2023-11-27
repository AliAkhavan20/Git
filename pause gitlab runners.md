# wanna pause runners?

## Pause your runners or block new jobs from starting by adding following to your /etc/gitlab/gitlab.rb:
```
nginx['custom_gitlab_server_config'] = "location /api/v4/jobs/request {\n deny all;\n return 503;\n}\n"
```
And reconfigure GitLab with:
```
sudo gitlab-ctl reconfigure
```