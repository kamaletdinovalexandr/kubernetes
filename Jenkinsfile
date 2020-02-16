pipeline {
    agent any
    environment {
        registry = "kamaletdinovalexandr/kuber-test"
        GOCACHE = "/tmp"
        ImageName = "go-hello-world"
    }
    stages {
        stage('Get Tag') {
            steps {
                sh "git rev-parse --short HEAD > commit-id"
                imageTag = readFile('commit-id').trim()
            }
        }
        stage('Build && push') {       
            sh "docker build -t ${ImageName}:${imageTag} ."
            sh "docker push ${ImageName}"     
        }
        stage ('Deploy') {
            steps {
                script{
                    def image_id = registry + ":$BUILD_NUMBER"
                    sh "ansible-playbook  playbook.yml --extra-vars \"image_id=${image_id}\""
                }
            }
        }
    }
}
