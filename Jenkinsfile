// 실습2-7. 파이프라인 + Maven + Tomcat 
pipeline{
    agent any

    triggers { // 젠킨스 트리가있는지 젠킨스는 모름. 따라서 한번은 [지금빌드]를 눌러줘야 젠킨스에서 트리거 확인 가능 
        pollSCM('* * * * *')
    }

    environment{
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64/"
        MAVEN_HOME = "/usr/share/maven"
        
    }

    tools{
        jdk 'Java-17'
        maven 'Maven-3' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/kkogggokk/source-maven-java-spring-hello-webapp'
            }
        }

        stage('Java-17'){ // java-17-openjdk-amd64
            steps {
                echo "[install JDK]=============================="
                sh "java -version"
                sh "javac -version"                 
            }
        }

        stage('Build'){// Maven install & Setting 
            steps {
               sh 'mvn clean install'
            }
        }

        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy'){
            steps {
                deploy adapters: [tomcat9(credentialsID: 'tomcat-manager', url: 'http://192.168.56.102:8080/')], contextPath: null, war: 'target/hello-world.war'
            }
        }
    }
}