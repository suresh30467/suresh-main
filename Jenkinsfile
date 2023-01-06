pipeline {
    agent any
    tools {
        maven "maven"
    }
    
     stages {
     	stage("clone code"){
	        steps{
	           sh 'rm -rf *'
	           git branch: 'main', url: 'https://github.com/suresh30467/suresh-1.git'   
	        }
	    }

        stage ('build with maven'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
		}	


        stage ('ansible'){
            steps {
                sh 'docker build -t demo .'

            }
		}
        stage ('docker login'){
            steps {
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/x5q3g8p0'
            }
		}	

        stage ('tag'){
            steps {
                sh 'docker tag demo:latest public.ecr.aws/x5q3g8p0/demo:latest'

            }
		}

        stage ('push'){
            steps {
                sh 'docker push public.ecr.aws/x5q3g8p0/demo:latest'

            }
		}

        stage ('deploy'){
            steps {
                sh 'chmod 400 surya1.pem'
                sh 'ansible-playbook -i hosts pull.yaml'

            }
		}

    }
}
