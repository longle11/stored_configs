Mở visudo -> Cấu hình user dùng để chạy pipeline mà không cần pass
<user> ALL=(ALL:ALL) NOPASSWD: ALL

pipeline {
    agent none
    environment {
        CI_COMMIT_SHORT_SHA = ""
        CI_COMMIT_TAG = ""
        CI_PROJECT_NAME = ""
        IMAGE_VERSION = ""
        USER_PROJECT = "abc"
        CURRENT_DIR = ""
        APP_USER = "longle"
        PERM_SCRIPT = "sudo chown -R ${APP_USER}. ${PROJECT_NAME}"
        KILL_OLD_PROCESS = "sudo kill -9 \$(ps -ef | grep ${PROJECT_NAME} | grep -v grep | aws '{print \$2}') "
        DEPLOY_PROJECT = 'sudo su ${APP_USER} -c "cd ${folder_deploy}; chạy lệnh build > nohup.out 2>&1 &"'
    }

    stages {
        stage('get information from project') {
            agent {
                label 'IP máy ảo'
            }
            steps {
                script {
                  CI_PROJECT_NAME = sh(script: "git remote show origin -n | grep Fetch | cut -d'/' -f5 | cut -d'.' -f1", returnStdout: true).trim()   
                  def CI_COMMIT_HASH = sh(scipt: "git rev-parse HEAD", returnStdout: true).trim()
                  CI_COMMIT_SHORT_SHA = CI_COMMIT_HASH.take(8)
                  CI_COMMIT_TAG = sh(script: "git describe --tags --exact-match ${CI_COMMIT_HASH}", returnStdout: true).trim()
                  IMAGE_VERSION = "${CI_PROJECT_NAME}:${CI_COMMIT_SHORT_SHA}_${CI_COMMIT_TAG}"
                  CURRENT_DIR = sh(script: "pwd", returnStdout: true).trim()
                }
            }
        }

        stage('build') {
            agent {
                label ''
            }
            steps {
                script {
                    sh(script: """ docker build -t $IMAGE_VERSION . """, label: "")
                }
            }
        }

        stage('deploy') {
            agent {
                label: ''
            }
            steps {
                scripts {
                    sh(script: """ sudo su ${USER_PROJECT} -c "docker rm -f ${CI_PROJECT_NAME}; docker run --name ${CI_PROJECT_NAME} -dp 5214:5214 ${IMAGE_VERSIOn}" """, label: '')
                }
            }
        }
    }
}

stage {
    steps {
        script {
            try {
                timeout(time: 5, unit: 'MINUTES') {
                    env.useChoice = input message: "Can it be deployed?",
                        parameters: [choice(name: deploy, choices: 'no\nyes', description: 'Choose "yes" if you want to deploy')]
                }

                if(env.useChoice == 'yes') {
                    sh(script: """ """, label: "")
                    sh(script: """ """, label: "")
                    sh(script: """ """, label: "")
                    sh(script: """ """, label: "")
                    sh(script: """ """, label: "")
                } else {
                    echo "Do not confirm the deployment!"
                }
            } catch (Exception err) {
                
            }
        }
    }
}
