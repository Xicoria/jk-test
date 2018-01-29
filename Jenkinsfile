podTemplate(label: 'microservices',
    namespace: 'jenkins',
    nodeSelector: 'dev',
    containers: [
        containerTemplate(
            name: 'jnlp',
            image: 'xicoria/jkslave:v3.0'
        ),
        containerTemplate(
            name: 'php',
            image: 'php:7-fpm',
            ttyEnabled: true,
            command: 'cat',
            envVars: [
				envVar(key: 'REDIS_HOST', value: 'redis'),
				envVar(key: 'REDIS_PORT', value: '6379')
			]
        )
    ],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
) {
  node ('microservices') {
    stage('Credentials') {
      withCredentials([string(credentialsId: 'ID_SECRET', variable: 'ID_SECRET')]) {
	    container('php') {
          sh 'printenv'
          def vari = credentials('ID_SECRET')
          echo "${vari}"
        }
      }
    }
  }
}
