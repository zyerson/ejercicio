<<<<<<< HEAD
#!/usr/bin/env groovy

properties([
  [$class: 'GithubProjectProperty', displayName: 'Simple Application', projectUrlStr: 'https://github.com/alecharp/simple-app/'],
  buildDiscarder(logRotator(artifactNumToKeepStr: '5', daysToKeepStr: '15'))
])

<<<<<<< HEAD
def dockerImageBuild = 'alecharp/maven-build-tools:a43bb39'
docker.image(dockerImageBuild).inside {
  stage('Checkout') {
    checkout scm
    short_commit = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
    currentBranch = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
    currentBuild.description = "#${short_commit}"
  }

  stage('Build') {
    sh 'mvn clean package -Dmaven.test.skip=true'
    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
=======
node {
  stage('Checkout') {
    checkout scm
    commit = sh(returnStdout:true, script:'git rev-parse --short HEAD').trim()
    currentBranch = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
    currentBuild.description = "#${commit}"
  }

  stage('Build') {
    mvn 'clean package -Dmaven.test.skip=true'
    archiveArtifacts 'target/*.jar'
>>>>>>> feature/build-in-jenkins-with-nodes
    stash name: 'docker', includes: 'src/main/docker/Dockerfile, target/*.jar'
  }
}

stage('Tests') {
  parallel 'Unit tests': {
<<<<<<< HEAD
    docker.image(dockerImageBuild).inside {
      checkout scm
      sh 'mvn clean test'
      junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
    }
  }, 'Integration tests': {
    docker.image(dockerImageBuild).inside {
      checkout scm
      sh 'mvn clean test-compile failsafe:integration-test'
=======
    node {
      checkout scm
      mvn 'clean test'
      junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
    }
  }, 'Integration tests': {
    node {
      checkout scm
      mvn 'clean test-compile failsafe:integration-test'
>>>>>>> feature/build-in-jenkins-with-nodes
      junit allowEmptyResults: true, testResults: 'target/failsafe-reports/*.xml'
    }
  }
}

stage('Build Docker img') {
  node {
    unstash 'docker'
<<<<<<< HEAD
    image = docker.build("alecharp/simple-app:${short_commit}", '-f src/main/docker/Dockerfile .')
  }
}

stage('Validate Docker img') {
=======
    image = docker.build("alecharp/simple-app:${commit}", "-f src/main/docker/Dockerfile .")
  }
}

stage('Validate Docker container') {
>>>>>>> feature/build-in-jenkins-with-nodes
  node {
    container = image.run('-P')
    ip = container.port(8080)
  }
<<<<<<< HEAD
  try {
    input message: "Is http://${ip} ok?", ok: 'Publish'
=======

  try {
    input message: "See http://${ip}. Is it ok?", ok: 'Publish'
>>>>>>> feature/build-in-jenkins-with-nodes
  } finally {
    node { container.stop() }
  }
}

stage('Publish Docker img') {
  node {
    docker.withRegistry('http://localhost:5000') {
<<<<<<< HEAD
      image.push "${short_commit}"
=======
      image.push "${commit}"
>>>>>>> feature/build-in-jenkins-with-nodes
      if ('master'.equals(currentBranch)) {
        milestone label: 'docker-image-latest'
        image.push "latest"
=======
pipeline {
  agent any
  
  stages {
    stage('Build') {
      tools {
        maven '3.3.9'
        jdk '8u112'
      }
      
      steps {
        sh 'mvn clean verify'
      }
      
      post {
        success {
          archiveArtifact 'target/simple-app.jar'
        }
>>>>>>> feature/pipeline-test
      }
    }
  }
}
<<<<<<< HEAD

if ('master'.equals(currentBranch)) {
  stage('Release') {
    milestone label: 'release only the latest build'
    def release = input message: 'Choose release parameters', ok: 'Done',
      parameters: [
        string(defaultValue: '', description: 'Version for the release', name: 'version'),
        string(defaultValue: '', description: 'Next development version', name: 'nextVersion')
      ]
    docker.image(dockerBuildImage).inside('-v /Users/adrien/.m2:/home/build/.m2') {
      checkout scm
      sh 'git config user.name Jenkins && git config user.email no-mail@example.com'
      sh "mvn clean release:prepare release:perform -B" +
        (release?.version?.trim() ? " -DreleaseVersion=" + release.version : '') +
        (release?.nextVersion?.trim() ? " -DdevelopmentVersion=" + release.nextVersion : '')
    }
  }
}
<<<<<<< HEAD
=======

def mvn(def goals) {
  withMaven(jdk: '8u112', maven: '3.3.9') {
    sh "mvn ${goals}"
  }
}
>>>>>>> feature/build-in-jenkins-with-nodes
=======
>>>>>>> feature/pipeline-test
