pipeline {
    agent any
    environment {
        branch = 'main'
        scmUrl = 'git@github.com:saintenvoy/jenkins-beginning.git'

    }
    stages {
        stage('checkout git') {
            steps {
                echo "1. checkout git"
                git branch: branch, url: scmUrl
            }
        }

        stage('docker build') {
            steps {
                echo "2. docker build"
                sh 'docker build -t registry.cn-hangzhou.aliyuncs.com/huorepo/jenkins-demo:${BUILD_NUMBER} .'
            }
        }

        stage ('docker push') {
            steps {          
                echo "3. docker push"
                sh 'docker login registry.cn-hangzhou.aliyuncs.com -u 274020988@qq.com -p 778825hh'
                sh 'docker push registry.cn-hangzhou.aliyuncs.com/huorepo/jenkins-demo:${BUILD_NUMBER}'
            }
        }

        stage('YAML') {
            echo "5. Change YAML File Stage"
            sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
            sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' k8s.yaml"
        }

        stage('Deploy to rancher') {
            steps {
            echo "4. Deploy to rancher"
            sh "kubectl apply -f deploy.yaml"
            
            }
            
        }

    }

}
