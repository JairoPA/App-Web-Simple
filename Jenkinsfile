pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/JairoPA/App-Web-Simple.git', branch: 'main'
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
            // Configurar notificaciones por correo electrónico con contenido HTML
            emailext (
                to: 'preciadojairo82@gmail.com',
                subject: "Notificación de Construcción: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: '''<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Correo de Notificación</title>
</head>
<body>
    <div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
        <h2 style="color: #333;">Notificación de Construcción</h2>
        <p>Hola,</p>
        <p>Este correo es una notificación de construcción generada automáticamente por Jenkins.</p>
        <p>Detalles de la construcción:</p>
        <ul>
            <li>Nombre del Proyecto: ${env.JOB_NAME}</li>
            <li>Número de Construcción: ${env.BUILD_NUMBER}</li>
            <li>Estado de la Construcción: ${currentBuild.currentResult}</li>
            <li>Para más información, visita: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></li>
        </ul>
        <p>¡Gracias!</p>
    </div>
</body>
</html>''',
                mimeType: 'text/html'
            )
            // Configurar notificaciones por Slack
            // slackSend (channel: '#deployments', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]': ${currentBuild.currentResult}")
        }
    }
}
