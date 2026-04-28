pipeline {
    agent any

    environment {
        IMAGE_NAME = "ravi685/petclinic"
    }

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', 
                    usernameVariable: 'docker_username',
                    passwordVariable: 'docker_password'
                )]) {
                    sh '''
                    echo "${docker_password}" | docker login -u "${docker_username}" --password-stdin
                    docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                    docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }
    }
}
