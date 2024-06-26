pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/usuario/repositorio.git'
        BRANCH = 'main'
        RECIPIENT = 'preciadojairo82@gmail.com'
        EMAIL_SUBJECT = "Notificación de Construcción: ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        EMAIL_BODY = '''<!DOCTYPE html>
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
        </html>'''
        
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(credentials: ['2']) {
                    git branch: "${env.BRANCH}", url: "${env.REPO_URL}"
                }
            }
        }
        stage('Build') {
            steps {
                // Build steps go here
            }
        }
        stage('Deploy') {
            steps {
                // Deploy steps go here
            }
        }
    }

    post {
        always {
            // Configurar notificaciones por correo electrónico con contenido HTML
            emailext (
                to: "${env.RECIPIENT}",
                subject: "${env.EMAIL_SUBJECT}",
                body: "${env.EMAIL_BODY}",
                mimeType: 'text/html'
            )
        }
    }
}
