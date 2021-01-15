# Gitlab

## Gitlab Installation with docker-compose
Create required directory
```
# mkdir -p gitlab/{data,logs,config,gitlab-runner-config}
# touch gitlab/gitlab-runner-config/config.toml
# cd gitlab
```

Create docker-compose.yaml file
```
services:
  web:
    image: 'gitlab/gitlab-ce:13.7.3-ce.0'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
        gitlab_rails['gitlab_shell_ssh_port'] = 2222
        gitlab_rails['time_zone'] = 'Asia/Tehran'
        gitlab_rails['gitlab_email_from'] = 'gitlab-no-reply@example.com'
        gitlab_rails['gitlab_email_display_name'] = 'GitLab Administrator'
        gitlab_rails['gitlab_email_reply_to'] = 'admin@hexample.com'
        gitlab_rails['backup_keep_time'] = 15000000
        gitlab_rails['smtp_enable'] = true
        gitlab_rails['smtp_address'] = "smtp.example.com"
        gitlab_rails['smtp_port'] = 587
        gitlab_rails['smtp_user_name'] = "no-reply@example.com"
        gitlab_rails['smtp_password'] = "smptpasswordhere"
        gitlab_rails['smtp_domain'] = "example.com"
        gitlab_rails['smtp_authentication'] = "login"
        gitlab_rails['smtp_enable_starttls_auto'] = true
        unicorn['worker_timeout'] = 60
        unicorn['worker_processes'] = 3
        logging['logrotate_frequency'] = "weekly"
        logging['logrotate_rotate'] = 52
        logging['logrotate_compress'] = "compress"
        logging['logrotate_method'] = "copytruncate"
        logging['logrotate_delaycompress'] = "delaycompress"
        nginx['listen_port'] = 443
        nginx['redirect_http_to_https'] = true
        nginx['ssl_certificate'] = "/etc/gitlab/ssl/example.com.cert"
        nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/example.com.key"
        nginx['ssl_protocols'] = "TLSv1.1 TLSv1.2"
        nginx['logrotate_frequency'] = "weekly"
        nginx['logrotate_rotate'] = 52
        nginx['logrotate_compress'] = "compress"
        nginx['logrotate_method'] = "copytruncate"
        nginx['logrotate_delaycompress'] = "delaycompress"
        letsencrypt['enable'] = false
        # gitlab_rails['initial_root_password'] = File.read('/run/secrets/gitlab_root_password')
        # Add any other gitlab.rb configuration here, each on its own line
    ports:
      - '80:80'
      - '443:443'
      - '2222:22'
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - './data:/var/opt/gitlab'
  gitlab-runner:
    image: gitlab/gitlab-runner:alpine-v13.7.0
    volumes: 
      - "./gitlab-runner-config:/etc/gitlab-runner"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

Create selfsigned certificate
```
openssl req -x509 -new -days 365 -nodes -keyout example.com.key -out example.com.cert
```


