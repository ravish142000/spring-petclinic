pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ravi685/petclinic:${BUILD_NUMBER} .'
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
                    docker push ravi685/petclinic:${BUILD_NUMBER}
                    docker tag ravi685/petclinic:${BUILD_NUMBER} ravi685/petclinic:latest
                    docker push ravi685/petclinic:latest
                    '''
                }
            }
        }
    }
}
