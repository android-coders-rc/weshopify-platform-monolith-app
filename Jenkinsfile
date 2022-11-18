pipeline{
    agent any
    tools {
        maven "weshopify-platform-maven"
    }
    stages{
        stage('pull the source code'){
            steps{
                echo 'pulling the source code'
                git branch: 'security', url: 'https://github.com/NarsiMyTeaching/weshopify-platform-monolith-app.git'
                echo '==============================='
                echo 'source code pulling completed'
                echo '==============================='
            }
        }
        stage('building the source code'){
            steps{
                echo 'starting the code build'
                sh 'mvn clean deploy -DskipTests=true'
            }
        }

        stage('copying the docker and jfrog files to ansible'){
            steps{
                echo 'copying the docker and jfrog files'
                sshagent(['Ansible-Machine']){
                    sh 'scp jfrog-server-conn.sh ansible-admin@172.31.0.173:/home/ansible-admin/ci-cd-files'
                    sh 'scp Dockerfile ansible-admin@172.31.0.173:/home/ansible-admin/ci-cd-files'
                    sh 'scp dockerplaybook.yml ansible-admin@172.31.0.173:/home/ansible-admin/ci-cd-files'
                    sh 'ansible-playbook -i ci-cd-files/dockerplaybook.yml'
                }
            }
        }
    }
}
