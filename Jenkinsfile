pipeline {
    // agent any
    // tools {
    //     maven 'maven3.9.16'
    // }
    agent {
        docker {
            image 'maven:3.9.16-eclipse-temurin-21'
        }
    }
    environment {
        MAVEN_OPTS = "-Dmaven.repo.local=${WORKSPACE}/.m2/repository"
    }
    triggers {
        githubPush()
    }
    stages {
        // stage('Checkout SCM') {
        //     steps {
        //         git branch: 'master', url : 'https://github.com/devops-instructor/spring-petclinic-rest.git'
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
    }
    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        cleanup {
            cleanWs()
        }
    }
}