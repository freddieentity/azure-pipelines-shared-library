```sh
helm repo add universal-helm-charts https://freddieentity.github.io/universal-helm-charts
helm dependency update ./deploy/helm
helm search repo universal-helm-charts

yq -i '.microservices-standard.image.repository = "0908887875/weatherservice"' ./deploy/helm/values.yaml  
yq -i '.microservices-standard.image.tag = "1.0.0"' ./deploy/helm/values.yaml  
yq -i '.microservices-standard.fullnameOverride = "weatherservice"' ./deploy/helm/values.yaml  
yq -i '.name = "weatherservice"' ./deploy/helm/Chart.yaml # Override chart name for specific microservices 

helm package ./deploy/helm --version 1.0.0 --app-version 1.0.0 --destination ./deploy/helm
helm upgrade --install weatherservice --dry-run --debug ./deploy/helm/weatherservice-1.0.0.tgz  
helm push ./deploy/helm/weatherservice-1.0.0.tgz oci://freddieentity.azurecr.io/helm 
```