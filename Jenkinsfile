pipeline {
    agent any
    tools {
        maven 'maven3.9.16'
    }
    stages {
        // stage('Checkout SCM') {
        //     steps {
        //         git branch: 'master', url: 'https://github.com/devops-instructor/spring-petclinic-rest.git'
        //     }
        // }
        stage('Compile') {
            steps {
                sh 'mvn clean compile -B -ntp'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -B -ntp'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Coverage') {
            steps {
                sh 'jacoco:report -B -ntp'
            }
        }        
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests -B -ntp'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        cleanup {
            cleanWs()
        }
    }
}