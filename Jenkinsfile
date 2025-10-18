pipeline {
    agent any
     tools {
        
       jdk 'JAVA'     
        maven 'MAVEN'   
    }
    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('frontend-reactapp') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat '''
                echo Deleting old frontend from Tomcat...
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\frontend-reactapp" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\frontend-reactapp"
                )
                echo Creating new frontend folder...
                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\frontend-reactapp"
                echo Copying new build files...
                xcopy /E /I /Y reactfrontend\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\frontend-reactapp\\"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('backend-springbootapp') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                bat '''
                echo Removing old backend WAR and folder...
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\backend-springbootapp.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\backend-springbootapp.war"
                )
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\backend-springbootapp" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\backend-springbootapp"
                )
                echo Copying new backend WAR file...
                copy "springbootbackend\\target\\*.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\"
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo ' Pipeline Failed.'
        }
    }
}