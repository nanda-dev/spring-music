node('master') {
    stage('Code Checkout') {
        println("Entering Code checkout Stage")
        checkout scm
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
    stage('Deploy') {
        println("Entering Deploy Stage")
        pushToCloudFoundry(
            target: 'api.system.cumuluslabs.io',
            organization: 'nsreekala-PAL-JAN8',
            cloudSpace: 'sandbox',
            credentialsId: 'nanda-pcf2',
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
        ) 
    }
}
