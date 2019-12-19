
pipeline {
    agent  any 
    triggers {
        cron('0 */2 * * *')
    }
    environment {
        BUILD_USER = ''
    }
    options {
        retry(2)
        timeout(time: 15, unit: 'MINUTES')
        timestamps()
    }
    stages {
        stage("start") {
            steps {
                withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                    slackSend teamDomain: 'bigeworld',
                        channel: '#jenkins', 
                        token: slackCredentials, 
                        color: 'good',
                        message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has started."
                }
            }
        }
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
        stage('publish') {
            steps {
                sh 'date; pwd; hostname'
            }
        }
    }
    post {
        success {
            echo "Success result. 1"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'good',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (success): ${currentBuild.currentResult}."
            }
        }
        failure {
            echo "Failure result. 2"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'danger',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (failure): ${currentBuild.currentResult}."
            }
        }
        changed {
            echo "Changed result. 3"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'danger',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (changed): ${currentBuild.currentResult}."
            }
        }
        always {
            echo "Always result. 4"
            mail to: 'qi.jin@supplyframe.cn',
              subject: "Status of pipeline: ${currentBuild.fullDisplayName} [Jenkins-mailer@aliyun-eefocus]",
              body: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result: ${currentBuild.currentResult}."
            // withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
            //     slackSend teamDomain: 'bigeworld',
            //         channel: '#jenkins', 
            //         token: slackCredentials, 
            //         color: 'good',
            //         message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (always): ${currentBuild.currentResult}."
            // }
        }
        unstable {
            echo "Unstable result. 5"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'danger',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (unstable): ${currentBuild.currentResult}."
            }
        }
        aborted {
            echo "Task aborted. 6"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'danger',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (aborted): ${currentBuild.currentResult}."
            }
        }
        unsuccessful {
            echo "Unsuccessful result. 7"
            withCredentials([string(credentialsId: 'slack-token', variable: 'slackCredentials')]) {
                slackSend teamDomain: 'bigeworld',
                    channel: '#jenkins', 
                    token: slackCredentials, 
                    color: 'danger',
                    message: "${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BUILD_URL}) has result (unsuccessful): ${currentBuild.currentResult}."
            }
        }
        cleanup {
            echo "Cleanup result. 8"
        }
    }
}
