pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/usuario/repositorio.git'
        BRANCH = 'main'
        DEPLOY_SERVER = 'user@Simple_Web_App.com'
        DEPLOY_PATH = '/var/www/jenkins'
        DEPLOY_URL = 'http://Simple_Web_App.com'
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
                script {
                    // Captura de errores durante la construcción
                    try {
                        // Instalar dependencias
                        sh 'npm install'
                        
                        // Ejecutar tareas de construcción (ajusta según tu configuración de gulp/grunt/webpack)
                        sh 'npm run build' // Asumiendo que este comando ejecuta gulp o webpack
                    } catch (Exception e) {
                        // Manejo de errores y registros
                        echo "Error en la fase de construcción: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error "Build failed"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Copiar archivos construidos al servidor Apache
                    try {
                        sh """
                            scp -r dist/* ${env.DEPLOY_SERVER}:${env.DEPLOY_PATH}
                        """
                        
                        // Verificar despliegue accediendo a la URL
                        def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' ${env.DEPLOY_URL}", returnStdout: true).trim()
                        if (response != '200') {
                            error "Despliegue fallido. Código de respuesta: ${response}"
                        } else {
                            echo "Despliegue exitoso. Aplicación disponible en: ${env.DEPLOY_URL}"
                        }
                    } catch (Exception e) {
                        // Manejo de errores durante el despliegue
                        echo "Error en la fase de despliegue: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error "Deploy failed"
                    }
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
