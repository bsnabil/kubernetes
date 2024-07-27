pipeline {
    agent any

    stages {
        stage('Preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                sh "scp -r -i /path/to/minikube/ssh-key docker@$(minikube ip):~/"
                sh "minikube ssh 'docker build -t webapp:${commit_id} .'"
                echo 'Build complete'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh "sed -i 's/sirichardchesterwood\/k8s-fleetman-webapp-angular:release-5/webapp:${commit_id}/g' deployment.yaml"
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl get all'
                echo 'Deployment complete'
            }
        }
    }
}

