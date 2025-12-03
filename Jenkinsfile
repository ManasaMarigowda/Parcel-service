pipeline {
    agent any
	stages {
    stage('Checkout') {
            steps {
                 sh "rm -rf Parcel-service"
				 sh "git clone https://github.com/ManasaMarigowda/Parcel-service"
        } 
	}
	}
    stages {
        stage('Install Java 17') {
            steps {
                sh '''
                    if ! java -version &>/dev/null; then
                        echo "Installing Java 17..."
                        sudo apt update
                        sudo apt install -y openjdk-17-jdk
                    else
                        echo "Java is already installed:"
                        java -version
                    fi
                '''
            }
        }
    }

    stages {
        stage('Set JAVA_HOME') {
            steps {
                script {
                    sh '''
                        JAVA_HOME_PATH=$(dirname $(dirname $(readlink -f $(which java))))
                        if ! grep -q "JAVA_HOME=$JAVA_HOME_PATH" /etc/environment; then
                            echo "Setting JAVA_HOME..."
                            echo "JAVA_HOME=$JAVA_HOME_PATH" | sudo tee -a /etc/environment
                            echo "export JAVA_HOME=$JAVA_HOME_PATH" | sudo tee -a /etc/profile
                            echo 'export PATH=$JAVA_HOME/bin:$PATH' | sudo tee -a /etc/profile
                            source /etc/profile
                            echo "JAVA_HOME set to $JAVA_HOME_PATH"
                        else
                            echo "JAVA_HOME is already set."
                        fi
                    '''
                }
            }
        }
    }

}
