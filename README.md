# hello-world-app

Basic `Python 3.8` Flask application to run in a Kubernetes cluster.

[![Linkedin](https://i.stack.imgur.com/gVE0j.png) LinkedIn](https://www.linkedin.com/in/fmdlc) [![GitHub](https://i.stack.imgur.com/tskMh.png) GitHub](https://github.com/fmdlc)

### Build

```bash
$: docker build -t tty0/flask_app:1.0
```

## How to run

You need to have all requirements installed.

### Local execution

```bash
$ cd src/
$ pip install -r requirements.txt
$ export FLASK_APP=server.py
$ export FLASK_RUN_PORT=8000
$ export FLASK_DEBUG=True
$ flask run --host=0.0.0.0
```

### Run in Docker (meinheld)

```bash
$ docker run -p 8080 -it tty0/flask_app
```

### Run in Kubernetes (Helm)
> Be sure to have Helm 3 installed: ([https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/)

1) Edit `values.yaml` file in order to setup your environment.

2) First test installation with `--dry-run` parameter. You can specify the namespace by using `-n` modificator or alter any input parameter with `--set=<PARAMETER:VALUE>`.

```bash
$ cd ./helm/ekoparty-flask-app
$ helm install ekoparty-flask-app . -n default
Release "ekoparty-flask-app" has been upgraded. Happy Helming!
NAME: ekoparty-flask-app
LAST DEPLOYED: Thu Sep  3 23:01:39 2020
NAMESPACE: wildwest
STATUS: deployed
REVISION: 4
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace wildwest -l "00app.kubernetes.io/name=ekoparty-flask-app,app.kubernetes.io/instance=ekoparty-flask-app" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace wildwest port-forward $POD_NAME 8080:8000
```

#### Values.yaml

```yaml
# Default values for ekoparty-flask-app.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 5

image:
  repository: tty0/flask_app
  pullPolicy: Always
  tag: "1.0"

service:
  type: ClusterIP
  port: 8000

limits:
  cpu: 0.2
  memory: 128Mi
requests:
  cpu: 0.4
  memory: 256Mi

autoscaling:
  enabled: true
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 80
  targetMemoryUtilizationPercentage: 80

conf:
  salute: "better ask for forgiveness than permission"
  header: "https://ekoparty.org/web/image/1309/badges.jpg"
```

 ## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Authors

Module managed by [Facu de la Cruz](mailto:fmdlc.unix@gmail.com)

## License

[Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)
