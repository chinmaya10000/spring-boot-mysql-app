pipeline {
    agent{
        label "slave-1"
    }

    tools {
        maven 'Maven'
    }

    stages{
        stage("clone"){
            steps{
                script {
                    echo 'clone repo'
                    git branch: 'main', url: 'https://github.com/chinmaya10000/spring-boot-mysql-app.git'
                }
            }
        }
        stage("build Jar") {
            steps {
                script {
                    echo 'Building the app..'
                    sh 'mvn clean package'
                }
            }
        }
        stage('build and push') {
            steps {
                script {
                    echo 'Build and push image'
                    withDockerRegistry(credentialsId: '	docker-creds', toolName: 'docker') {
                        sh 'docker build -t chinmayapradhan/spring-boot-mysql-app:1.0 .'
                        sh 'docker push chinmayapradhan/spring-boot-mysql-app:1.0'
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploy to dev..'
                    def dockerComposeCmd = 'docker-compose up -d'
                    
                    sshagent(['ec2-server-key']) {
                        sh "scp docker-compose.yml ubuntu@52.15.242.81:/home/ubuntu"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@52.15.242.81 '${dockerComposeCmd}'"
                    }
                }
            }
        }
    }
}