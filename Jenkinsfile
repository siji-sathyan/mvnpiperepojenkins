pipeline {
    agent any
    tools{
        maven 'maven'
    }

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/siji-sathyan/mvnpiperepojenkins.git'
            }
            
        }
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('install') {
            steps {
                sh 'mvn install'
               
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
    }
}
