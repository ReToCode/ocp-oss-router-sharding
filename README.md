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
# Create a Knative Service for both shards
oc apply -f yaml/ksvc-dev.yaml
oc apply -f yaml/ksvc-prod.yaml
```

## Verification

```bash
# Check the routes of the Knative Services:
oc get ksvc -n default
NAME         URL                                                    LATESTCREATED      LATESTREADY        READY   REASON
hello        https://hello-default.dev.apps.sno.codemint.ch         hello-00001        hello-00001        True
hello-prod   https://hello-prod-default.prod.apps.sno.codemint.ch   hello-prod-00001   hello-prod-00001   True
```

```bash
# Check the assignment to the router shards
# (ignore the default router, it was not configured to ignore the shards and just claims every route)
oc get route -n knative-serving-ingress -o jsonpath='{range .items[*]}{@.metadata.name}{" "}{@.spec.host}{" "}{@.status.ingress[*].routerName}{"\n"}{end}'
route-19e6628b-77af-4da0-9b4c-1224934b2250-323461616533 hello-prod-default.prod.apps.sno.codemint.ch default ingress-prod
route-cb5085d9-b7da-4741-9a56-96c88c6adaaa-373065343266 hello-default.dev.apps.sno.codemint.ch default ingress-dev
```

