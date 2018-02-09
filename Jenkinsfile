#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String elasticSearchPackageName = ""
String elasticSearchStoragePackageName = ""
String elasticSearchChartName = "elasticsearch"
String elasticSearchStorageChartName = "elasticsearch-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        def helm = new io.stakater.charts.Helm()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, elasticSearchChartName)
            elasticSearchPackageName = helm.package(WORKSPACE, elasticSearchChartName)

            helm.lint(WORKSPACE, elasticSearchStorageChartName)
            elasticSearchStoragePackageName = helm.package(WORKSPACE, elasticSearchStorageChartName)
        }

        stage('Upload Chart') {
            chartManager.uploadToChartMuseum(WORKSPACE, elasticSearchChartName, elasticSearchPackageName)
            chartManager.uploadToChartMuseum(WORKSPACE, elasticSearchStorageChartName, elasticSearchStoragePackageName)
        }
    }
}
