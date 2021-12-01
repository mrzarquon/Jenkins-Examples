pipeline {
    agent any

    stages {
        stage('Install Snyk') {
            steps {
                sh '''
                wget -q "https://static.snyk.io/cli/latest/snyk-linux"
                wget -q "https://static.snyk.io/cli/latest/snyk-linux.sha256"
                if sha256sum -c snyk-linux.sha256; then
                  mv snyk-linux ./snyk
                  chmod +x ./snyk
                  SNYK_VERSION=$(./snyk --version | cut -d' ' -f1)
                  echo "Snyk Version: ${SNYK_VERSION} installed successfully"
                else
                  echo "Snyk Binary Download failed, exiting"
                  exit 1
                fi
                '''
            }
        }

        stage('Check Snyk Status') {
            steps {
            withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN')]) {
                        sh '''
                        echo "token = ${SNYK_TOKEN}"
                        wget -q --header "Authorization: token $SNYK_TOKEN" https://snyk.io/api/v1/user/me -O -
                        ./snyk auth "${SNYK_TOKEN}"
                        '''
                }
            }
        }
        
        stage('Run something with snyk') {
            steps {
                sh '''
                    ./snyk --version
                    ./snyk test --debug
                '''
            }
        }
    }
}