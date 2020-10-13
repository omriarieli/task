# task
 DevOps - Home Assignment
1. Deploy kind on your local machine using a config file which mounts the docker.sock and a host folder to hold jenkins_home data for persistence.
kind ​create​ ​cluster​ --config=./misc/kind/​cluster​.yaml
2. Deploy jenkins helm chart using values.yml file customized for persistence, additional
plugins and a nodejs global tool needed for this task.
3. Deploy nginx ingress controller and ingress object to expose jenkins UI.
4. After jenkins resources finished deploying Login to jenkins UI at http://localhost/login​ with admin password retrieved from helm notes instruction 1 and prepare jenkins for the pipeline as follows:
- ​Add credentials for dockerhub:​ ​jenkins home > Manage Jenkins > Manage Credentials > click the arrow next to (global) > Add credentials. choose Username with password, add your dockerhub creds and give it an id (dockerhub) click ok.
- ​Add credentials for kubeconfig:​ Repeat the previous step again up to Add credentials this time choosing Kubernetes configuration give it an id (kindconfig), choose enter directly and paste the kind config. Make sure to change the server url to https://kubernetes:443​ since the deployment is going to be applied from within the same cluster jenkins is on. Click ok.
5. Return to jenkins home screen click new item on the left enter an item name choose pipeline and click ok. Under ​Pipeline​ Definition choose Pipeline script from SCM. under SCM choose git and paste the .git url to the repository under Repository URL, specify the branch to build under Branch Specifier. In Script Path enter the path to the Jenkinsfile in the repository. Click save. On the left click build now.
6. When the build finishes verify the sample-app-docker got deployed in the gruntwork namespace.
kubectl -n gruntwork ​get​ ​all
  helm install jenkins jenkins​/jenkins -f ./misc/​jenkins_helm​/values.yaml
 kubectl ​apply​ -f ./misc/kind/nginx-ingess.yaml kubectl ​apply​ -f ./misc/kind/jenkins-ingress.yaml
   
 7. Copy sample-app-docker pod name and expose it to test the app is serving Hello World! Correctly.
8. Browse to ​http://localhost:8888/​ and check the page is displaying Hello World!
9. Copy the app logs from the console.
 kubectl -​n​ gruntwork port-forward pod/​sample​-​app​-docker-56cd44977d-zfx7q 8888:80
  kubectl -​n​ gruntwork logs pod/​sample​-​app​-docker-56cd44977d-zfx7q
