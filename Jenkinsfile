pipeline {
    agent any

    environment {
        // Using SSH key stored in Jenkins credentials
        EC2_SSH_KEY = credentials('ec2_private_key')
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        EMAIL_RECIPIENT = 'prakharsingh1932003@gmail.com'
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i /var/lib/jenkins/ansible_quickstart/aws_ec2.yaml /var/lib/jenkins/ansible_quickstart/playbook.yaml -u ubuntu --private-key $EC2_SSH_KEY'
            }
        }
    }

    post {
        success {
            script {
                def buildNumber = currentBuild.number
                def user = currentBuild.getBuildCauses()[0]?.userId ?: 'Unknown'
                def buildTime = new Date(currentBuild.getStartTimeInMillis()).format('yyyy-MM-dd HH:mm:ss')

                def emailSubject = "Build #${buildNumber} completed"
                def emailBody = """
                    The build number ${buildNumber} has completed successfully.

                    Triggered by user: ${user}

                    Build completed at: ${buildTime}

                    Git commit ID: ${env.GIT_COMMIT}
                """

                emailext(
                    to: EMAIL_RECIPIENT,
                    subject: emailSubject,
                    body: emailBody
                )
            }
        }

        failure {
            script {
                def buildNumber = currentBuild.number
                def user = currentBuild.getBuildCauses()[0]?.userId ?: 'Unknown'
                def buildTime = new Date(currentBuild.getStartTimeInMillis()).format('yyyy-MM-dd HH:mm:ss')

                def emailSubject = "Build #${buildNumber} failed"
                def emailBody = """
                    The build number ${buildNumber} has failed.

                    Triggered by user: ${user}

                    Build failed at: ${buildTime}

                    Git commit ID: ${env.GIT_COMMIT}
                """

                emailext(
                    to: EMAIL_RECIPIENT,
                    subject: emailSubject,
                    body: emailBody
                )
            }
        }
    }
}
