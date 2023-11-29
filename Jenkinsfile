pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Check out your code from the Git repository
                checkout scm
            }
        }

        stage('Determine Version Update') {
        when {
          expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
    }
            steps {
                script {
                    def commitMessages = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                     echo "Latest Commit Message: ${commitMessages}"
                    def versionUpdateType = 'patch' // Default to patch update

                    if (commitMessages.contains("major update")) {
                        versionUpdateType = 'major' // Major update if "BREAKING CHANGE" is in commit messages
                    } else if (commitMessages.contains("feat:") || commitMessages.contains("new feature:")) {
                        versionUpdateType = 'minor' // Minor update if "feat" is in commit messages
                    } else if (commitMessages.contains("fix")) {
                        versionUpdateType = 'patch'
                    } else {
                        versionUpdateType = 'patch'
                    }
                    echo "Version update type: ${versionUpdateType}"
                    env.VERSION_UPDATE_TYPE = versionUpdateType
                }
            }
        }

        stage('Bump Version and Create Tag') {
        when {
        expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
    }
            steps {
                script {
                    // Read the current version from your project's version file or any other source
                    def currentVersion = readFile('{VERSION_FILE_PATH}').trim()
                
                    // Determine the next version based on the update type (e.g., "major," "minor," or "patch")
                    def nextVersion = incrementVersion(currentVersion, env.VERSION_UPDATE_TYPE)
                    
                    // Update the version.txt file with the new version using the absolute file path
                    writeFile file: '{VERSION_FILE_PATH}', text: nextVersion

                    // Update the dedicated version.txt file with the new version
                    sh "echo '${nextVersion}' > /{VERSION_FILE_PATH}"
                    
                    // // Commit the changes to version.txt and push to the remote repository
                    // sh 'git add version.txt'
                    // sh "git commit -m 'Bump version to ${nextVersion}'"
                    // sh "git push origin HEAD:devops"

                    // Create and push the Git tag
                     sh "git tag -a v${nextVersion} -m 'Release version ${nextVersion}'"
                     sh "git push origin v${nextVersion}"
                }
            }
        }
        stage('Bump Version and Create Tag') {
            steps {
                script {
                    // Read the current version from your dedicated version.txt file
                    def currentVersion = sh(script: 'cat {VERSION_FILE_PATH}', returnStdout: true).trim()

                    // Determine the next version based on the update type (e.g., "major," "minor," or "patch")
                    def nextVersion = sh(script: 'increment_version $currentVersion minor', returnStdout: true).trim()

                    // Update the dedicated version.txt file with the new version
                    sh "echo '${nextVersion}' > {VERSION_FILE_PATH}"

                    // Create and push the Git tag to the remote repository
                    sh "git tag -a v${nextVersion} -m 'Release version ${nextVersion}'"
                    sh "git push origin v${nextVersion}"
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                // Add your deployment steps here
                // This could involve copying files to a production server or deploying to a cloud service
            }
        }
    }
}

def incrementVersion(version, updateType) {
    def parts = version.split("\\.")
    def major = parts[0] as int
    def minor = parts[1] as int
    def patch = parts[2] as int

    if (updateType == 'major') {
        major += 1
        minor = 0
        patch = 0
    } else if (updateType == 'minor') {
        minor += 1
        patch = 0
    } else {
        patch += 1
    }

    return "${major}.${minor}.${patch}"
}

