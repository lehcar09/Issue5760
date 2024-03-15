### Github Issue Link
[- https://github.com/firebase/firebase-android-sdk/issues/5760](https://github.com/firebase/firebase-android-sdk/issues/5760)

### Summary
`Missing app id. Please check that it was passed in and try again` error for split APK 

### Prerequisite:
- add `google-services.json` file in app
- add credential folder and service accounts 

### Steps to reproduce
Run command `./gradlew assembleDevDebug appDistributionUploadX86DemoDebug`

#### Actual Logs
```
> Configure project :app
> Task :app:appDistributionUploadX86DevDebug FAILED
Using APK path specified by the artifactPath parameter in your app's build.gradle: <PROJECT PATH>/Issue5760/app/build/outputs/apk/dev/debug/app-dev-x86-debug.apk.
Uploading APK to Firebase App Distribution...

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:appDistributionUploadX86DevDebug'.
> Missing app id. Please check that it was passed in and try again

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.
> Get more help at https://help.gradle.org.

BUILD FAILED in 11s
```

### Workaround
```
tasks.register("appDistributionUpload${abiName.capitalize()}${variant.name.capitalize()}", com.google.firebase.appdistribution.gradle.UploadDistributionTask) {
    **appId = "<Set App ID here>"**
    artifactPath = output.outputFile.absolutePath
    serviceCredentialsFile = project.file("credential/service-accountA.json").absolutePath
    group = "tester"
}
```
