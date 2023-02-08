pipeline {
   agent any
    stages {
         stage('Clean existing Built Containers'){
            steps {
            sh 'docker rmi -f hub.enlight.dev/360-containers/sample1:$BRANCH_NAME || true'
            }  
        }
       stage('Build container Image'){
         steps{
             echo '$BRANCH_NAME'
             sh 'docker build -t hub.enlight.dev/360-containers/sample1:$BRANCH_NAME .'
            }
            
        }
        stage('Push Image'){
		   	steps{
			withDockerRegistry(credentialsId: 'enlight360-dev-hub', url: 'https://hub.enlight.dev') 
			{
			    sh 'docker push hub.enlight.dev/360-containers/sample1:$BRANCH_NAME'
			}
		   	}	
		}
        	
    
    stage('Deploying App to Kubernetes') {
      steps {
        script {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''ansible-playbook -i /etc/ansible/hosts /opt/k8-lab/ans-vault.yaml;
ansible-playbook -i /etc/ansible/hosts /opt/k8-lab/cicd-deploy1.yml''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/opt/k8-lab/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)]) 
        }
      }
    }
    } 
}
