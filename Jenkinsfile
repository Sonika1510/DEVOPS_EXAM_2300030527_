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
        copy "backend-springbootapp\\target\\backend-springbootapp.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\backend-springbootapp.war"
        '''
    }
}
