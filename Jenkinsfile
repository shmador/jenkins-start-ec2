pipeline {
    agent any
    parameters {
        string(name: 'INSTANCE_ID')
        string(name: 'REGION', defaultValue: 'il-central-1')
        string(name: 'CRED_ID', defaultValue: 'e154818c-e253-4c7a-9955-4c0cf6da708c')
        booleanParam(name: 'START', defaultValue: true, description: 'Whether to start or stop this instance')
    }
    stages {
        stage('Set action type') {
            steps {
                script {
                    env.ACTION = params.START ? 'start' : 'stop'
                }
            }
        }
        stage('Start EC2 instance by ID') {
            steps {
                withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: "${CRED_ID}",
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
            ]]) {
                    sh '''
                        aws ec2 ${ACTION}-instances --instance-ids ${INSTANCE_ID} --region ${REGION}
                    '''
                }
            }
        }
    }
}
