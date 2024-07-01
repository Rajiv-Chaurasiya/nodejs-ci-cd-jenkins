pipeline {
    agent any
    stages {
        stage('git cloning') {
            steps {
                git branch: 'master', credentialsId: 'git', url: 'https://github.com/Rajiv-Chaurasiya/nodejs-ci-cd-jenkins.git'
            }
        }
        stage('install dependecies') {
            steps {
                sh 'npm install'
            }
        }
        stage('scanner') {
            steps {
                script{
                    def sonarHome = tool 'sonar'
                    withSonarQubeEnv('sonar') {
                        sh "${sonarHome}/bin/sonar-scanner"
                    }    
                }
            }
        }
        stage('image') {
            steps {
                sh 'docker system prune -a'
                sh 'docker build -t rajiv84ia/nodejs-nodetodo .'
            }
        }
        stage('docker hub') {
            steps {
                withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
                    sh 'docker login -u rajiv84ia -p ${docker}'
                    sh 'docker push rajiv84ia/nodejs-nodetodo'
                } 
            }
        }
    }
}
