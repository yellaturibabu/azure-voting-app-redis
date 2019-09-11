pipeline {
	
	agent any
	options {
		disableConcurrentBuilds()
		timestamps()
		buildDiscarder(logRotator(numToKeepStr: '10'))
	}
	parameters {
	    choice(name: 'Environment', choices: 'dev\nqa\prod', description: 'The environment to deploy')
	}
			
	stages {
		stage ('azure-voting-app-redis - Checkout') {
			steps {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/yellaturibabu/azure-voting-app-redis.git']]])
			}
		}
		stage ('load configurations') {
			steps{
		                echo 'load configurations'
			   	load '"${params.Environment}"/config/Deploy.groovy'
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

				sh "helm upgrade --install --force ${Layout_release} ${Layout_chart}"
				}
		}	
	}

	post { 
		always { 
			echo 'Build Steps Completed'
		}
	}
}
