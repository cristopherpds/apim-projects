pipeline {
    agent any

    tools {
        jfrog 'jfrog-cli'
    }
    
    stages {
        stage('Build Packaging') {
            steps {
                dir('C:\\Axway\\apigateway\\Win32\\bin') {
                    bat '''
                    mkdir "%WORKSPACE%\\APIManager\\target"
                    setlocal enabledelayedexpansion
                    set "projectsToAdd="
                    for /D %%i in ("%WORKSPACE%\\pr*") do (
                        set "projectsToAdd=!projectsToAdd! "%%i""
                    )
                    projpack.bat --create --passphrase-none --name deployPack --type fed --add !projectsToAdd! --projpass-none --dir "%WORKSPACE%\\APIManager\\target"
                    '''
                }
            }
        }
        
        stage('Publish to Artifactory') {
            steps {
                jf 'rt u APIManager\\target\\*.* api-management-poc-repository'
                jf 'rt bp'

            }
        }
    }
}
