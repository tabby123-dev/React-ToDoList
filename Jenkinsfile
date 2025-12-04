pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = "wtabitha/react-todo"   // change this
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main-branch',
                    url: 'https://github.com/tabby123-dev/React-ToDoList.git'
            }
        }

        stage('Frontend Build') {
            steps {
                dir('dive-react-app') {
                    sh 'npm install'
                    sh 'npm run lint'
                    sh 'npm run build'
                }
            }
        }

        stage('Backend Install') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    sh 'npm test || true'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKERHUB_REPO}:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub_creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push ${DOCKERHUB_REPO}:latest"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
