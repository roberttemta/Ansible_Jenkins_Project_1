
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
        ZIP_FILE_NAME = "playbooks_${BUILD_ID}.zip"
        JFROG_CRED = 'jfrog_Crendentials'
        ARTIFACTPATH = '/home/ec2-user/workspace/Ansible_Pipeline'
        ARTIFACTORY_URL = 'http://ec2-34-238-157-80.compute-1.amazonaws.com:8081/artifactory'
        REPO = 'Playbooks_1'
        ARTIFACTTARGETPATH = "playbooks_${BUILD_ID}.zip"
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

        stage('Package Artifact in zip file') {
            steps {
                script {
                    /*
                    sh """" ansible-playbook -i ${ANSIBLE_PLAYBOOK_PATH} -b """
                    */
                    
                    sh """zip -r ${ZIP_FILE_NAME} . """
                    
                }
            }
        }

        stage('Upload zip to Jfrog') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${JFROG_CRED}",
                 usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                    script {
                // Upload the artifact using curl
                sh '''
                    curl -u $ARTIFACTORY_USER:$ARTIFACTORY_PASSWORD \
                         -T $ARTIFACTPATH \
                         ${ARTIFACTORY_URL}/${REPO}/${ARTIFACTTARGETPATH}
                '''
                    }
                 }
            }
    }
    }
    
}