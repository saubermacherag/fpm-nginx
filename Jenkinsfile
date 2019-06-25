pipeline {
    agent {
        node {
            label 'amazon-base'
        }
    }

    options {
        timestamps()
    }

    stages {

        stage('build docker image') {
            steps {
                script {
                    withCredentials([
                        string(variable: 'AWS_ACCOUNT_ID', credentialsId: 'aws_account_id')
                    ]) {
                        docker.build('pinkrobin/ext/cakephp')
                        docker.withRegistry('https://${AWS_ACCOUNT_ID}.dkr.ecr.eu-central-1.amazonaws.com', 'ecr:eu-central-1:dockerman') {
                            docker.image('pinkrobin/ext/cakephp').push('php7.3.6-nginx1.15.12')
                        }
                    }
                }
            }
        }
    }
}
