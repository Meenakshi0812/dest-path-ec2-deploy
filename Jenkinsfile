pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    def clonePath = "/home/ubuntu/${buildNumber}"
                    dir(clonePath) {
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/applicationserver-deploy.git'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    def folderName = "${buildNumber}"
                    def zipFileName = "${folderName}.zip"

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'mkdir -p /var/www/html/${folderName}'"
                    sh "scp -o StrictHostKeyChecking=no -r /home/ubuntu/${buildNumber}/* ubuntu@3.86.97.100:/var/www/html/${folderName}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'cd /var/www/html/${folderName} && zip -r /var/www/html/${zipFileName} *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'unzip -o /var/www/html/${zipFileName} -d /var/www/html'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'ln -sfn /var/www/html/${folderName} /var/www/html/code'"
                }
            }
        }
    }
}
