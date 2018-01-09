podTemplate(label: 'pod-php',
    namespace: 'jenkins',
    nodeSelector: 'dev',
    containers: [
        containerTemplate(
            name: 'jnlp',
            image: 'xicoria/jkslave:v1.0'
        ),
        containerTemplate(
            name: 'php',
            image: 'php',
            ttyEnabled: true,
            command: 'cat'
        )
    ],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
) {
      node ('pod-php') {
        stage('Verify PHP version from inside pod') {
            container('php') {
                sh ("php -v")
            }
        }	
    }
    stage('Build Docker image') {
        container('jnlp') {
            sh "docker build -t test ."
        }
    }
}
