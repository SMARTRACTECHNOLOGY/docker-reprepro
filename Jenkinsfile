node {

  stage 'checkout'
  checkout scm

  def tag = (env.BRANCH_NAME == 'master') ? env.BUILD_NUMBER : "snapshot-${env.BUILD_NUMBER}"

  stage 'docker build'

  def dockerImage = docker.build "smartcosmos/reprepro:${tag}"

  if (env.BRANCH_NAME == 'master') {
    stage 'push images'

    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
      dockerImage.push('latest')
    }
  }

  // remove images to save space
  sh "docker rmi ${dockerImage.id}"

}
