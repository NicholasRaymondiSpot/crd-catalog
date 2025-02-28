# CRDs Catalog  

This is a copy of the [CRDs-catalog](https://github.com/datreeio/CRDs-catalog) but is stripped down only to the CRDs that have been deployed to our clusters. This is needed to deal with the custom VPA autoscaling CRDs created by nops.  

This repository aggregates hundreds of popular Kubernetes CRDs (`CustomResourceDefinition`) in JSON schema format. These schemas can be used by [Kubeconform](https://github.com/yannh/kubeconform) as an alternative to `kubectl --dry-run`, to perform validation on custom (and native) Kubernetes resources.  

Running Kubernetes schema validation checks helps apply the **"shift-left approach"** on machines **without** giving them access to your cluster (e.g. locally or on CI).  

## How to use the schemas in the catalog  
### Kubeconform  
```  
kubeconform -schema-location default -schema-location 'https://raw.githubusercontent.com/datreeio/CRDs-catalog/main/{{.Group}}/{{.ResourceKind}}_{{.ResourceAPIVersion}}.json' [MANIFEST]  
```  

👉 If you encounter custom resources that are not part of the catalog, or you want to validate the schemas in an air-gapped environment, use the [CRD Extractor](#crd-extractor) in the Utilities dir.   

---

## CRD Extractor  

This repository also contains a handy utility that extracts all CRDs from a cluster and converts them to JSON schema.  

### What does this utility do?  
1. Checks that the prerequisites are installed.  
2. Extracts your CRDs from your cluster using kubectl.  
3. Downloads a script from the [kubeconform](https://github.com/yannh/kubeconform/blob/master/scripts/openapi2jsonschema.py) repo that converts your CRDs from openAPI to JSON schema.  
4. Runs the script, and saves the output to your machine under `$HOME/.datree/crdSchemas/`  

### Supported Platforms  
This utility supports **MacOS** and **Linux**.  

### Prerequisites  
The following programs are required to be installed on the machine running this utility:  
* [python3](https://www.python.org/downloads/)  
* [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)  

### Usage  
To use the CRD Extractor:  
1. Use AWS-CLI and authenticate with the correct account  
2. Use aws eks to load the config for the correct EKS cluster  
3. Run the extractor and look for the results in `$HOME/.datree/crdSchemas/`  
```  
./Utilities/crd-extractor.sh  
```  

## Contributing CRDs to the catalog  
If the catalog is missing public custom resources (CRs) that you would like to automatically validate using these tools, you can open an issue or use the **[CRD Extractor](#crd-extractor)** to add the schemas to this repository by creating a pull request.  

## Resources  
* [opensource.com - Why you need to use Kubernetes schema validation tools](https://opensource.com/article/21/7/kubernetes-schema-validation)  
* [redhat.com - Validating OpenShift Manifests in a GitOps World](https://cloud.redhat.com/blog/validating-openshift-manifests-in-a-gitops-world)  
* [kubeval/issues/47 - cannot validate CustomResourceDefinitions](https://github.com/instrumenta/kubeval/issues/47)  
