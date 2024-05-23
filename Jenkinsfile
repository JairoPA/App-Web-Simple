pipeline {
    agent any
    
    stages {
        stage('Clonar repositorio') {
            steps {
                git clone 'https://github.com/JairoPA/App-Web-Simple.git'
            }
        }
        stage('Desplegar aplicación') {
            steps {
                // Aquí colocarías los comandos para desplegar tu aplicación
                // Puedes usar shell, scp, rsync, etc.
                // Por ejemplo, para un servidor local:
                sh 'mkdir -p /var/www/html/mi-aplicacion && cp -r * /var/www/html/mi-aplicacion/'
                
                // Para un servidor remoto:
                // sh 'scp -r * user@yourserver:/path/to/deploy/'
            }
        }
    }

    post {
        always {
            // Configurar notificaciones por correo electrónico
            emailext (
                to: 'preciadojairo82@gmail.com',
                subject: "Job",
                body: "Job "
            )
            // Configurar notificaciones por Slack
            //slackSend (channel: '#deployments', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]': ${currentBuild.currentResult}")
        }
    }
}
