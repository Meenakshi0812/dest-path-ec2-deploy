pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                script {
                    def clonePath = "/home/ubuntu/build_${env.BUILD_NUMBER}"
                    dir(clonePath) {
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/dest-path-ec2-deploy.git'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def folderName = "build_${env.BUILD_NUMBER}"
                    def zipFileName = "${folderName}.zip"
                    def destinationPath = "/var/www/html/"

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.82.155.56 'mkdir -p ${destinationPath}${folderName}'"
                    sh "scp -o StrictHostKeyChecking=no -r /home/ubuntu/build_${env.BUILD_NUMBER}/* ubuntu@3.82.155.56:${destinationPath}${folderName}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.82.155.56 'cd ${destinationPath}${folderName} && zip -r ${destinationPath}${zipFileName} *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.82.155.56 'unzip -o ${destinationPath}${zipFileName} -d ${destinationPath}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.82.155.56 'ln -sfn ${destinationPath}${folderName} ${destinationPath}code'"
                }
            }
        }
    }
}
