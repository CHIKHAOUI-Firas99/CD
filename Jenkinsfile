pipeline {
  agent any
  
  stages {
      stage('Pull GIT') {
                steps {
                    echo 'Pulling...';
                      git branch: 'master',
                      url : 'https://github.com/CHIKHAOUI-Firas99/CD.git',
                      credentialsId: 'github';
                }
    }
    
    stage('build') {
        steps {
            sh 'ansible-playbook Ansible/build.yml -i Ansible/inventory/host.yml'
            
        }
    }
    stage('Docker image') {
        steps {
            sh 'sudo systemctl stop docker'
            sh 'sudo dockerd&'
            sh 'ansible-playbook Ansible/docker.yml -i Ansible/inventory/host.yml'
            
        }
    }
        stage('Docker registry & push') {
        steps {
            sh 'ansible-playbook Ansible/docker-registry.yml -i Ansible/inventory/host.yml'
            
        }
    } 
     stage('run application on minikube'){
         steps {
        sh 'minikube kubectl -- apply -f deployment-service.yml -n angular'
        }     
         
     }
}
}
