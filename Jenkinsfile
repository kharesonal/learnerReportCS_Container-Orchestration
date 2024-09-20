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
                script {
                    // Checkout code from GitHub repository
                    git branch: 'main', url: 'https://github.com/kharesonal/learnerReportCS_Container-Orchestration'
                }
            }
        }

        stage('Checkout submodules') {
            steps {
                script {
                    sh 'git submodule init && git submodule update'
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    sh 'cd learnerReportCS_backend && docker build -t $DOCKER_HUB_REPO:backend-$VERSION -t $DOCKER_HUB_REPO:backend-latest .'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    sh 'cd learnerReportCS_frontend && docker build -t $DOCKER_HUB_REPO:frontend-$VERSION -t $DOCKER_HUB_REPO:frontend-latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                script {
                    // Push both the versioned and latest tags for backend
                    sh 'docker push $DOCKER_HUB_REPO:backend-$VERSION'
                    sh 'docker push $DOCKER_HUB_REPO:backend-latest'
                }
            }
        }

        stage('Push Frontend Image') {
            steps {
                script {
                    // Push both the versioned and latest tags for frontend
                    sh 'docker push $DOCKER_HUB_REPO:frontend-$VERSION'
                    sh 'docker push $DOCKER_HUB_REPO:frontend-latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure that you set the Kubernetes context if not already set
                    sh 'kubectl config use-context $KUBE_CONTEXT'
                    
                    // Deploy using the specific version tag for both backend and frontend
                    sh 'helm upgrade --install learner ./learnerReport_helm --set backend.image=$DOCKER_HUB_REPO:backend-$VERSION --set frontend.image=$DOCKER_HUB_REPO:frontend-$VERSION'
                }
            }
        }
    }
}
