pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        ACR_SERVER = 'springbootacr98107777.azurecr.io'
        IMAGE_NAME = 'Springboot'
        IMAGE_TAG = 'latest'
    }
    stages {
        stage ('Checkout from Git') 
        {
            steps {
                git branch: 'main' , url: 'https://github.com/doggachaitanya/springboot.git'
            }
        }
        stage ('Validate with Maven') 
        {
            steps {
                sh 'mvn validate'
            }
        }
        stage ('Compile with Maven')
        {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('Test with Maven')
        {
            steps {
                sh 'mvn test'
            }
        }
         stage ('Sonar Qube Analysis')
        {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh '''
                    mvn sonar:sonar \
                    -Dsonar.organization=bootcamp1 \
                    -Dsonar.projectKey=springbootjavaapp \
                    -Dsonar.projectName=springbootjavaapp \
                    -Dsonar.java.binaries=target/classes
                '''
                }
            }
        }
        stage ('Package with Maven')
        {
            steps {
                sh 'mvn package'
            }
        }
    
        stage('Docker Build') {
        steps {
            sh '''
                docker build -t $ACR_SERVER/$IMAGE_NAME:$IMAGE_TAG .
            '''
                }
        }
    }
}

 