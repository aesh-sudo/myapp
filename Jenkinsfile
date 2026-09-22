pipeline {
    agent {
        docker {
            image 'lachlanevenson/k8s-kubectl:v1.25.0'
            args '--entrypoint= --network=minikube'
        }
    }
    stages {
        stage('Verify kubectl') {
            steps {
                sh 'kubectl version --client'
            }
        }
        stage('Prepare manifests') {
            steps {
                sh "sed -i 's/IMAGE_TAG/${BUILD_NUMBER}/g' k8s/deployment.yml"
                sh 'cat k8s/deployment.yml | grep image:'
            }
        }
        stage('Deploy to K8s') {
            steps {
                withCredentials([file(credentialsId: 'minikube-config', variable: 'KUBECONFIG')]) {
                    sh 'kubectl apply -f k8s/deployment.yml'
                    sh 'kubectl get pods'
                }
            }
        }
    }
}
