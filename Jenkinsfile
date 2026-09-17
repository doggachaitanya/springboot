pipeline {
    agent any
    tools {
        maven 'maven'
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
    }
}