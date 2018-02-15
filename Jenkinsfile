node('master') {
    stage('Code Checkout') {
        println("Entering Code checkout Stage")
        //checkout scm
    }
    stage('Build') {
        println("Entering Build Stage")
         if (isUnix()) {
                println("UNIX Build Stage")
                sh './gradlew clean build'
            } else {
                println("WIN Build Stage")
                //bat 'call gradlew clean build -x test'
            }
    }
    stage('Test') {
        println("Entering Test Stage")
    }
    stage('Teardown'){
        println("Teardown PCF apps and services")
        try{
            bat 'call cf delete spring-musics -f'
            bat 'call cf delete-service music-database -f'
        } catch(err){
            echo "CF delete app/service failed"
            //println(e.getMessage())
            throw err
            currentBuild.result = 'FAILURE'
        }
    }
    stage('Deploy') {
        when {
            expression {
                currentBuild.result = 'SUCCESS'
            }
        }   
        println("Entering Deploy Stage")
        /*pushToCloudFoundry(
            target: 'api.system.cumuluslabs.io',
            organization: 'nsreekala-PAL-JAN8',
            cloudSpace: 'sandbox',
            credentialsId: 'nanda-pcf',
            selfSigned: true, 
            pluginTimeout: 240, 
            servicesToCreate: [
              [name: 'music-database', type: 'p-mysql', plan: '100mb', resetService: true]
            ],
            envVars: [
              [key: 'FOO', value: 'bar']
            ],
            manifestChoice: [
                manifestFile: 'manifest.yml'
            ]
        ) */
        
        //pushToCloudFoundry cloudSpace: 'sandbox', credentialsId: 'nanda-pcf', organization: 'nsreekala-PAL-JAN8', selfSigned: true, servicesToCreate: [[name: 'music-database', plan: '100mb', resetService: true, type: 'p-mysql']], target: 'api.system.cumuluslabs.io'
        pushToCloudFoundry cloudSpace: 'sandbox', credentialsId: 'nanda-pcf', organization: 'nsreekala-PAL-JAN8', pluginTimeout: 360, selfSigned: true, servicesToCreate: [[name: 'music-database', plan: '100mb', resetService: true, type: 'p-mysql']], target: 'api.system.cumuluslabs.io'
    }
    
    stage('Notify'){
        prinln(currentBuild.result)
        step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: 'nandagopan.gs@cognizant.com 673326@cognizant.com', sendToIndividuals: false])
    }
}
