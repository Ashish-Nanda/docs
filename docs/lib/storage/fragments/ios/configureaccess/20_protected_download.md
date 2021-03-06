<amplify-block-switcher>

<amplify-block name="Listener (iOS 11+)">

```swift
func downloadData(key: String, userId: String) {
    let options = StorageDownloadDataRequest.Options(accessLevel: .protected, targetIdentityId: userId)
    Amplify.Storage.downloadData(
        key: key,
        options: options,
        progressListener: { progress in
            print("Progress: \(progress)")
        }, resultListener: { (event) in
            switch event {
            case let .success(data):
                print("Completed: \(data)")
            case let .failure(storageError):
                print("Failed: \(storageError.errorDescription). \(storageError.recoverySuggestion)")
        }
    })
}
```

</amplify-block>

<amplify-block name="Combine (iOS 13+)">

```swift
var progressSink: AnyCancellable?
var resultSink: AnyCancellable?
    
func downloadData(key: String, userId: String) {
    let options = StorageDownloadDataRequest.Options(accessLevel: .protected,
                                                     targetIdentityId: userId)
    let storageOperation = Amplify.Storage.downloadData(key: key, options: options)
    progressSink = storageOperation.progressPublisher.sink { progress in print("Progress: \(progress)") }
    resultSink = storageOperation.resultPublisher.sink {
        if case let .failure(storageError) = $0 {
            print("Failed: \(storageError.errorDescription). \(storageError.recoverySuggestion)")
        }
    }
    receiveValue: { data in
        print("Completed: \(data)")
    }
}
```

</amplify-block>

</amplify-block-switcher>