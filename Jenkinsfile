
import java.time.*
import java.text.SimpleDateFormat

pipeline {
    agent none
    stages {
        stage ("init"){
            agent any
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        env.TIMESTAMP = new SimpleDateFormat("yyyy-MM-dd'T'HH-mm-ssXXX").format(new Date())
                        env.TAG = sh (
                            script: 'git -c \'versionsort.suffix=-\' ls-remote --exit-code --refs --sort=\'version:refname\'  --tags  https://$GIT_PASSWORD@github.com/serignemodou/devops.git | tail -n1 | sed \'s/.*\\///; s/\\^{}//\'',
                            returnStdout: true
                        ).trim()
                        int len = ((env.TAG).length())
                        env.VERSION = env.TAG.substring(0,len)
                        if (env.TAG ==""){
                            env.TAG  = "0.0.0"
                        }
                    }
                }
            }
        }
        stage ('pipeline-ci-cd'){
            parallel {
                stage('build and push image main') {
                    when {
                        branch 'main'
                    }
                    steps {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-token', passwordVariable: 'password', usernameVariable: 'username')]) {
                       sh 'docker login -u $username -p $password'
                     }
                        sh 'docker build --build-arg ENVIRONMENT=dev  --no-cache -t smodou/devops/nginx01:1.0.0 .'
                        sh 'docker tag smodou/devops/nginx01:1.0.0 smodou/devops/nginx01:latest'
                        sh 'docker push smodou/devops/nginx01:1.0.0
                        sh 'docker rmi -f smodou/devops/nginx01:1.0.0 || true'
                        sh 'docker rmi -f $(docker images -q --filter "dangling=true") || true'
                    }
                }
            }
        }
    }
}
