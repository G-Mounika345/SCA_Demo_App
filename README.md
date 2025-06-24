Project Name : SCA_Demo_App
                  This is a simple .NET console application created to demonstrate **Software Composition Analysis (SCA)** using **OWASP Dependency-Check**, integrated into a **Jenkins CI/CD pipeline**.

Project Overview :
- Framework: .NET Core
- CI/CD Tool: Jenkins
- Security Tool: OWASP Dependency-Check (CLI)
- Source Control: Git + GitHub

Project Structure :
SCA_Demo_App/

├── Program.cs
├── SCA_Demo_App.csproj
├── Jenkinsfile
└── README.md

yaml
Copy
Edit

Build & Run
-	Restore & Build the Project
```bash
dotnet restore
dotnet build
-	Run the Application
bash
Copy
Edit
dotnet run

Security Scanning (Dependency-Check)
This project uses OWASP Dependency-Check CLI to scan for known vulnerabilities in NuGet packages.

Run SCA Manually
bash
Copy
Edit
dependency-check.bat --project SCA_Demo_App --scan . --format HTML --out SCA_Report
Output will be generated in the SCA_Report directory.

HTML report: SCA_Report/dependency-check-report.html

 Jenkins Pipeline Stages
The Jenkinsfile contains:
Clone: Pull code from GitHub
Build: Run dotnet restore and dotnet build
SCA: Use Dependency-Check to scan dependencies
Archive: Save the vulnerability report as a Jenkins artifact

Sample Output
dependency-check-report.html — viewable in browser after build
Jenkins build logs show results and alerts (CVEs)

