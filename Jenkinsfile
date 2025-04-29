pipeline {
    agent any
    parameters {
        string(name: 'INSTANCE_ID')
        string(name: 'REGION', defaultValue: 'il-central-1')
        string(name: 'CRED_ID', defaultValue: '05e6adcb-0128-4101-bca4-350064a1de3d')
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
                    script {
                        def EC2_STATE = sh(
                            script: "aws ec2 describe-instances --instance-ids ${env.INSTANCE_ID} --query 'Reservations[0].Instances[0].State.Name' --output text",
                            returnStdout: true
                        ).trim()
			echo "${EC2_STATE}
                        if (EC2_STATE == 'stopped') {
                            sh "aws ec2 start-instances --instance-ids ${INSTANCE_ID} --region ${REGION}"
                        }
                    }
                }
            }
        }
    }
}

