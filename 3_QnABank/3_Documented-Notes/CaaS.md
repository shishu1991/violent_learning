# CaaS Setup and Enablement

**Table Of Contents**
1. [CaaS Setup](#caas-setup)
2. [CaaS context and namespace setup](#caas-context-and-namespace-setup)
### CaaS Setup 

1. Follow the link [here](https://github.deere.com/caas/caas-cli) and go through the **First Time Setup** section of the README doc available in the repository.
2. Download the windows.zip file for Windows machine from [CaaS-CLI-Releases](https://github.deere.com/caas/caas-cli/releases/)
3. Open cmd in admin mode and navigate to the directory where you have exploded the above zip file. Then run the command 
    > caas setup tools -i
4. Set the environment variable and add (.krew\bin) & (caas\bin) to your path variable and restart the system
5. Run the following command in cmd (admin mode)
    > caas setup contexts
6. You will be able to load the contexts and clusters.

### CaaS context and namespace setup

1. Check CaaS client ***kubectl*** is set and you have access by running the following command
    > kubectl version --short
2. To get help on command of kubectl (CaaS cli)
    > kubectl config --help 
    > kubectl get --help
3. To list the contexts running in telemetry
	> kubectl config get-contexts  
    
	**NOTE:** Following context you can use as your playground **caas_us_central_1_development**
4. To list the cluster (generally we don't have access to most of the clusters)
    > kubectl config get-clusters
5. Set your context to one specific cluster
	> kubectl config use-context caas_us_central_1_development 
6. To check the current context                    
	> kubectl config current-context
7. To list namespaces
	> kubectl get namespaces
8. Creating your namespace 
    * To have your namespace as a playground to run and test your application 
    * To create your namespace you need a namespace.yml  [download here](https://github.deere.com/team-webplatform/onboarding/blob/main/namespace.yaml) and update it accordingly to have your namespace
		- Download the file and update it for you with 
			1. Namespace: {your team prefix}-{your name}-playground \<mfg-shishu-playground-devl>
			2. APM: APM0022548
			3. AD: CAAS_MFG_IOT
9. Once a file is updated, navigate to the dir where your file is kept and run the command
	> kubectl apply -f namespace.yml 
10. Verify your namespaces inside the cluster by running the cmd.	
	> kubectl get namespaces 
11. To set the default namepace
	> kubectl config set-context --current --namespace=\<mfg-shishu-playground-devl>