def containerName="project"
def tag="latest"
def dockerHubUser="ankeetchauhan505"
def gitURL="https://github.com/AnkeetC/star-agile-health-care.git"

node {
    stage('Checkout') {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: gitURL]]]
    }

    stage('Build'){
        sh "mvn clean install"
    }

    stage("Image Prune"){
         sh "docker image prune -f"
    }
     stage('Containerize the application'){
        echo 'Creating Docker image'
        sh "docker build -t ankeetchauhan505/health2 ."
    }
	stage('test'){
	     sh "mvn test"
    }	 
	
	stage("Ansible Deploy"){
	    ansiblePlaybook credentialsId: '34.227.224.24', installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: '/etc/ansible/star-agile-health-care/ansible-playbook.yml'
	}
}
