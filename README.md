# WebPush

This Gem will send the Web Push API. It supports the encryption necessary to payload.

Payload is supported by Chrome50+, Firefox48+.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'webpush'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install webpush

## Usage

```ruby
message = {
  title: "title",
  body: "body",
  icon: "http://example.com/icon.pn"
}

Webpush.payload_send(
  message: JSON.generate(message),
  endpoint: "https://android.googleapis.com/gcm/send/eah7hak....",
  p256dh: "BO/aG9nYXNkZmFkc2ZmZHNmYWRzZmFl...",
  auth: "aW1hcmthcmFpa3V6ZQ==",
  api_key: "[GoogleDeveloper APIKEY]" # optional, not used in Firefox.
)
```

### ServiceWorker sample

see. https://github.com/zaru/web-push-sample

p256dh and auth generate sample code.

```javascript
navigator.serviceWorker.ready.then(function(sw) {
  Notification.requestPermission(function(permission) {
    if(permission !== 'denied') {
      sw.pushManager.subscribe({userVisibleOnly: true}).then(function(s) {
        var data = {
          endpoint: s.endpoint,
          p256dh: btoa(String.fromCharCode.apply(null, new Uint8Array(s.getKey('p256dh')))).replace(/\+/g, '-').replace(/\//g, '_'),
          auth: btoa(String.fromCharCode.apply(null, new Uint8Array(s.getKey('auth')))).replace(/\+/g, '-').replace(/\//g, '_')
        }
        console.log(data);
      });
    }
  });
});
```

payloads received sample code.

```javascript
self.addEventListener("push", function(event) {
  var json = event.data.json();
  self.registration.showNotification(json.title, {
    body: json.body,
    icon: json.icon
  });
});
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/zaru/webpush.
