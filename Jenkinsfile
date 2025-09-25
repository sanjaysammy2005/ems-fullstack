pipeline {
    agent any

    stages {
        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== COPY FRONTEND TO BACKEND =====
        stage('Integrate Frontend into Backend') {
            steps {
                bat '''
                if exist backend\\EmployeemanagementSystem\\src\\main\\resources\\static (
                    rmdir /S /Q backend\\EmployeemanagementSystem\\src\\main\\resources\\static
                )
                mkdir backend\\EmployeemanagementSystem\\src\\main\\resources\\static
                xcopy /E /I /Y frontend\\dist\\* backend\\EmployeemanagementSystem\\src\\main\\resources\\static
                '''
            }
        }

        // ===== BACKEND BUILD (WAR includes frontend now) =====
        stage('Build Backend') {
            steps {
                dir('backend/EmployeemanagementSystem') {
                    bat 'mvn clean package'
                }
            }
        }

        // ===== DEPLOY TO TOMCAT =====
        stage('Deploy Backend + Frontend') {
            steps {
                bat '''
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springbootemployeeapi.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springbootemployeeapi.war"
                )
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springbootemployeeapi" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springbootemployeeapi"
                )
                copy "backend\\EmployeemanagementSystem\\target\\*.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springbootemployeeapi.war"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline Failed.'
        }
    }
}
