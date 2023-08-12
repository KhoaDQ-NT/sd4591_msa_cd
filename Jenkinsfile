pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-1'
        K8S_NAMESPACE = 'sd4591_K8S_NAMESPACE'
        EKS_NAME = 'my-eks-cluster-devops'
    }

    stages {
        stage('Deploy to EKS') {
            steps {
                script {
                    withAWS(region: AWS_REGION, credentials: 'AWS Cred') {
                        sh "aws eks --region ${AWS_REGION} update-kubeconfig --name ${EKS_NAME}"
                        sh "kubectl apply -f mongodb.yaml -n ${K8S_NAMESPACE}"
                        sh "aws ecr get-login-password --region ap-southeast-1 | sudo docker login --username AWS --password-stdin ${backendEcrRepo}"
                        sh "kubectl apply -f backend.yaml -n ${K8S_NAMESPACE}"
                        sh "aws ecr get-login-password --region ap-southeast-1 | sudo docker login --username AWS --password-stdin ${frontendEcrRepo}"
                        sh "kubectl apply -f frontend.yaml -n ${K8S_NAMESPACE}"
                    }
                }
            }
        }
    }
}