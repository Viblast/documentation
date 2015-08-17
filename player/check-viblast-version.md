
## Checking the version of Viblast Player that you're using

Open the browser console and execute:

```javascript
Viblast.version()
```

You should see a message similar to:

```
viblast|5.92.fc98e3ce|Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.152 Safari/537.36
```

The string before the first `|` is the name of the product. It's always `viblast`. The string between the two `|` is the `version` itself and the string after the second `|` is the browser version.

You can read more about Viblast Player versions in our <a href="{% url 'vb-player:doc' article='release-history' %}">Release History</a> page.

If you do not see a version, just some hash, for example:

```
viblast|7042ff8d|Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.152 Safari/537.36
```

Then you are using a legacy version of Viblast Player and should update. 

To update, use the download link in the e-mail we sent you announcing the release. If you have not received such an e-mail and have downloaded Viblast Player from our website, please [contact us]({% url 'vb-player:index' %}#contact). 
