pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK = 'C:/Tools/dependency-check-12.1.0-release/dependency-check/bin/dependency-check.bat'
        REPORT_PATH = 'SCA_Report'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/G-Mounika345/SCA_Demo_App.git'
            }
        }

        stage('Build') {
            steps {
                bat '''
                    echo === START BUILD ===
                    dotnet --version
                    dotnet restore
                    dotnet build
                    echo === END BUILD ===
                '''
            }
        }

        stage('SCA - Dependency Check') {
            steps {
                bat """
                    echo === START SCA ===
                    cd
                    dir
                    ${DEPENDENCY_CHECK} --project SCA_Demo_App --scan . --format HTML --out ${REPORT_PATH}
                    echo === END SCA ===
                """
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: "${REPORT_PATH}/dependency-check-report.html", fingerprint: true
        }
    }
}
