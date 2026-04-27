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
                sh 'docker build -t ravi685/petclinic:v1 .'
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                echo "Ravish142000" | docker login -u ravi685 --password-stdin
                docker push ravi685/petclinic:v1
                '''
            }
        }
    }
}
