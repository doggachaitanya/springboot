pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        ACR_SERVER = 'springbootacr98107777.azurecr.io'
        IMAGE_NAME = 'springboot'
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
        stage('PUSH to ACR') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'acr-creds', usernameVariable: 'AC_USER', passwordVariable: 'ACR_PASS')]) {

            sh '''
            echo $ACR_PASS | docker login $ACR_SERVER -u "$AC_USER" --password-stdin
            docker push $ACR_SERVER/$IMAGE_NAME:$IMAGE_TAG
            docker logout $ACR_SERVER
            '''
        }
    }
}
stage ('Deploy to AKS') {
    steps {
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBCONFIG')]) {
            sh '''
            kubectl apply -f k8s/deployment.yaml
            kubectl apply -f k8s/service.yaml
            '''
        }
    }
}
    }

}

 