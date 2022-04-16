<img src="https://cdn.socialtoapp.com/img/logo/logo.svg" alt="drawing" width="150px"/>

# SocialToApp Wordpress plugin
You can now shorten links directly from WordPress using shortcode. If you don't want to upload a plugin, you can use this method. It is very easy to setup and it works with all versions of WordPress and with any theme. All links you will shorten will be safely stored in your account.

###  Instructions

1. Copy your unique php code below
2. Go to WordPress Admin > Appearance > Theme File Editor
3. On the right side, under Theme Files, find and click on Theme Functions (functions.php)
4. Paste the code below at the very end of the file and save

###  Code to add in functions.php
Attention, change YOURAPIKEY with your api key generated on SocialToApp
```php
// This code simply registers the shortcode "shorturl". You can change it if you want something else 
add_shortcode("shorturl", "pus_shortcode_shorten_url");

// Function to send the request
function pus_shortcode_shorten_url($atts, $content){
    
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, "https://socialtoapp.com/api/url/add");
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode(["url" => $content]));
    curl_setopt($ch, CURLOPT_TIMEOUT, 10);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        "Authorization: Bearer YOURAPIKEY",
        "Content-Type: application/json"
    ]);

    $response = curl_exec($ch);
    curl_close($ch);

    $object = json_decode($response);

    if($object && isset($object->shorturl)){
        return $object->shorturl;
    }

    return $content;
}     
```

### How to use
The following code will apply the shortcode "shorturl" to the system and you will be able to use the following format.

```
[shorturl]https://google.com[/shorturl]
```


You can also use the shortcode in html.

```
<a href="[shorturl]https://google.com[/shorturl]">Google</a>
```


