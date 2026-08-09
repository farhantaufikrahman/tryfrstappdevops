pipeline {
    agent { label 'kyrie-han' }
    stages {
        stage('Checkout'){steps{checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '73bb9939-0ebe-4556-8fac-2f7af356ff63', url: 'https://github.com/farhantaufikrahman/tryfrstappdevops']])}}
        stage('Build') {
            steps { 
                dir('apps') {
                    sh 'npm i'
                }
            }
        }
        stage('Testing') {
            steps {
                dir('apps') {
                    sh 'npm test'
                    sh 'npm run test:coverage'
                }
            }
        }
        stage('Code Review') {
            steps {
               sh '''
                    cd apps
                    sonar-scanner \\
                    -Dsonar.projectKey=simple-project \\
                    -Dsonar.sources=. \\
                    -Dsonar.host.url=http://172.23.7.101:9000 \\
                    -Dsonar.login=sqp_5bc1ba8094f75324edb18640431c269f918af8a8'''

            }
        }
        stage('Approval') {
            steps {
                input message: 'Check Sonarqube server'
                ok: 'Apporve'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose -f docker.compose.yml up -d --build'
            }
        }
    }
}