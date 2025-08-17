pipeline {
    agent any
    
    environment {
        UNITY_PATH = "C:\\Program Files\\Unity\\Hub\\Editor\\2022.3.4f1\\Editor\\Unity.exe" // CAMBIAD ESTO
        REPO_URL = "https://github.com/Gamedev-Crafters/CookieClickerJenkins.git"
    }
    
    stages {
        stage('Checkout') {
            steps {
                bat "git pull ${REPO_URL}"
            }
        }
        
        stage('Test') {
            steps {
                bat """
                    if not exist "CI" mkdir "CI"
                    "${UNITY_PATH}" -runTests -projectPath "%WORKSPACE%" -exit -batchmode -testResults "%WORKSPACE%\\CI\\results.xml" -testPlatform EditMode
                """
            }
        }
        
        stage('Build') {
            steps {
                bat """
                    "${UNITY_PATH}" -executeMethod SimpleBuildScript.Build -projectPath "%WORKSPACE%" -quit -batchmode
                """
                archiveArtifacts artifacts: 'Build/**/*', fingerprint: true
            }
        }

        stage('Upload') {
            steps {
                bat "butler push Build knexator/unity-ci-test:win"
            }
        }
    }
}