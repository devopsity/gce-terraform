# gce-terraform

## Set up your Google Cloud Platform working environment

install and configure the Google Cloud SDK and the Kubernetes command line tool

```echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install google-cloud-sdk kubectl
gcloud init
```

enable the Google Cloud APIs we will be using:

```gcloud services enable compute.googleapis.com
gcloud services enable servicenetworking.googleapis.com
gcloud services enable cloudresourcemanager.googleapis.com
gcloud services enable container.googleapis.com
```

Create a service account

```gcloud iam service-accounts create terraform-gke```

Grant the necessary roles for our service account to create a GKE cluster and the associated resources:


```gcloud projects add-iam-policy-binding gmg-hosting --member serviceAccount:terraform-gke@gmg-hosting.iam.gserviceaccount.com --role roles/container.admin
gcloud projects add-iam-policy-binding gmg-hosting --member serviceAccount:terraform-gke@gmg-hosting.iam.gserviceaccount.com --role roles/compute.admin
gcloud projects add-iam-policy-binding gmg-hosting --member serviceAccount:terraform-gke@gmg-hosting.iam.gserviceaccount.com --role roles/iam.serviceAccountUser
gcloud projects add-iam-policy-binding gmg-hosting --member serviceAccount:terraform-gke@gmg-hosting.iam.gserviceaccount.com --role roles/resourcemanager.projectIamAdmin
gcloud projects add-iam-policy-binding gmg-hosting --member serviceAccount:terraform-gke@gmg-hosting.iam.gserviceaccount.com --role roles/iam.serviceAccountUser
```


create and download a key file that Terraform will use to authenticate as the service account against the Google Cloud Platform API:

```gcloud iam service-accounts keys create terraform-gke-keyfile.json --iam-account=terraform-gke@gmg-hosting.iam.gserviceaccount.com
```



## Create the GKE cluster

Then perform the following commands on the root folder:

```terraform init
terraform plan
terraform apply
terraform destroy
```
