# kraster

A daemon that runs on a Kubernetes cluster to restart all tagged deployments on a schedule using `kubectl rollout restart`. 

Kubernetes adds an annotation which details this at `deployment.spec.metadata.annotations:kubectl.kubernetes.io/restartedAt: 2020-03-09T10:59:27Z`

## Installation

Download the files to your local machine/build agent.

### Configuration

Edit the crontab schedule in `kraster.yml` using your favourite editor

### Applying Resources

Run `kubectl apply -f ./kraster.yml`

## Use

The crontab ensures the job detailed in `kraster.yml` runs on a given schedule, no interaction is required.

## Debugging

### Jobs

`k describe job -n kraster`

### Target Deployment Status

You can validate the last restart time of deployments with:

`kubectl get deploy --all-namespaces -l=restartEnabled=true -o json | jq " .items[] | {name: .metadata.name, lastRestart: .spec.template.metadata.annotations.\"kubectl.kubernetes.io/restartedAt\"}"`
