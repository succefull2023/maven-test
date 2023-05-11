pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '827648740654.dkr.ecr.us-east-1.amazonaws.com/devops-terra'
    registryCredential = 'aws-credentials'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/succefull2023/maven-test.git'
            }
        }
        stage(Sonarqube scan) {
          steps{
          witSonarQubeEnv('sonar') {
        sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=succefull2023_geolocation1' 
                } 
          }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean install package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
