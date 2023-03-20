pipeline {
    agent any
    stages {
         stage('Checkout') {
             steps{
                 script {
                  // The below will clone your repo and will be checked out to master branch by default.
                  git credentialsId: 'github', url: 'https://github.com/dapiroy/argocd-proj.git'
                  // List all branches in your repo. 
                  sh "git branch -a"
                  // Checkout to a specific branch in your repo.
                  sh "git checkout main"
      
                 }
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
