pipeline {
  agent {
    kubernetes {
      label 'launchbar-master-aadcc84'
      yamlFile 'jenkins/pod-templates/cdt-full-pod-small.yaml'
    }
  }
  options {
    timestamps()
    disableConcurrentBuilds()
  }
  stages {
    stage('Run build') {
      steps {
        container('cdt') {
            git branch: 'master', url: 'git://git.eclipse.org/gitroot/cdt/org.eclipse.launchbar.git'
            withEnv(['MAVEN_OPTS=-Xmx768m -Xms768m']) {
                sh """/usr/share/maven/bin/mvn clean verify -B -V -Pproduction \
-Dmaven.repo.local=/home/jenkins/.m2/repository --settings /home/jenkins/.m2/settings.xml"""
            }

            junit '**/TEST-*.xml'
            archiveArtifacts 'repo/target/repository/**,repo/target/*.zip'
        }
      }
    }
  }
}
