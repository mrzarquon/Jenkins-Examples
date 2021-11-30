pipeline {
    agent { label "aws-spot" }
    stages{
        stage('Validate that we have a working snyk token') {
                withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN_VAR')]) {
                    sh '''
                    echo "snyk token var = ${SNYK_TOKEN_VAR}"
                    echo "token = ${SNYK_TOKEN}"
                    wget -q --header "Authorization: token $SNYK_TOKEN" https://snyk.io/api/v1/user/me -O -
                    '''
                
            }
        }
    }
}