# scepter-vsts-build-release-definitions

[![scepter-logo](http://res.cloudinary.com/source-4-society/image/upload/v1519221119/scepter_hzpcqt.png)](https://github.com/source4societyorg/SCEPTER-core)

This repository contains useful build definitions that can be imported into [Visual Studio Team Services][https://visualstudio.com/vso] build and release for your [SCEPTER](https://github.com/source4societyorg/SCEPTER-Core] project.

# ui-s3-deployment

This build definition will automatically run the unit tests associated with your web user interface and proceed to upload them to the S3 bucket in your definition. For this script to work, you will need to setup the branch in the core repository that will automatically trigger the build, as well as the file path to the user interface that you wish to publish (so that when a change is made to your ui application in the core, it will automatically publish).

Furthermore you will need to configure the specific environment targets and bucket destination on the individual tasks within the definition.

## setup

Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn)

# ui-s3-deployment
Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn)

Set the following variables up in the variables tab:

    scepter.branch        # the branch you would like to target for automatic build triggers, ex. integration
    scepter.environment   # the specific configuration environment to trigger, ex. development
    scepter.pat           # your VSTS base64 encoded PAT token. [See this article for help](https://blog.devmatter.com/personal-access-tokens-and-vsts-apis/)
    scepter.repositoryurl # The URL of the repository you wish to send the authentication header to, ex. scepter.visualstudio.com
    scepter.uifolder      # the folder name of the user interface you wish to target in your projects ui folder, ex. webui
    scepter.bucketprefix  # The prefix of your S3 bucket. The script expects the bucket to be in format $(scepter.bucketprefix)-$(scepter.environment) in S3.

Be sure to add your build triggers to the appropriate branch. SCEPTER projects should target the core repository of their project and only when changes to the ui folder they are targeting are made.

# service-lambda-deployment

Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn)

Set the following variables up

    scepter.service        # The name of the service as it appears in your folder structure (likely the name you used to create the service)
    scepter.provider       # The name of the provider. Currently only "aws" is supported
    scepter.environment    # The name of the environment to target, i.e. "development"

See [scepter-command-service - deploy](https://github.com/source4societyorg/SCEPTER-command-service) for an idea how this works

# ui-android-build

Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn), [vsts-react-native-tasks](https://github.com/Microsoft/vsts-react-native-tasks), [Build:Gradle](https://go.microsoft.com/fwlink/?LinkID=613720), Decrypt File (Open SSL), [Build: Android Signing](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/android-signing?view=vsts), [Utility: Copy Files](https://go.microsoft.com/fwlink/?LinkID=708389), [Utility: Publish Build Artifacts](https://docs.microsoft.com/en-us/vsts/build-release/tasks/utility/publish-build-artifacts?view=vsts), [AppCenter: Distribute](https://intercom.help/appcenter/).

This will trigger a react-native android build using the gradle configuration within your project, with a deploy to AppCenter. By default it looks for new commits to the `integration` branch, specifically to the mobile app ui folder. You may need to adjust this to match your unique scepter setup. Set the variables as follows:

    scepter.keyencryptpass # Password for you encrypted key file
    scepter.keypass        # Password for your signing key
    scepter.keystorefile   # The name of your keystore file. You should have a file of the same name with .enc appended to it stored in your android/app/keystores folder.
    scepter.keystorepass   # Yet another password, this one for your keystore itself.
    scepter.pat            # Your base64 encoded username:pat token for repository access via https
    scepter.repositoryurl  # scepter.repositoryurl # The URL of the repository you wish to send the authentication header to, ex. scepter.visualstudio.com
    scepter.uifolder      # the folder name of the user interface you wish to target in your projects ui folder, ex. mobileui

You can encrypt your signing key and store it in the repository safely (assuming P != NP) or modify this to store it in VSTS [Secure Files](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/secure-files?view=vsts) depending on whether you want a vendor dependent solution.

This is an export of an integration branch build, so zipalign is not set. You can make the necessary adjustments fairly easily though by understanding the [Build: Android Signing](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/android-signing?view=vsts), [Utility: Copy Files](https://go.microsoft.com/fwlink/?LinkID=708389) task.

# ui-ios-build

Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn), [vsts-react-native-tasks](https://github.com/Microsoft/vsts-react-native-tasks), [Utility: Install Apple Certificate](https://docs.microsoft.com/en-us/vsts/build-release/tasks/utility/install-apple-certificate?view=vsts), [Utility: Install Apple Provisioning Profile](https://docs.microsoft.com/en-us/vsts/build-release/tasks/utility/install-apple-provisioning-profile?view=vsts), [Build: Xcode Build](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xcode-build?view=tfs-2018), [Utility: Copy Files](https://go.microsoft.com/fwlink/?LinkID=708389), [Utility: Publish Build Artifacts](https://docs.microsoft.com/en-us/vsts/build-release/tasks/utility/publish-build-artifacts?view=vsts), [AppCenter: Distribute](https://intercom.help/appcenter/).

This will trigger a react-native ios build using xcode configuration within your project, with a deploy to AppCenter. By default it looks for new commits to the `integration` branch, specifically to the mobile app ui folder. You may need to adjust this to match your unique scepter setup. Set the variables as follows:

    scepter.pat            # Your base64 encoded username:pat token for repository access via https
    scepter.projectfolder  # name of your xcodeproj file without the .xcodeproj extension
    scepter.repositoryurl  # scepter.repositoryurl # The URL of the repository you wish to send the authentication header to, ex. scepter.visualstudio.com
    scepter.scheme         # Name of the iOS build scheme for your application (usually matches projectfolder)
    scepter.uifolder       # the folder name of the user interface you wish to target in your projects ui folder, ex. mobileui
    scepter.repositoryurl  # scepter.repositoryurl # The URL of the repository you wish to send the authentication header to, ex. scepter.visualstudio.com

Store your signing and provisioning certificates in [Secure Files](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/secure-files?view=vsts) and reference them in the appropriate tasks.


