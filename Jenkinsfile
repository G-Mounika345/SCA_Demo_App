pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK = 'C:/Tools/dependency-check-12.1.0-release/dependency-check/bin/dependency-check.bat'
        SONAR_SCANNER = 'C:/Users/gmoun/.dotnet/tools/dotnet-sonarscanner.exe'
        SONAR_TOKEN = 'sqp_101a9ca714108fee39eda11ae587037a5741116d'
        REPORT_PATH = 'SCA_Report'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/G-Mounika345/SCA_Demo_App.git'
            }
        }

        stage('SonarQube Begin') {
            steps {
                bat """
                    echo === SONAR BEGIN ===
                    "${SONAR_SCANNER}" begin /k:"SCA_Demo_App" /d:sonar.host.url="http://localhost:9000" /d:sonar.token="${SONAR_TOKEN}"
                """
            }
        }

        stage('Build') {
            steps {
                bat '''
                    echo === START BUILD ===
                    dotnet restore
                    dotnet build
                    echo === END BUILD ===
                '''
            }
        }

        stage('SonarQube End') {
            steps {
                bat """
                    echo === SONAR END ===
                    "${SONAR_SCANNER}" end /d:sonar.token="${SONAR_TOKEN}"
                """
            }
        }

        stage('SCA - Dependency Check') {
            steps {
                bat """
                    echo === START SCA ===
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
