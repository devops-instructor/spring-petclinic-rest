pipeline {
    agent any
    tools {
        maven 'maven3.9.16'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'master', url : 'https://github.com/devops-instructor/spring-petclinic-rest.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests -B -ntp'
            }
        }        
    }
}