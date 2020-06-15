pipeline {
  environment {
    registry = "bds1958/gameoflife"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent {
        label "master"
    }
    tools {
        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "MAVEN_HOME"
    }
  stages {
    stage('Cloning Git') {
            steps {
                    git credentialsId: '29018866-3bbb-431f-b8cb-7028317f0bab', url: 'https://github.com/bds1965/game-of-life.git'
                    //git credentialsId: '720dd959-df22-4b2b-a557-34d49158925c', url: 'https://github.com/bds1965/game-of-life.git'
                }
    }
    stage("mvn build") {
            steps {
                script {
                    // Since unit testing is out of the scope we skip them
                    sh "mvn package -DskipTests=true"
                }
            }
    }
    stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
    }
    stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
    }
stage('Running over ssh') {
            steps{
                script {
                    //sshagent(['bds3']) {
                    //    sh "ssh -o StrickHostKeyChecking=no bds3@bds3-Devops ansible-playbook /home/bds3/docker_project/demo.yml"
                    //sshPublisher(publishers: [sshPublisherDesc(configName: 'bds3@bds3-Devops', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /home/bds3/docker_project/demo.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                 
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'devopscdtool@172.16.1.170', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /devopstools/ansible/demo.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]) 
                } 
            }
    }
}
}
