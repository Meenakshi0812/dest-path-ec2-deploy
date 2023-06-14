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
                        git branch: 'main', url: 'https://github.com/Meenakshi0812/dest-path-ec2-deploy.git'
                    }

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.226.228.215 'mkdir -p ${destinationPath}${timestamp}'"
                    sh "scp -o StrictHostKeyChecking=no -r ${sourcePath}/* ubuntu@54.226.228.215:${destinationPath}${timestamp}/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.226.228.215 'cd ${destinationPath}${timestamp} && zip -r ${destinationPath}${timestamp}.zip *'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.226.228.215 'unzip -o ${destinationPath}${timestamp}.zip -d ${destinationPath}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.226.228.215 'ln -sfn ${destinationPath}${timestamp} ${destinationPath}code'"
                }
            }
        }
    }
}
