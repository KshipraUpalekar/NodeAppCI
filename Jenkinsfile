pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                bat 'npm install'
            }
        }

        stage('Deploy and Keep Running') {
            steps {
                echo 'Starting Node.js app in background on port 3000...'

                bat '''
                @echo off
                for /f "tokens=5" %%a in ('netstat -aon ^| find ":3000" ^| find "LISTENING"') do (
                    echo Killing PID %%a
                    taskkill /F /PID %%a
                )
                '''

                bat 'start "NodeApp" /B node index.js'

                input message: "App is running at http://localhost:3000. Click PROCEED to stop."
                bat 'taskkill /F /IM node.exe'
            }
        }
    }

    triggers {
        githubPush()
    }
}
