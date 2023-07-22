
import java.time.*
import java.text.SimpleDateFormat

pipeline {
    agent none
    stages {
        stage ("init"){ 
          agent any
            steps {
              //  script {
                   // withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                     //   env.TIMESTAMP = new SimpleDateFormat("yyyy-MM-dd'T'HH-mm-ssXXX").format(new Date())
                    //}
               // }
                git credentialsId: 'github-token', branch: "main", url: 'https://github.com/serignemodou/devops.git'
            }
        }
        stage ('pipeline-ci-cd'){
            parallel {
                stage('build and push image main') {
                    when {
                        branch 'main'
                    }
                    agent {
                      label 'dockeragent'  
                    }
                    steps {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-token', passwordVariable: 'password', usernameVariable: 'username')]) {
                       sh 'docker login -u $username -p $password'
                     }
                        sh 'docker --version'
                      //  sh 'docker build --build-arg ENVIRONMENT=dev  --no-cache -t smodou/devops/nginx01:1.0.0 .'
                       // sh 'docker tag smodou/devops/nginx01:1.0.0 smodou/devops/nginx01:latest'
                       // sh 'docker push smodou/devops/nginx01:1.0.0'
                       // sh 'docker rmi -f smodou/devops/nginx01:1.0.0 || true'
                       // sh 'docker rmi -f $(docker images -q --filter "dangling=true") || true'
                    }
                }
            }
        }
    }
}
