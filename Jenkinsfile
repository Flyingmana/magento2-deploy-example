#!groovy​
pipeline {
  agent any

  environment {
    MAGENTO_PACKAGES__DOMAIN__COM_USERNAME = credentials('magento-repo-username')
    MAGENTO_PACKAGES__DOMAIN__COM_PASSWORD = credentials('magento-repo-password')
    MAGENTO_ARTIFACT_HOST = credentials('ssh_magento2_artifact_host')
  }
  stages {
    stage('fetch dependencies') {
        steps {
            checkout scm
            sh 'php --version'
            sh 'composer config -a -g http-basic.repo.magento.com $MAGENTO_PACKAGES__DOMAIN__COM_USERNAME $MAGENTO_PACKAGES__DOMAIN__COM_PASSWORD'
            sh 'php bin/install_main.php'
        }
    }
    stage('build code') {
        steps {
            sh './project-community-edition/bin/magento setup:static-content:deploy --strategy=standard -f'
            sh 'php ./project-community-edition/bin/magento setup:di:compile'
        }
    }
    stage('placeholder') {
        steps {
            sh 'php ./project-community-edition/bin/magento'
        }
    }
    stage('build artifact') {
        steps {
            sh 'tar --exclude=artefacts -cjSf artefacts/magento-pipeline-deploy.tar.bz2 ./'
            archiveArtifacts 'artefacts/magento-pipeline-deploy.tar.bz2'
            sh 'scp artefacts/magento-pipeline-deploy.tar.bz2 root@$MAGENTO_ARTIFACT_HOST:/$BUILD_TAG-magento-pipeline-deploy.tar.bz2'
        }
    }
  }
}
