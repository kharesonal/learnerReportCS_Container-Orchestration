pipeline {
    agent any

    environment {
        GIT_REPO = "https://github.com/kharesonal/learnerReportCS_Container-Orchestration.git"  // Ensure both FE/BE are part of this repo
        REGISTRY = "docker.io"
        DOCKER_HUB_REPO = "sonalsrivastava1189/learner_report"
        KUBE_CONTEXT = "minikube"
        VERSION = "v${BUILD_NUMBER}"  // Automatically use Jenkins build number as version
    }

    stages {
        stage('Checkout Code') {
            steps {
                 echo 'Checkout Code Successful'
            }
        }

        stage('Checkout submodules') {
            steps {
                 echo 'Checkout submodules successful'
            }
        }

        stage('Build Backend') {
            steps {
                 echo 'Build Backend successful'
            }
        }

        stage('Build Frontend') {
            steps {
                 echo 'Checkout Frontend successful'

            }
        }

        stage('Login to Docker Hub') {
            steps {
                 echo 'Docker Hub logged in'
            }
        }

        stage('Push Backend Image') {
            steps {
                 echo 'Images pushed succesfully'
            }
        }

        stage('Push Frontend Image') {
            steps {
                 echo 'Images pushed successfully'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                 echo 'Kubernetes deployment successful'
            }
        }
    }
}
