# gallery-dl-Reddit-rate-limit-bypass
How to bypass Reddit Rate Limit when using gallery-dl to download subreddits.

To bypass the Reddit Rate Limit when using gallery-dl there is a couple of things you will have to do and change.

1. Start by downloading the basic "gallery-dl.conf" in the repo above.
2. Either put it in the root folder where your "gallery-dl.exe" is located or:

*gallery-dl* searches for configuration files in the following places:

Windows:
  * ``%APPDATA%\gallery-dl\config.json``
    
  * ``%USERPROFILE%\gallery-dl\config.json``
    
* ``%USERPROFILE%\gallery-dl.conf``

    (``%USERPROFILE%`` usually refers to a user's home directory,
    i.e. ``C:\Users\<username>\``)

When run as `executable <Standalone Executable_>`__,
*gallery-dl* will also look for a ``gallery-dl.conf`` file
in the same directory as said executable.

3. Open the "gallery-dl.conf" file with your preferred editor or simply just notepad.
4. Search for "Reddit"
5. Go to where there is "Reddit", and here will be some options you can configure for Reddit
6. Next login to Reddit in your browser and navigate to:
```html
  https://www.reddit.com/prefs/apps/
```
7. Click the "are you a developer? create an app..." button or the "create another app..." button and fill out the form like so:
      * choose a name
      * select "installed app"
      * set ``http://localhost:6414/`` as "redirect uri"
      * solve the "I'm not a rebot" reCATCHA if needed
      * click "create app"
8. Copy the client id (third line, underneath your application's name and "installed app") and put it in your configuration file as the ``"client-id"``
9. In your configuration file use "``Python:<application name>:v1.0 (by /u/<username>)``" as ``user-agent`` and replace ``<application name>`` and ``<username>`` accordingly.
10. Clear your cache by running
```html
  gallery-dl --clear-cache reddit
```
to delete any remaining ``access-token`` entries.

11. Get a `refresh-token` by running
```html
  gallery-dl oauth:reddit
```
and allowing your `<application name>` to connect with your reddit account.

12. You will then be redirected to a page which will present you with your `refresh-token`
13. Copy the `refresh-token` in your browser and place it in your configuration file as the ``"refresh-token"``
14. Remember all three the new values we placed in your configuration file (``"client-id"``, ``"user-agent"`` and ``"refresh-token"``) should be enclosed in dubble quotations ""

The Reddit config part in your configuration file should look something like:
```json
          "reddit":
        {
            "comments": 0,
            "morecomments": false,
            "date-min": 0,
            "date-max": 253402210800,
            "date-format": "%Y-%m-%dT%H:%M:%S",
            "id-min": null,
            "id-max": null,
            "recursion": 0,
            "videos": true,
            "client-id": "<client-id-here>",
            "user-agent": "Python:<application-name-here>:v1.0 (by /u/<username-here>)",
            "refresh-token": "<refresh-token-here>"
        },
```

Remember to not include the `<` and `>` symbols!

Voila!

Now you should be able to run "gallery-dl.exe" to download reddit posts or whole subreddits without rate limmiting!

Basic example gallery-dl command to run in your cli:
```txt
  gallery-dl URL
```

When downloading whole subreddits consider taking it slow by using "--range 1-50" and upping the value each time the range has completed:
```txt
  gallery-dl --range 1-50 URL
```
then
```txt
  gallery-dl --range 1-200 URL
```
etc. etc.
