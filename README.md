**All right, getting redash on kubernetes is like making a fish walk.**



For Joule, we put most of our deployments on Kubernetes for the easy management and efficiency. But it does cause a bit of trouble with persistent deployments, especially redash. And this is how we solved it 

It seems using a satefull prostgress DB on GKE is futile, we dicided to use GCP CloudSQL to sync with the postgress on every start up, so you can stop and delete the whole deployment and start it back up with all your information still on. 

We are also very very lucky to have someone create most of the setup [here](https://github.com/donaldrauscher/redash-gke). Shout out to Donald Rauscher.



## Dependencies

 [Terraform](https://www.terraform.io/), a HashiCorp software that helps you auto provision anything on GCP, AWS, and many many more.

[Helm](helm.sh), Helm helps you manage Kubernetes applications — Helm Charts helps you define, install, and upgrade even the most complex Kubernetes application.

## Infrastructure Setup

We will use Terraform to deploy our infrastructure, but before we do that we need to create a GCP service account and download it as a json. 

After you downloaded the JSON, put it in  a safe directory that you will remember, then set the environmental variable to `GOOGLE_APPLICATION_CREDENTIALS`

```bash
expose GOOGLE_APPLICATION_CREDENTIALS = /path/to/json	
```

Set the context

```bash
export PROJECT_ID=$(gcloud config get-value project -q)
```

Run terraform

```bash
terraform apply -var project=${PROJECT_ID}	
```









```dockerfile
# Set the base image
FROM redash/redash:4.0.0-rc.1
# Dockerfile author / maintainer 
ENV REDASH_ADDITIONAL_QUERY_RUNNERS "redash.query_runner.cass" 
```



