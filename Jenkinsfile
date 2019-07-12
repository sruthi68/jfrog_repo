node {
    stage('Uploading and downloading artifacts')
    git url: 'https://github.com/sruthi68/playbook.git'

    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "Artifactory-1"

    // Read the upload spec and upload files to Artifactory.
    def downloadSpec =
            '''{
            "files": [
                {
                    "pattern": "Test_repo/*.txt",
                    "target": "dependencies/",
                    "props": "p1=v1;p2=v2"
                }
            ]
        }'''

    def buildInfo1 = server.download spec: downloadSpec

    // Read the upload spec which was downloaded from github.
    def uploadSpec =
            '''{
            "files": [
                {
                    "pattern": "*.txt",
                    "target": "Test_repo",
                    "props": "p1=v1;p2=v2"
                }
            ]
        }'''

    // Upload to Artifactory.
    def buildInfo2 = server.upload spec: uploadSpec

    // Merge the upload and download build-info objects.
    buildInfo1.append buildInfo2

    // Publish the build to Artifactory
    server.publishBuildInfo buildInfo1
}
