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
        stage('package') {
            steps {
                sh 'mvn install'
               
            }
        }
    }
}
