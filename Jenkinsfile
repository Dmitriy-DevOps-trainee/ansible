node {
    
    stage('Clone repository') {
        checkout scm
    }
    
    stage('Install roles') {
        sh "ansible-galaxy install --force --role-file requirements.yml"
    }

    stage('Run ansible playbook') {
        ansiblePlaybook(inventory: 'inventory', playbook: 'playbook.yml')
    }
    
    stage('Integration test') {
        sh """
        response=\$(ssh -i ~/.ssh/ansible root@192.168.56.127 curl -s -o /dev/null -w "%{http_code}" localhost:80)
        if [ "\$response" != "200" ]
        then
          exit 1
        else
          echo "Integration test passed"
        fi
        """
    }
} 
