# <ins>**Getting Started with your NYS ITS CI/CD repository.** </ins>

# NYS ITS CI/CD User Manual


#### Post Intake AD Review & Membership Augmentation<a name="Post Intake AD Review & Membership Augmentation"></a>

>The CICD intake creates four AD groups that can be managed in NYS ITS [Active Roles System](https://ars.svc.ny.gov/ARWebSelfService/). The _mgr group created during the intake will be the group with  the ability to add or remove users from each of the four groups. The full group name will be the created by what was input during intake and follow a pattern of **${ORG} ${TEAM} ${APPLICATION} ${ROLE}**
	
><b><ins>GROUP ${ROLES}:</ins></b>

>  **_mgr:**  Users in this group manage the access to the groups in AD in the NYS ITS ARS system.

>  **_maintainers:**  Users in this group manage and configure the tools in the CICD lifecycle. Staff added to this group will be granted access to Gitlab, Artifactory, SonarQube, Prisma Compute. Users in this group will be the approvers of development merges into the dev, release and Main branches.

>   **_developers:**  Users in this group are able to push to feature branches and to request merges into the dev branch. Development of code should be maintained in a feature branch and reviewed before merging into the dev branch for quality and security vulnerabilities.

>  **_reviewers(TBD):**  Users in this group are able to review security scans performed by SonarQube, SAST, Prisma and other products to verify code compliance and standards are met.

> Click the Icon below to begin adjusting team membership:

[<img src="CICD/READMES/img/one-identity.png">](https://ars.svc.ny.gov/ARWebSelfService/)

#### Review CICD Variables and adjust as needed<a name="Review CICD Variables and adjust as needed"></a>
	
>Variables are located in Gitlab in the settings >> CICD >> variables section. These variables will be used to alter the way the pipeline executes the CICD stages and also the way your code tests and builds. Some variables are set as default and can be overridden. It is recommended you make sure the variables of BUILD_IMAGE is set to a docker image that is usable to build your code and also FQDN is set to a URL that you chose as a distinguisher for your application. The default for FQDN will be set at intake as **${ORG} ${ENV} ${TEAM} ${APPLICATION}**.apps.ocp-np.caas.ny.gov. 

>The **.apps.ocp-np.caas.ny.gov, .apps.ocp.caas.ny.gov or .apps.ocp-lab.caas.ny.gov** should be the ending of any FQDN you chose or it will not work. For a list of Variables go to the Cheat sheet DevSecOps CICD Variable cheat sheet here.

[INDEX OF PIPELINE VARIABES](CICD/READMES/PIPELNE_BRANCHES_OVERVIEW.md#base-pipeline-variables-index)



Click the ICON Below to learn about CICD Variables in Gitlab:

[<img src="CICD/READMES/img/Gitlab.png">](https://docs.gitlab.com/ee/ci/variables/#define-a-cicd-variable-in-the-ui)

#### Create Feature Branch<a name="Create Feature Branch"></a>
>Create a branch that is named feature-* from your dev branch. If it does not have the beginning of "feature-" then the pipeline will not kick off to deploy the feature build. This 
means it will not work. 

[<img src="CICD/READMES/img/Gitlab.png">](https://docs.gitlab.com/ee/user/project/repository/branches/)

#### Edit Helm Values.yml<a name="Edit Helm Values.yml"></a>
>Your code will not deploy properly until you edit the Helm chart values.yml in the CICD >> helmProps >> $ENV . To get the deployment to work properly you will have to make sure the
readiness and liveness probe areas are setup with a proper URI that will be accessible to verify the deployment is up. These settings are used as they are names to verify the pod is 
ready and after to make sure it is still live. These are needed to get the deployment to work. More on the helm chart can be found here.	

Click below to edit the appropriate value file in the repository. 

[<img src="CICD/READMES/img/HELM.png">](./CICD/helmProps/)

Click this the below Icon to learn more about helm

[<img src="CICD/READMES/img/HELM.png">](https://helm.sh/docs/intro/)


#### Adjust Build Variables</a>
>Build Variables are located in Gitlab's settings >> CICD >> variables section. These variables will be used to alter the way the Build Verify and Build Arfitfact execute. Some variables are set as default and can be overridden. It is recommended you make sure the variables of BUILD_IMAGE is set to a version compatible with the version of code being built. If the build commands are not acceptabled you may add a script that will be executed in there places. 

If you are using Mavan to build click this link:
[INDEX OF MAVEN BUILD VARIABES](CICD/READMES/BUILD_OVERVIEW.md#maven-pipeline-variables)

If you are using dotnet to build click this link:
[INDEX OF MAVEN BUILD VARIABES](CICD/READMES/BUILD_OVERVIEW.md#dotnet-pipeline-variables)


#### Add Code to feature Branch & push<a name="Add your code to feature branch and push"></a>
>After you edit the helmProps in the repo in the previous step replace the existing repository code with yours. Do not remove the helm or CICD folders because they are needed for 
some pipeline stages. If this is a docker image make sure you have edited the Dockerfile as needed and then push to the feature branch. More on the feature pipeline can be found here




#### Merge To dev Branch<a name="Merge To dev Branch"></a>
>Once you have verified the code is working as a feature you can merge to the dev branch. An approval will be needed by a maintainer for merge on the 
dev branch pipeline. You can find more information on the release pipeline and its stages here.



#### Merge To release Branch<a name="Merge To release Branch"></a>
>Once you have verified the code is working in dev you can merge to the release branch to promote as a production release. An approval will be needed by a maintainer for merge on the 
release branch pipeline. You can find more information on the release pipeline and its stages here.


#### Tool In the CICD Pipeline


![alt text](CICD/READMES/img/ITS-DEVSECOPS-view.png "DEVSECOPS Overview")








#### Useful Links<a name="Useful Links"></a>

Self Service Active Directory Group and user management:

[<img src="CICD/READMES/img/one-identity.png">](https://ars.svc.ny.gov/ARWebSelfService)


NYS ITS Artifactory repository. Your repositories should be accesable for use after intake:

[<img src="CICD/READMES/img/artifactory.png">](https://registry.ny.gov/ui/login/)


NYSITS Prisme Cloud Console, This will provide a view of your projects in thier namespace if you are using OpenShift:

[<img src="CICD/READMES/img/prisma.png">](https://prisma-compute.apps.ocp.caas.ny.gov/)


NYSITS logs and monitoring Elastic OpenShift Dashboard. you can filter on Namspace and POD:

[<img src="CICD/READMES/img/ece.png">](https://b1fca3cc314c4adcb9791dd49536fad3.ecenys.svc.ny.gov:9243/app/dashboards#/view/1cfcd4a0-9948-11ed-b713-7d483be8965d?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now)))



NYS ITS sonarQube console. Accessable through Gitlab auth:

[<img src="CICD/READMES/img/sonar.png">](https://code-analysis.svc.ny.gov)

Documentation on creating a JMeter test plan:

[<img src="CICD/READMES/img/apachejmeter.png">](https://jmeter.apache.org/)



#### Useful README Links<a name="Useful Links"></a>


[CICD BRANCHES OVERVIEW](CICD/READMES/PIPELNE_BRANCHES_OVERVIEW.md)

[CICD BUILD OVERVIEW](CICD/READMES/BUILD_OVERVIEW.md)


[CICD ARS OVERVIEW](CICD/READMES/AD_ARS_OVERVIEW.md)


[CICD ARTIFACTORY OVERVIEW](CICD/READMES/ARTIFACTORY_OVERVIEW.md)


[CICD HELM OVERVIEW](CICD/READMES/HELM_OVERVIEW.md)
