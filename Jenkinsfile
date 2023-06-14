pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def sourcePath = "/home/ubuntu/${timestamp}"
                    def destinationPath = "/var/www/html/"

                    dir(sourcePath) {
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/applicationserver-deploy.git'
                    }

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'mkdir -p ${destinationPath}${timestamp}'"
                    sh "scp -o StrictHostKeyChecking=no -r ${sourcePath}/* ubuntu@3.86.97.100:${destinationPath}${timestamp}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'cd ${destinationPath}${timestamp} && zip -r ${destinationPath}${timestamp}.zip *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'unzip -o ${destinationPath}${timestamp}.zip -d ${destinationPath}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.86.97.100 'ln -sfn ${destinationPath}${timestamp} ${destinationPath}code'"
                }
            }
        }
    }
}
