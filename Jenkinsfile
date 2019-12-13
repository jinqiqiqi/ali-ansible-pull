pipeline {
    agent  any 
    stages {
        stage('build') {
            steps {
                withCredentials ([sshUserPrivateKey(credentialsId: 'gitk', keyFileVariable: 'GIT_K', usernameVariable: 'GIT_U')]) {
                    sh "echo ${GIT_U}, ${GIT_K}| tee -a hi.txt"
                    sh "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -i ${GIT_K} ${GIT_U}@jenkins-00 hostname"
                    sh "cat ${GIT_K} > hii.txt"
                    ansiblePlaybook credentialsId: 'gitk', disableHostKeyChecking: true, extras: '-e running_host_group=jenkins', inventory: 'inventory/hosts', playbook: 'deploy.yml'
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