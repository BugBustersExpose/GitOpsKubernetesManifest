node {
  def app

  stage('Clone repository') {

    checkout scm
  }

  stage('Update GIT') {

    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'Failure') {
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
          // def encodedPassword = URLEncoder.encode("$GIT_PASSWORD", 'UTF-8')
          sh "git config user.email BugBustersExpose@proton.me"
          sh "git config user.name BugBustersExpose"
          // sh "git switch master"
          sh "cat deployment.yaml"
          sh "sed -i 's;bugbustersexpose/test.*;bugbustersexpose/test:${DOCKERTAG};g' deployment.yaml"
          sh "cat deployment.yaml"
          sh "git add ."
          sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
          sh "git config credential.helper 'store --file .git/credentials'"
          sh "echo https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com > .git/credentials"
          sh "git push origin HEAD:main"
          sh "rm .git/credentials"
        }
      }
    }
  }
}
