pipeline {
    agent any

    stages {
        stage('Docker Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerHub-meilecor', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            export DOCKER_HOST=tcp://172.17.0.6:2375
                            docker login -u $USERNAME -p $PASSWORD
                            docker buildx build --platform linux/amd64 -t meilecor/node-web-app:latest . --push
                        '''
                    }
                }
            }
        }
        stage('Trigger Render Deployment') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'RenderDeployKey', variable: 'KEY')]) {
                        sh "curl https://api.render.com/deploy/$KEY"
                    }
                }
            }
        }        
    }
}
