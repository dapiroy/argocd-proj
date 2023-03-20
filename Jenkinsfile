pipeline {
    agent any
    stages {
         stage('Checkout'){
             steps {
                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dapiroy/argocd-proj.git']])
             }
         }

         stage('Update GIT') {
             steps{
                 script {
                     catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                      withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email dapiroy007.com"
                        sh "git config user.name dapiroy"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+dapiroy/regapp.*+dapiroy/regapp:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/argocd-proj.git HEAD:main"
                      }
                     }
                 }
             }
         }
    }
}
