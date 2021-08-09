# Universal Kubernetes Helm Charts

This is a set of standardized helm charts meant for installation of best-practice services on Kubernetes.  They support a wide range of usages but generally are to be used in combination with the rest of the DevOps Nirvana stack.  Feel free to adopt/adapt them and/or submit bug requests and/or fixes!

# Usage

```
# Add the repository with..
helm repo add devops-nirvana https://devops-nirvana.s3.amazonaws.com/helm-charts/
```

Then after this use the "deployment" helm chart within this repo with your own values file.  TODO add more docs here.

# TODO

* Add more info in this file to help others integrate with this 
* Add back the other charts with the latest best-practices (daemonset, statefulset)
* Add examples and documentation
* Add tests to ensure no breakage
* Add some backwards compatibility with older/newer versions (eg: API version dynamic based on version of k8s)
* Auto-publish updates of helm charts via Github Actions
* Profit...?  :P

# Contact

For information/questions either file an issue on Github or Email farley _at_ neonsurge **dot** com
