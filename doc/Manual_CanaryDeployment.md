# Step-by-Step Guide to Implement Canary Deployment with Nginx

### Step 1: Define the Upstream Servers
First, define the upstream servers for both the old and new versions of your application.

```bash
http {
    upstream old_version {
        server 192.168.1.10:80;
    }

    upstream new_version {
        server 192.168.1.20:80;
    }

    ...
}
```
Here, 192.168.1.10 is the IP of the old version, and 192.168.1.20 is the IP of the new version.

### Step 2: Create a Split Traffic Configuration

You can use the split_clients directive to control the percentage of traffic routed to each version.

```bash
http {
    upstream old_version {
        server 192.168.1.10:80;
    }

    upstream new_version {
        server 192.168.1.20:80;
    }

    # Splitting traffic: 90% to old version, 10% to new version
    split_clients "${remote_addr}${uri}" $canary_bucket {
        90%     old_version;
        10%     new_version;
    }

    server {
        listen 80;
        server_name yourdomain.com;

        location / {
            proxy_pass http://$canary_bucket;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```
- (split_clients) directive uses a variable (like IP address and URI) to deterministically route traffic based on the specified percentages.
- ($canary_bucket) is dynamically set based on the split, deciding whether to use the `old_version` or `new_version`.

### Step 3: Gradually Increase Traffic to the New Version

To increase the percentage of traffic going to the new version, simply adjust the percentages in the `split_clients` directive.

## For example:

. 80% to old, 20% to new:
```bash
split_clients "${remote_addr}${uri}" $canary_bucket {
    80%     old_version;
    20%     new_version;
}
```

. 50% to old, 50% to new:

```bash
split_clients "${remote_addr}${uri}" $canary_bucket {
    50%     old_version;
    50%     new_version;
}
```
### Step 4: Finalize the Deployment
Once you are confident that the new version is stable, you can route all traffic to the new version:

```bash
split_clients "${remote_addr}${uri}" $canary_bucket {
    100%     new_version;
}
```
Or simply remove the `split_clients` directive and directly proxy to the new version.
```bash
location / {
    proxy_pass http://new_version;
    ...
}
```

## Additional Considerations
### 1-Health Checks:
Ensure you have health checks in place to monitor the status of both versions.

### 2-Logging and Monitoring: 
Use logs and monitoring tools to keep track of the traffic distribution and application performance.

### 3-Rollback:
Have a rollback plan ready in case the new version shows issues. This would involve changing the split_clients directive back to route all traffic to the old version.

This setup allows for a controlled, gradual release of new versions of your application, minimizing risks associated with deployments.














