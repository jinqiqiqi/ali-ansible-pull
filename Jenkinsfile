pipeline {
    agent  any 
    stages {
        stage('build') {
            steps {
                withCredentials ([sshUserPrivateKey(credentialsId: 'gitk', keyFileVariable: 'GIT_K', usernameVariable: 'GIT_U')]) {
                    sh "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -i ${GIT_K} ${GIT_U}@jenkins-00 hostname"
                    sh "cat ${GIT_K} | tee ~/.ssh/id_rsa; chmod 600 ~/.ssh/id_rsa"
                    ansiblePlaybook credentialsId: 'gitk', disableHostKeyChecking: true, extras: '-vvv -e running_host_group=jenkins', inventory: 'inventory/hosts', playbook: 'deploy.yml'
                    sh "rm -f ~/.ssh/id_rsa"
                }
                sh 'hostname'
                sh 'uptime'
                sh 'pwd'
                sh 'ls -la'
                sh 'echo ${KEY}'
            
            }
        }
        stage('build2') {
            steps {
                sh 'date; pwd'
            }
        }
    }
    post {
        success {
            echo "Success result."
        }
        failure {
            echo "Failure result."
        }
        changed {
            echo "Changed result."
        }
        always {
            echo "Always result."
        }
        unstable {
            echo "Unstable result."
        }
    }
}
