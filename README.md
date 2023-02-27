# Sample Azure Pipeline YAML structure
```yaml
name: $(BuildID)

resources:
  repositories: # commit repo trigger pipeline
    - repository: "repoName"
      type: git
      name: "project/repo"
      ref: refs/heads/main
  pipelines: # pipeline trigger pipeline
    - pipeline: "theSourcePipelineName"
      source: "The real Pipeline name"
      project: "theSourcePipelineProject"
      trigger:
          branches:
           - main
  containers:
    - container: myContainerRegistry
      type: ACR
      azureSubscription: azureSub
      resourceGroup: rg
      registry: reg
      repository: repo
      trigger:
        tags:
          include:  
            - production*

schedules: # schedule pipeline trigger (UI take precedence over YAML and YAML only work if there is no UI settings)
- cron:
  displayName: Daily midnight build
  branches:
    include:
     - master
     - releases/*
    exclude:
     - releases/old/*

trigger:
- main
pool:
  name: 'MyAgent'
#   vmImage: 'ubuntu-latest'
variables:
  mykey: myvalue
# - template: <path_to_yaml>
stages:
- stage: Build
  displayName: Build
  condition: ""
  jobs:
    # - template: <path_to_yaml>
    - job: FirstBuild
      displayName: First Build
    #   dependsOn: <another_job>
      steps:
        # - template: <path_to_yaml>
        - bash: |
            echo 'First Build'
          displayName: First Build

        - download: theSourcePipelineName # download artifact from source pipeline
          artifact: theSourcePipelineArtifact

```
https://marketplace.visualstudio.com/items?itemName=gittools.gittools

https://learn.microsoft.com/en-us/azure/architecture/example-scenario/apps/devops-dotnet-baseline
![CI/CD baseline architecture with Azure Pipelines](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/apps/media/azure-devops-ci-cd-architecture.svg#lightbox)

https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/microservices-with-aks?source=recommendations
![Microservices with AKS and Azure DevOps](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/media/microservices-with-aks.svg)

https://learn.microsoft.com/en-us/azure/architecture/microservices/ci-cd-kubernetes
![Build a CI/CD pipeline for microservices on Kubernetes with Azure DevOps and Helm](https://learn.microsoft.com/en-us/azure/architecture/microservices/images/aks-cicd-flow.png)

https://learn.microsoft.com/en-us/azure/devops/pipelines/process/environments-kubernetes?view=azure-devops

- opening pull request EVENT triggers CI
- merging pull request EVENT triggers CD
———
- there is nothing called dev/qa environments, but it’s PREVIEW environment ( per pull request) . The environment dies when the Pull request is approved ( even before merged ) which a 3rd EVENT.
- Prod of v1 becomes staging of v2 called also blue .. prod of v2 is green ..it’s promotion
———
- Do not allow manual versioning , but auto-calculate it from semantic git commits
- Use the auto-calculated version everywhere to Tag everything
———
- Do not rollback DB by SQL scripts but by Point-in-time recovery ( DB snapshot)