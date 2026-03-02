# VRTC Token Library

You may copy the source code to your own project folder.

### Quick Start
Take Golang SDK as an example，after `import ""xxx/AccessToken""`:

```go
var (
    appID  = "123456781234567812345678" // Applied account id  
    appKey = "DO_NOT_HARDCODE_HERE" // Applied secret key
    roomID = "room"
    userID = "uid"
)

t := AccessToken.New(appID, appKey, roomID, userID)

// For example, add stream-subscribe permission for audience,
// expire two hours later.	
t.AddPrivilege(AccessToken.PrivSubscribeStream, time.Now().Add(time.Hour*2))   

// Add stream-publish permission for host,
// expire 1 hour later.
t.AddPrivilege(AccessToken.PrivPublishStream, time.Now().Add(time.Hour*1))

// Get the final VRTC token string.
token, err := t.Serialize()
```

### PS
Reference of other language can be found in `Test` code or file.
