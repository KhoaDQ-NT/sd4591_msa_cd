pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-1'
        K8S_NAMESPACE = 'sd4591-k8s-namespace'
        EKS_NAME = 'my-eks-cluster-devops'
        backendEcrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/backend'
        frontendEcrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/frontend'
    }

    stages {
        stage('Deploy to EKS') {
            steps {
                script {
                    withAWS(region: AWS_REGION, credentials: 'AWS Cred') {
                        sh "aws eks --region ${AWS_REGION} update-kubeconfig --name ${EKS_NAME}"
                        sh "kubectl create namespace ${K8S_NAMESPACE} --region ap-southeast-1"
                        sh "kubectl apply -f mongodb.yaml -n ${K8S_NAMESPACE}"
                        sh "aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin ${backendEcrRepo}"
                        sh "kubectl apply -f backend.yaml -n ${K8S_NAMESPACE}"
                        sh "aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin ${frontendEcrRepo}"
                        sh "kubectl apply -f frontend.yaml -n ${K8S_NAMESPACE}"
                        sh "kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.1/deploy/static/provider/cloud/deploy.yaml"
                        sh "kubectl apply -f ingress.yaml"
                        sh "kubectl get ingress -o wide"
                    }
                }
            }
        }
    }
}