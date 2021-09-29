def dockerRun='docker run -p 80:80 -d --name mavenimg sijisdocker/mavenimg:v1'
pipeline {
    agent {label 'agent'}
    tools{
        maven 'maven'
        dockerTool 'docker'
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
        stage('Sonarqube analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar'){
                        sh 'ls'
                        sh 'mvn sonar:sonar -DskipTests'
                     }
                 }
            }
        }
        stage('docker-build') {
            steps {
              script{
                docker.withTool('docker'){
                  sh 'docker version'
                  sh 'docker build -t mavenimg:v1 .'
                  sh 'docker tag mavenimg:v1 sijisdocker/mavenimg:v1'
                }
            }
        }
    }
      stage('docker-login'){
        steps{
          
            withCredentials([string(credentialsId: 'DOCKER_HUB', variable: 'PASSWORD')]) {
              sh "docker login -u sijisdocker -p ${PASSWORD}"
     
        
      }
      }
      }
      stage('push image to docker hub'){
        steps{
        sh 'docker push sijisdocker/mavenimg:v1'
      }
      }
      
       stage('Run on server'){
        steps{
          
          sshagent(['SSH-ID']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@35.175.115.151 ${dockerRun}"
      }

      }
      }
    }
}
