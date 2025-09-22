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
                    usernamePassword(
                        credentialsId: 'SP-appliance-API',
                        usernameVariable: 'ORCHESTRATOR_HOST', // 自定义用户名环境变量名
                        passwordVariable: 'ORCH_TOKEN'  // 自定义密码环境变量名
                    )
                ]) {
                    // 在这个块内的所有步骤都可以访问到上面定义的环境变量
                    echo "Username is $MY_USERNAME"
                    echo "Password is $MY_PASSWORD" // Jenkins 会自动用 **** 屏蔽值
                  
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
