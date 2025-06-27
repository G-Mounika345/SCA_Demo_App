pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK = 'C:/Tools/dependency-check-12.1.0-release/dependency-check/bin/dependency-check.bat'
        SONAR_SCANNER_PATH = 'C:/Users/gmoun/.dotnet/tools/.store/dotnet-sonarscanner/10.2.0/dotnet-sonarscanner/10.2.0/tools/netcoreapp3.1/any/dotnet-sonarscanner.dll'
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
                    dotnet "%SONAR_SCANNER_PATH%" begin /k:"SCA_Demo_App" /d:sonar.host.url="http://localhost:9000" /d:sonar.token="${SONAR_TOKEN}"
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
                    dotnet "%SONAR_SCANNER_PATH%" end /d:sonar.token="${SONAR_TOKEN}"
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
