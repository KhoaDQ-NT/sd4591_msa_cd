pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-1'
        K8S_NAMESPACE = 'sd4591_K8S_NAMESPACE'
    }

    stages {
        stage('Deploy to EKS') {
            steps {
                script {
                    withAWS(region: AWS_REGION, credentials: 'AWS Cred') {
                        sh "kubectl apply -f backend.yaml -n ${K8S_NAMESPACE}"
                        sh "kubectl apply -f frontend.yaml -n ${K8S_NAMESPACE}"
                        sh "kubectl apply -f mongodb.yaml -n ${K8S_NAMESPACE}"
                        sh "kubectl apply -f ingress.yaml -n ${K8S_NAMESPACE}"
                    }
                }
            }
        }
    }
}