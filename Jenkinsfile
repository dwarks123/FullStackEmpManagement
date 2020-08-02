import java.text.*

properties([ disableConcurrentBuilds(), buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '15'))])

timestamps
{
    node('master')
    {
        try
        {
           
            stage('Set Version')
            {
                def dateFormat = new SimpleDateFormat("yyyy.MM.dd")
                def date = new Date()
                def dateString = dateFormat.format(date)
                def versionString = dateString + '.' + BUILD_NUMBER
                //Version = VersionNumber versionNumberString: versionString, versionPrefix: ''
                currentBuild.displayName = versionString
            }

            stage('SCM Pull')
            {
                deleteDir()
                checkout scm
            }

            stage('NuGet Restore')
            {
                
            }

           
        } catch (e)
        {
            echo e.getMessage()
            currentBuild.result='FAILURE'
            throw e
        } finally
        {
            if(currentBuild.result != 'SUCCESS')
            {
                webHook = "https://outlook.office.com/webhook/6d91536f-db7f-4f9c-9a71-6141a7e183d2@62ccb864-6a1a-4b5d-8e1c-397dec1a8258/JenkinsCI/65f650d3d2c948308b5a8813f73e1179/d8e4c789-09d5-4da7-a22e-338f36c50044"
                notifyBuild.sendBuildNotificationToTeams(currentBuild.result, "$webHook" )
            }

            bat 'git clean -fdx'
        }
    }
}
