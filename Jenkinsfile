pipeline {
    agent any

    environment {
         // Using SSH key stored in Jenkins credentials
        EC2_SSH_KEY = credentials('ec2_private_key')
        ANSIBLE_HOST_KEY_CHECKING = 'False'

    }

    stages {
        
        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i /var/lib/jenkins/ansible_quickstart/aws_ec2.yaml /var/lib/jenkins/ansible_quickstart/playbook.yaml -u ubuntu --private-key $EC2_SSH_KEY'
            }
        }
    }
}
