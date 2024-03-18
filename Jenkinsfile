pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        /* stage("Create an EKS Cluster") {
            steps {
                script {
                    dir('terraform') {
                        sh "terraform init"
                        sh "terraform validate"
                        sh "terraform plan"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        } */

        stage("Deploy to EKS") {
            steps {
                script {
                    dir('k8s_manifests') {
                        sh "aws eks update-kubeconfig --name my-eks-cluster"
                        sh "kubectl apply -f backend-deployment.yaml"
                        sh "kubectl apply -f backend-service.yaml"
                        sh "kubectl apply -f frontend-deployment.yaml"
                        sh "kubectl apply -f frontend-service.yaml"
                        sh "kubectl apply -f full_stack_lb.yaml"
                    }
                }
            }
        }
        
       /* stage("Delete the EKS Frontend") {
            steps {
                script {
                    dir('k8s_manifests/mongo') {
                        sh "kubectl delete -f ."
                    }
                }
            }
        } */
        stage("Deletion of Backend") {
            steps {
                script {
                    dir('k8s_manifests') {
                        sh "kubectl delete -f ."
                    }
                }
            }
        }

        stage("Delete the EKS Cluster") {
            steps {
                script {
                    dir('terraform') {
                        sh "aws eks update-kubeconfig --name my-eks-cluster"
                        sh "terraform destroy -auto-approve"
                    }
                }
            }
        }
    }
}
