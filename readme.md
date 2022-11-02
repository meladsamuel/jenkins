# Jenkins on Kubernetes

## Install Helm

To install Helm CLI, follow the instructions from the [Installing Helm](https://helm.sh/docs/intro/install/) page.

## Configure Helm

```bash
helm repo add jenkinsci https://charts.jenkins.io
helm repo update
```

The helm charts in the Jenkins repo can be listed with the command:

```bash
helm search repo jenkinsci
```

## Create a persistent volume

```bash
kubectl apply -f jenkins-volume.yaml
```

## Create a service account

```bash
kubectl apply -f jenkins-sa.yaml
```

## Install Jenkins

We will deploy Jenkins including the Jenkins Kubernetes plugin. See the [official chart](https://github.com/jenkinsci/helm-charts/tree/main/charts/jenkins) for more details.

```bash
chart=jenkinsci/jenkins
helm install jenkins -n jenkins -f jenkins-values.yaml $chart
```

1. Get your 'admin' user password by running:

    ```bash
    kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
    ```

2. Get the Jenkins URL to visit by running these commands in the same shell:

    ```bash
    echo http://127.0.0.1:8080
    kubectl --namespace jenkins port-forward svc/jenkins 8080:8080
    ```

3. Login with the password from step 1 and the username: admin

4. Configure security realm and authorization strategy

5. Use Jenkins Configuration as Code by specifying configScripts in your values.yaml file, see documentation: http:///configuration-as-code and examples: https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos

