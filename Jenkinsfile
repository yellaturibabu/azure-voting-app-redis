pipeline {
	
	agent any
		
	stages {
		stage ('azure-voting-app-redis - Checkout') {
			steps {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/yellaturibabu/azure-voting-app-redis.git']]])
			}
		}
		stage ('load configurations') {
			steps{
					load 'dev/config/Deploy.groovy'
					echo "${clusterName}"
			}
	}
		stage ('authenticate') {
			steps{
					//exectute build, linter, and test runner here    
                    			sh "az account set --subscription ${subscriptionID}"
					sh "az aks get-credentials --resource-group ${resourceGroup} --name ${clusterName}"
			}
	}
		stage ('Helm Deploy to K8s'){
			steps{

					sh "helm upgrade --install --force ${release} stable/tomcat"
				}
		}	
	}

	post { 
		always { 
			echo 'Build Steps Completed'
		}
	}
}
