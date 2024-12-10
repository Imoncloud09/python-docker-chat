pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('e1991516-ff53-4be1-a278-e1951422c0de') // DockerHub credentials
        DOCKER_IMAGE_NAME = 'halrshaldocker/your-image-name' // Replace with your DockerHub repo
        GIT_REPO = 'https://github.com/Imoncloud09/python-docker-chat.git'  // Replace with your GitHub repo
        GIT_BRANCH = 'master' // Replace with the branch you want to build
        DOCKER_IMAGE = "halrshaldocker/your-image-name" // Replace with your image
        KUBERNETES_NAMESPACE = "default"               // Update for a specific namespace
        KUBE_CONFIG_CRED_ID = "k8s"                    // Jenkins credential ID for kubeconfig
        DEPLOYMENT_NAME = "example-deployment"
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: env.GIT_BRANCH]], 
                              userRemoteConfigs: [[url: env.GIT_REPO]]])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat """
                    docker build -t %DOCKER_IMAGE_NAME% .
                    """
                
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    bat """
                    docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%
                    docker push %DOCKER_IMAGE_NAME%
                    """
                }
            }
        }
        stage('Write Deployment File') {
            steps {
                script {
                    def deploymentYaml = """
                    apiVersion: apps/v1
                    kind: Deployment
                    metadata:
                      name: ${DEPLOYMENT_NAME}
                      namespace: ${KUBERNETES_NAMESPACE}
                    spec:
                      replicas: 1
                      selector:
                        matchLabels:
                          app: ${DEPLOYMENT_NAME}
                      template:
                        metadata:
                          labels:
                            app: ${DEPLOYMENT_NAME}
                        spec:
                          containers:
                          - name: ${DEPLOYMENT_NAME}
                            image: ${DOCKER_IMAGE}
                            ports:
                            - containerPort: 80
                    """
                    writeFile file: 'deployment.yaml', text: deploymentYaml
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'k8s', variable: 'KUBECONFIG')]) {
                    bat """
                    kubectl --kubeconfig=%KUBECONFIG% apply -f deployment.yaml --validate=false
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([file(credentialsId: 'k8s', variable: 'KUBECONFIG')]) {
                    bat """
                    kubectl --kubeconfig=%KUBECONFIG% rollout status deployment/${DEPLOYMENT_NAME} -n ${KUBERNETES_NAMESPACE}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment succeeded!"
        }
        failure {
            echo "Deployment failed."
        }
    }
}
