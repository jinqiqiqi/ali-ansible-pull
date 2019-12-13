pipeline {
    agent  any 
    stages {
        stage('preparing') {
            steps {
                withCredentials ([sshUserPrivateKey(credentialsId: 'gitk', keyFileVariable: 'GIT_K', usernameVariable: 'GIT_U')]) {
                    sh "cat ${GIT_K} | tee ~/.ssh/id_rsa; chmod 600 ~/.ssh/id_rsa"
                }
            
            }
        }
        stage('building') {
            steps {
                ansiblePlaybook credentialsId: 'gitk', disableHostKeyChecking: true, extras: '', inventory: 'inventory/hosts', playbook: 'deploy.yml'
            }
        }
        stage('cleaning') {
            steps {
                sh "rm -f ~/.ssh/id_rsa"
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
