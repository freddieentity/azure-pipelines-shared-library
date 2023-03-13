```sh
# helm repo add universal-helm-charts https://freddieentity.github.io/universal-helm-charts
# helm dependency update ./deploy/helm
# helm search repo universal-helm-charts

# yq -i '.microservices-standard.image.repository = "freddieentity.azurecr.io"' ./deploy/helm/values.yaml  
# yq -i '.microservices-standard.image.tag = "1.0.0"' ./deploy/helm/values.yaml  
# yq -i '.microservices-standard.fullnameOverride = "testservice"' ./deploy/helm/values.yaml  
# yq -i '.name = "testservice"' ./deploy/helm/Chart.yaml # Override chart name for specific microservices 

# helm package ./deploy/helm --version 1.0.0 --app-version 1.0.0 --destination ./deploy/helm
# helm upgrade --install testservice --dry-run --debug ./deploy/helm/testservice-1.0.0.tgz  
# helm push ./deploy/helm/testservice-1.0.0.tgz oci://freddieentity.azurecr.io/helm 
```