# OpenShift Serverless with Router Sharding

## Setup
```bash
# Setup two router shards with custom domains
oc apply -f yaml/router-shards.yaml

# Install serverless operator
oc apply -f yaml/serverless-operator.yaml

# Install KnativeServing with custom domains
oc apply -f yaml/knative-serving.yaml
```

## Testing

```bash
# Create a Knative Service for the dev shard
oc apply -f yaml/ksvc-dev.yaml
```



