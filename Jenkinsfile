pipeline {
    agent any

    environment {
        IMAGE = "yaswanth1999/myapp"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            steps {
                sh 'cd app && docker build -t $IMAGE:$TAG .'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE:$TAG'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                helm upgrade --install myapp ./helm-chart \
                --set image.repository=$IMAGE \
                --set image.tag=$TAG
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
                sh 'kubectl get ingress'
            }
        }
    }
}

