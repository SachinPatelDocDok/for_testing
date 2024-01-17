podTemplate(label: 'maven-selenium', containers: [
    containerTemplate(name: 'maven-chrome', image: 'maven:3.8.6-jdk-11', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'selenium-hub', image: 'selenium/hub:3.4.0'),

    // Ensure there are no port conflicts and adapt selenium images
    containerTemplate(name: 'selenium-chrome',
        image: 'selenium/node-chrome:3.4.0', envVars: [
            containerEnvVar(key: 'HUB_PORT_4444_TCP_ADDR', value: 'localhost'),
            containerEnvVar(key: 'HUB_PORT_4444_TCP_PORT', value: '4444'),
            containerEnvVar(key: 'DISPLAY', value: ':99.0'),
            containerEnvVar(key: 'SE_OPTS', value: '-port 5556'),
        ]),

]) {
    node('maven-selenium') {
        stage('Checkout') {
            echo "Done git"
        }
        
        chrome: {
            container('maven-chrome') {
                stage('Test chrome') {
                    sh """
                        mvn -B clean test -Dselenium.browser=chrome \
                        -Dsurefire.rerunFailingTestsCount=5 -Dsleep=0
                    """
                }
            }
        }
        
    }
}
