pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                script {
                    def clonePath = "/home/ubuntu/${env.BUILD_NUMBER}"
                    dir(clonePath) {
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/applicationserver-deploy.git'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def folderName = "code_${env.BUILD_NUMBER}"
                    def zipFileName = "${folderName}.zip"
                    def destinationPath = "/var/www/html/"

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'mkdir -p ${destinationPath}${folderName}'"
                    sh "scp -o StrictHostKeyChecking=no -r /home/ubuntu/${env.BUILD_NUMBER}/* ubuntu@3.86.97.100:${destinationPath}${folderName}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'cd ${destinationPath}${folderName} && zip -r ${destinationPath}${zipFileName} *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'unzip -o ${destinationPath}${zipFileName} -d ${destinationPath}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'ln -sfn ${destinationPath}${folderName} ${destinationPath}code'"
                }
            }
        }
    }
}
