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
    ]
) {
    node ('pod-php') {

        stage 'Execute inside container'
        container('php') {

          sh ("php -v")

        }
    }
}
