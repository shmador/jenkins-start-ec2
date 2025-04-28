pipeline {
    agent any
    parameters {
        string(name: 'INSTANCE_ID')
        string(name: 'REGION',    defaultValue: 'il-central-1')
        string(name: 'CRED_ID',   defaultValue: 'aws-imtech')
    }
    stages {
        stage('Get EC2 State') {
            steps {
                script {
                    EC2_STATE = sh(
                        script: "aws ec2 describe-instances --instance-ids ${params.INSTANCE_ID} --region ${params.REGION} --query 'Reservations[0].Instances[0].State.Name' --output text",
                        returnStdout: true
                    ).trim()
                    echo "EC2_STATE = ${EC2_STATE}"
                }
            }
        }

        stage('Start/Stop EC2 instance by ID') {
            steps {
                script {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: params.CRED_ID,
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        if (EC2_STATE == 'running') {
                            sh "aws ec2 stop-instances --instance-ids ${params.INSTANCE_ID} --region ${params.REGION}"
                        }
                        else if (EC2_STATE == 'stopped') {
                            sh "aws ec2 start-instances --instance-ids ${params.INSTANCE_ID} --region ${params.REGION}"
                        }
                        else {
                            echo "Instance is in state: ${EC2_STATE}"
                        }
                    }
                }
            }
        }
    }
}

