## Steps to Trigger GitHub Action Pipeline
1- Update the GitHub Actions secrets. Go to https://github.com/Wole-001/clo835_fall2022_assignment1/settings/secrets/actions and update the following secrets:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_SESSION_TOKEN

2- Commit the changes to the final-project branch of the repository. This will trigger the pipeline to generate the Docker images.

3- Wait for the pipeline to build the Docker images and push them to ECR.

## Steps to Apply Kubernetes Resources
1- Open your terminal and navigate to the k8s-manifests directory of the repository.

2- Create a new Kubernetes namespace for the application by running the following command:

`kubectl create ns final`

3- Apply the Kubernetes resources using the following command:

`kubectl apply -f .`
This will apply all the resources defined in the k8s-manifests directory.
