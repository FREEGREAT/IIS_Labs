pipeline {
    agent any

    stages {
        stage('Installing NPM (if needed)') {
            steps {
                sh '''
                    if ! command -v npm &> /dev/null
                    then
                        sudo apt-get update
                        sudo apt-get install -y npm
                    else
                        echo "npm already installed"
                    fi
                '''
            }
        }

        stage('Installing dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Running dev server (test only)') {
            steps {
                sh '''
                    nohup npm run dev > dev.log 2>&1 &
                    DEV_PID=$!
                    sleep 10
                    kill $DEV_PID
                    echo 'Dev server started and stopped successfully.'
                '''
            }
        }

        stage('SonarQube analysis') {
            steps {
                script {
                    def scannerHome = tool 'Lab11_sonar_scanner'
                    withSonarQubeEnv('Lab11_sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }


    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
