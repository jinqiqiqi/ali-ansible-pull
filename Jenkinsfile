pipeline {
    agent  any 
    triggers {
        cron('0 */2 * * *')
    }
    stages {
        stage('build') {
            steps {
                withCredentials ([sshUserPrivateKey(credentialsId: 'gitk', keyFileVariable: 'GIT_K', usernameVariable: 'GIT_U')]) {
                    sh "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -i ${GIT_K} ${GIT_U}@jenkins-00 hostname"
                    sh "cat ${GIT_K} | tee ~/.ssh/id_rsa; chmod 600 ~/.ssh/id_rsa"
                    ansiblePlaybook credentialsId: 'gitk', disableHostKeyChecking: true, extras: '-e running_host_group=jenkins', inventory: 'inventory/hosts', playbook: 'deploy.yml'
                    sh "rm -f ~/.ssh/id_rsa"
                }
            
            }
        }
        stage('build2') {
            steps {
                sh 'date; pwd; hostname'
            }
        }
    }
    post {
        success {
            echo "Success result. 1"

        }
        failure {
            echo "Failure result. 2"
        }
        changed {
            echo "Changed result. 3"
        }
        always {
            echo "Always result. 4"
            mailext to: 'qi.jin@supplyframe.cn',
              subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
              body: "${env.BUILD_URL}(${PROJECT_NAME} # ${BUILD_NUMBER}) has result: ${currentBuild.result} (${BUILD_STATUS})",
              attachLog: true,
              compressLog: true
        }
        unstable {
            echo "Unstable result. 5"
        }
        aborted {
            echo "Task aborted. 6"
        }
        unsuccessful {
            echo "Unsuccessful result. 7"
        }
        cleanup {
            echo "Cleanup result. 8"
        }
    }
}
