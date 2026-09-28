pipeline {

    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.6.3'
        JMETER_TEST = 'tests\\escenario-principal.jmx'
        JMETER_JTL  = 'results\\resultados.jtl'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Ejecutar JMeter') {
            steps {
                bat '''
                    if not exist "results" mkdir "results"

                    if exist "%JMETER_JTL%" (
                        del /f /q "%JMETER_JTL%"
                    )

                    "%JMETER_HOME%\\bin\\jmeter.bat" ^
                        -n ^
                        -t "%JMETER_TEST%" ^
                        -l "%JMETER_JTL%"
                '''
            }
        }
    }

    post {
        always {

            perfReport(
                sourceDataFiles: 'results/resultados.jtl',
                errorUnstableThreshold: 1,
                errorFailedThreshold: 10
            )

            archiveArtifacts(
                artifacts: 'results/**/*',
                fingerprint: true
            )
        }
    }
}