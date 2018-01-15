pipeline {
  agent any
  stages {
    stage('set version number') {
      steps {
        git 'https://github.com/kranthiB/hello-karyon-rxnetty.git'
        sh 'sed -i \\\'/^  release/s/5/\'+"${env.BUILD_NUMBER}"+\'/\\\' build.gradle'
      }
    }
    stage('build') {
      steps {
        sh './gradlew clean packDeb'
      }
    }
    stage('publish to aptly') {
      steps {
        sh 'aptly repo add hello-karyon-rxnetty build/distributions/*.deb'
        sh 'aptly publish update -skip-signing=true trusty'
      }
    }
    stage('archive') {
      steps {
        archiveArtifacts 'build/distributions/*.deb'
      }
    }
  }
}