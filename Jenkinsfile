pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input message: 'Lanjutkan ke tahap Deploy?', 
                    parameters: [choice(choices: ['Proceed', 'Abort'], description: 'Pilih salah satu', name: 'userChoice')]
                    if (userInput == 'Abort') {
                        error('Eksekusi pipeline dihentikan.')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                sleep(time: 60, unit: 'SECONDS')
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}