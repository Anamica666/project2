pipeline {
    agent any
    
    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout source code from GitHub
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Anamica666/project2']]])
            }
        }

        stage('Build') {
            steps {
                sh 'make build'
            }
        }

        stage('Deploy to Apache Server') {
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'apache2', 
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'rm -r /var/www/html , cp -r * /var/www/html',
                                        execTimeout: 120000,
                                        sourceFiles: '**/*index.html', 
                                        remoteDirectory: '/var/www/html',
                                        cleanRemote: false
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
