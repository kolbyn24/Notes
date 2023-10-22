---
creation date: October 22nd 2023
last modified date: October 22nd 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[cloud]]  
___

## Description:  

### Azure

Azurehound

Enumerate for Priv Esc:

```shell
# Login
$ az login -u <user> -p <password>

# Set Account Subscription
$ az account set --subscription "Pay-As-You-Go"

# Enumeration for Priv Esc
$ az ad user list -o table
$ az role assignment list -o table
```

### AWS
Shodan.io query to enumerate AWS Instance Metadata Service Access

```
/latest/meta-data/iam/security-credentials
```

Google Dorking for AWS Access Keys

```
inurl:pastebin "AWS_ACCESS_KEY"
```

Recursively searching for AWS Access Keys on *Nix containers

```shell
$ grep -ER "AKIA[A-Z0-9]{16}|ASIA[A-Z0-9]{16}" /
```

S3 Log Google Dorking

```
s3 site:amazonaws.com filetype:log
```

Python code to check if AWS key has permissions to read s3 buckets:

```python
import boto3
import json

aws_access_key_id = 'AKIA2OGYBAH6S4M5DDZO'
aws_secret_access_key = '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'
region = 'us-east-2'

session = boto3.Session(
    aws_access_key_id=aws_access_key_id,
    aws_secret_access_key=aws_secret_access_key,
    region_name=region
)

s3 = session.resource('s3')

try:
    response = []
    for bucket in s3.buckets.all():
        response.append(bucket.name)
    print(json.dumps(response))
except Exception as e:
    print(f"Error: {e}")
```

### Kubernetes Secrets Harvesting

```shell
$ curl -k -v -H “Authorization: Bearer <jwt_token>” -H “Content-Type: application/json” https://<master_ip>:6443/api/v1/namespaces/default/secrets | jq -r ‘.items[].data’
```

### Kubernetes Ninja Commands

```shell
# List all pods in the current namespace.
kubectl get pods

# Get detailed information about a pod.
kubectl describe pod <pod-name>

# Create a new pod.
kubectl create pod <pod-name> 

# List all nodes in the cluster.
kubectl get nodes 

# Get detailed information about a node.
kubectl describe node <node-name> 

# Create a new node
kubectl create node <node-name> 

# List all services in the cluster.
kubectl get services 

# Get detailed information about a service.
kubectl describe service <service-name> 

# Create a new service.
kubectl create service <service-name> 

# List all secrets in the cluster.
kubectl get secrets 

# Get detailed information about a secret.
kubectl describe secret <secret-name> 

# Create a new secret.
kubectl create secret <secret-name> 
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: October 22nd 2023 (01:08 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
