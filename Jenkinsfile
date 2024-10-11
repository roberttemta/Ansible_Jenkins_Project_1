
pipeline {
    agent { 
        label 'ansible_master_node' // Label of the Ansible control machine in Jenkins
    }

    environment {
        //ANSIBLE_PLAYBOOK_PATH = "/home/ec2-user/Ansible-dev/play.yml"  // This works hell // Path to the playbook on the Ansible control machine
        ANSIBLE_PLAYBOOK_PATH = 'playbooks/play2.yml'  // Path to the playbook on Github 
        ANSIBLE_INVENTORY_PATH = "/home/ec2-user/Ansible-dev/inv.yml"            // Path to the inventory file on the Ansible control machine
        GIT_BRANCH = 'main'
        GIT_URL = 'https://github.com/roberttemta/Ansible_Jenkins_Project_1.git'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Optionally check out the code or playbook repository if playbooks are stored in version control
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
            }
        }

        stage('Testing Ansible Playbook') {
            steps {
                script {
                    
                    sh """ ansible-playbook -i ${ANSIBLE_INVENTORY_PATH} ${ANSIBLE_PLAYBOOK_PATH} --syntax-check"""
                    
                }
            }
        }
        
        stage('Run Ansible Playbook') {
            steps {
                script {
                    /*
                    sh """" ansible-playbook -i ${ANSIBLE_PLAYBOOK_PATH} -b """
                    */
                    
                    sh """ ansible-playbook -i ${ANSIBLE_INVENTORY_PATH} ${ANSIBLE_PLAYBOOK_PATH} """
                    
                }
            }
        }
    }
/*
    post {
        always {
            // Post-build actions, e.g., archive logs, send notifications, etc.
            echo "Ansible playbook execution completed."
        }
    }
    */
}