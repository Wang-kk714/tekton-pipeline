## Setup

1. Install task git-clone
```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.6/git-clone.yaml
```

2. Install task kaniko
```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/kaniko/0.6/kaniko.yaml
```

3. Prepare your source repo, image registry and helm chart repo. Edit these on 'pipelinerun.yaml'.

- [Helm-Repo Github Example](https://helm.sh/docs/topics/chart_repository/#github-pages-example)

4. Genarate your secret for git, image & chart-repo(for me still git) authentication.
```
kubectl apply -f secret
```

5. Install local task: python-build, helm-chart-publish.
```
kubctl apply -f task
```

6. Install ci-pipeline and create pipelinerun.

```
kubectl apply -f pipeline.yaml
kubectl create -f pipelinerun.yaml
```

7. Use tekton-cli/tekton-dashboard inspect logs.

- [tkn cli](https://tekton.dev/docs/cli/)
- [tekton-dashboard](https://tekton.dev/docs/dashboard/)

```
tkn pipelinerun logs -f
```

## TODO

https://github.com/tektoncd/pipeline/pull/5536
PodSecurityPolicy remove in Kubernetes v1.25.0, so wait for tekton new release..
```
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
---output---
error: resource mapping not found for name: "tekton-triggers" namespace: "" from "https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml": no matches for kind "PodSecurityPolicy" in version "policy/v1beta1"
```
If works, use Eventlistener and Trigger to retrieve Github webhooks
