pipeline {
    agent { label 'slave' }
    tools {
        maven 'Maven'
        jdk 'JDK21'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HimanshuSinghThakurGit/test.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package'
            }
        }
        stage('Deploy to Local Server') {
            steps {
                bat 'copy target\\demo-0.0.1-SNAPSHOT.jar C:\\LocalServer\\demo-0.0.1-SNAPSHOT.jar'
                bat 'start /B "C:\\Program Files\\Java\\jdk-21.0.3\\bin\\java" -jar C:\\LocalServer\\demo-0.0.1-SNAPSHOT.jar'
            }
        }
        stage('Deploy to Production Server') {
            steps {
                input message: 'Deploy to Production?', ok: 'Yes'
                bat 'copy target\\demo-0.0.1-SNAPSHOT.jar C:\\ProdServer\\demo-0.0.1-SNAPSHOT.jar'
                bat 'start /B "C:\\Program Files\\Java\\jdk-21.0.3\\bin\\java" -jar C:\\ProdServer\\demo-0.0.1-SNAPSHOT.jar'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
            emailext(
                to: 'team@example.com',
                subject: "Build ${currentBuild.fullDisplayName}",
                body: "Build ${currentBuild.result} - Check console output at ${env.BUILD_URL}",
                attachLog: true
            )
        }
    }
}