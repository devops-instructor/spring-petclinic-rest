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
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Coverage') {
            steps {
                sh 'mvn jacoco:report -B -ntp'
            }
            post {
                success {
                    recordCoverage(tools: [[parser: 'JACOCO']])
                }
            }
        }        
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests -B -ntp'
            }
        }
        stage('SonarQube') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -B -ntp'
                }
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