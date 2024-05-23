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
                script {
                    // Crear el directorio de despliegue si no existe
                    sh 'mkdir -p /var/www/html/mi-aplicacion'
                    // Copiar los archivos del repositorio al directorio de despliegue
                    sh 'cp -r * /var/www/html/mi-aplicacion/'
                }
            }
        }
        stage('Iniciar servidor HTTP') {
            steps {
                script {
                    // Iniciar el servidor HTTP para servir los archivos
                    sh 'nohup http-server /var/www/html/mi-aplicacion -p 8081 &'
                }
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
        }
    }
}
