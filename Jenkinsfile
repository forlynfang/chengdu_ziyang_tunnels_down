pipeline {
    agent any
    stages {
        stage('Enable virtual environment pyats') {
            steps {
                echo 'Setup PYATS environment'
                sh 'python3 -m venv pyats'
                sh 'source pyats/bin/activate'
            }
        }
        stage('List files cloned from Git') {
            steps {
                echo 'Confirm required files are cloned'
                sh 'ls -la'                
            }
        }
        stage('Install dotenv') {
            steps {
                sh 'pip3 install dotenv'
            }
        }
        stage('Run the Python script for Chengdu and Ziyang') {
            steps {
                echo 'Activate Python script to check tunnels down'
                // 使用 withCredentials 指令将凭证注入到环境变量
                
                withCredentials([
                    // 绑定用户名密码凭证到两个环境变量：MY_USERNAME 和 MY_PASSWORD
                    string(
                        credentialsId: 'SP-API-TK',
                        variable: 'ORCH_TOKEN' // 自定义密钥环境变量名
                    ),
                    string(
                        credentialsId: 'Jenkins_webhook',
                        variable: 'TEAMS_WEBHOOK' // 自定义密钥环境变量名
                    )
                ]) {
                    // 在这个块内的所有步骤都可以访问到上面定义的环境变量
                    echo "API Token is $ORCH_TOKEN"
                    // 执行你的 Python 脚本
                    sh 'python3 chengdu_ziyang_tunnels.py'
                }               
            }
        }     
    }
    post {
        always {
            cleanWs(cleanWhenNotBuilt: true,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
        }
    }
}
