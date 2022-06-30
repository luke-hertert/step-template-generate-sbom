# step-template-generate-sbom
A Harness step template to generate SBOMs from docker images.

# Example usage

```yaml
- step:
    name: Generate SBOM
    identifier: sbom
    template:
      templateRef: Generate_SBOM
      versionLabel: v1
      templateInputs:
        type: Run
        spec:
          envVariables:
            image: myimage
            tag: latest
            outputDir: /outputs/sbom
```

Note this this step requires access to the Docker API, so it is recommended to also include a service dependency as part of the stage.  For example:

```yaml
serviceDependencies:
  - identifier: dind
    name: dind
    type: Service
    description: Docker in docker
    spec:
      connectorRef: account.Luke_Dockerhub
      image: docker:dind
      privileged: true
      entrypoint:
        - dockerd-entrypoint.sh
        
sharedPaths:
  - /var/run
  - /step-outputs
```
