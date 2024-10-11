pipeline{
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
    }
}