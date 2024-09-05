pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'Replace-it',
                    url: 'https://github.com/deadlyaman0000/cicd-end-to-end.git',
                    branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    sh '''
                    echo 'Build Docker Image'
                    docker build -t darkneth/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the Docker Image') {
            steps {
                script {
                    sh '''
                    echo 'Push to Docker Hub'
                    docker push darkneth/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM') {
            steps {
                git credentialsId: 'replace-it',
                    url: 'https://github.com/deadlyaman0000/jenkins.git',
                    branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'replace-it', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        git config --global user.email "your-email@example.com"
                        git config --global user.name "Your Name"
                        cat deploy.yaml
                        sed -i "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/deadlyaman0000/jenkins.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
