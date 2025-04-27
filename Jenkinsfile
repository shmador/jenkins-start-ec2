pipeline {
    agent any
    parameters {
        string(name: 'INSTANCE_ID')
        string(name: 'REGION', defaultValue: 'il-central-1')
        string(name: 'CRED_ID', defaultValue: 'aws-cred')
    }
    stages {
        stage('Start EC2 instance by ID') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "${CRED_ID}",
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh '''
                        aws ec2 start-instances --instance-ids ${INSTANCE_ID} --region ${REGION}
                    '''
                }
            }
        }
    }
}
