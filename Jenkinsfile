node{
    stage('Checkout'){


        git 'https://github.com/mbenguep/webapp.git'
    }
    stage('Clean Package') {
        echo 'Code Quality'
        withSonarQubeEnv('s1-91') { 
          sh "/opt/maven/bin/mvn clean package"
        }
    }
    stage('SonarQube Analysis') {
        echo 'Code Quality'
        withSonarQubeEnv('s1-91') { 
          sh "/opt/maven/bin/mvn sonar:sonar"
        }
    }
    stage('Copy Artifact to Ansible-Server'){
        // getting maven home path

        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
        execCommand: '''cd /opt/docker/webapp;
        ansible-playbook webdev_image.yml''', execTimeout: 900000000, flatten: false, makeEmptyDirs: false, 
        noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker//webapp', remoteDirectorySDF: false, 
        removePrefix: 'target', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, 
        useWorkspaceInPromotion: false, verbose: false)])

}
    stage('Deploy the onto openshift'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/webapp/inventory /opt/docker/webapp/kube_deploy.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('Create service onto openshift'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/webapp/inventory /opt/docker/webapp/kube_service.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('create route for the final service'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/webapp/inventory /opt/docker/webapp/kube_route.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('Restart the Deployment'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'oc rollout restart deployment mbengue -n deploy-mem', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}
}