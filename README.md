# argocd-deploy

Declarative [ArgoCD](https://argoproj.github.io/argo-cd/) manifest and applications to apply to Kubernetes cluster upon provisioning.  This is a starter/example we can use to implement CD to Kubernetes clusters we provision.

## Usage:

1. Fork this repo.

2. Create a namespace for argocd, then bootstrap ArgoCD (v1.0.2) install using:

   ```bash
   kubectl create namespace argocd
   kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.0.2/manifests/install.yaml -n argocd
   ```

   (In openshift, you can replace `kubectl` with `oc`)

3. In your fork, update `repoURL` in `bootstrap/argocd-application.yaml`, and `targetRevision` to indicate which branch to watch (`master` in our case), which will have argocd sync up itself using this repo, then add the `Application` CustomResource to the cluster.

   ```bash
   kubectl apply -f bootstrap/argocd-application.yaml -n argocd
   ```

4. In your fork of the application, you can add additional applications for Argo to sync.  Argo recommends the [application of applications](https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/#application-of-applications-pattern) pattern which allows us to use this repo to add and remove apps in in other git repos.  
 
   We have included the example [liberty-hello-world-openshift-deploy](https://github.com/jkwong888/liberty-hello-world-openshift-deploy) application in the `bases` directory, which will synchronize `hello-world` and deploy it to an openshift cluster.  You can remove or modify this for your own application.  Any Kubernetes resources added to `bases` will be synchronized to the cluster where ArgoCD is deployed; we recommend adding additional `Application` resources where the actual definitions are in other git repositories that are updated at the end of CI pipelines.