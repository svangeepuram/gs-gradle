node {
  //def server = Artifactory.server 'svangeepuram.jfrog.io'
  def myGradleContainer = docker.image('gradle:jdk8-alpine')
  myGradleContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
       sh 'cd complete && ./gradlew build'
     }
  }
  stage('test') {
     myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
       sh 'cd complete && ./gradlew test'
     }
  }
  stage('publish') {
    def uploadSpec = """{
      "files": [
        {
          "pattern": "complete/build/libs/gs-gradle-*.jar",
          "target": "gradle-repo/org/svangeepuram/gs-gradle/1.0/gs-gradle-1.0-docker.jar"
        }
     ]
    }"""
    server.upload(uploadSpec)
  }
}
