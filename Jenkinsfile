pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-creds'   // ID of your Jenkins GitHub credentials
        GIT_BRANCH = 'main'
        GIT_URL = 'https://github.com/EniyaAkash24/task.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        rm -rf task || true
                        git clone -b ${GIT_BRANCH} https://$USERNAME:$PASSWORD@github.com/EniyaAkash24/task.git
                        cd task
                        git status
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    cd task
                    npm install
                '''
            }
        }

        stage('Run App') {
            steps {
                sh '''
                    cd task
                    npm start &
                    sleep 10
                '''
            }
        }

        stage('Push Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        cd task
                        git config user.email "jenkins@example.com"
                        git config user.name "Jenkins"
                        git add .
                        git commit -m "Automated commit from Jenkins" || echo "No changes to commit"
                        git push https://$USERNAME:$PASSWORD@github.com/EniyaAkash24/task.git ${GIT_BRANCH}
                    '''
                }
            }
        }
    }
}

