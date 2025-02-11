
# Locate the IP Address from a website

This code checks the user's IP address to determine whether access to a certain web page should be allowed or denied.

The first part is the standard HTML declaration which includes information about the web page such as document type, language, metadata, title and style sheets.


The body of the page has a div element with the class 'center', which will centre the elements it contains.
The PHP code uses the `file_get_contents` function to get the geographical information from the IP address using the `ipinfo.io` website.
Then it checks if the 'country' property exists and if it does, if the user's country is France. If so, access to the page is allowed.

Otherwise, access is denied and an error message is displayed, indicating the user's IP address and the name and flag of his home country.

[!] **Note also that the code includes a file called "codes.php" which should contain an associative array of country codes and country names.**

---
## In case of error


If access to the site is denied, the div is displayed in `#FF0000` (red)
```css
.center {text-align:center;display: flex;justify-content: center;align-items: center;height: 100vh;}
.error {background: rgba(255, 0, 0, 0.4);padding: 10px;border-radius: 5px;}
```
![App Screenshot](https://media.discordapp.net/attachments/733366929561092157/1074383904875757679/image.png)

---
## Including the `codes.php` file

For this project, I use _include_ but you can also use the following instructions :
```php
require("codes.php");

require_once("codes.php");

include("codes.php");

include_once("codes.php");
```

Use `require` to include and run a file, and generate a fatal error if the file cannot be included.

Use `require_once` to include and run a file only once, which can avoid multiple re-inclusions and the potential errors that come with them.

Use `include` to include and run a file, but generate a warning instead of a fatal error if the file cannot be included.

Use `include_once` to include and run a file once, but generate a warning instead of a fatal error if the file cannot be included.

_It is important to choose the right type of instruction depending on the need for code and error handling in your application._

The file includes the name, flag and international code of all countries in the world, here is an example:
```php
$countryCodes = array(

    'FR' => 'France 🇫🇷',
    'GB' => 'United Kingdom 🇬🇧',
    'KR' => 'South Korea. 🇰🇷',
    'JP' => 'Japan 🇯🇵',
    [...]
);
```

---
## Get the geographical information from the IP address


We use the `file_get_contents` function to get the geographical information from the IP address ( with -> ipinfo.io ) :
```php
$ip = $_SERVER['REMOTE_ADDR'];
$info = json_decode(file_get_contents("http://ipinfo.io/{$ip}/json"));
```

---
## Check the `country` property


In this example, we check if the 'country' property exists and if so, if the user's country is France (code = FR). We also check if the IP address of the user is known in the whitelist.

```php
    if (property_exists($info, 'country') && $info->country === "FR" or in_array($ip, $whitelist)) {

            // [FR] On autorise l'accès au site
            // [GB] Access to the site is allowed
            echo "<div>Your IP address is from France! Welcome to our website :D<br>Your IP address is : " . $ip."</div>";
    }     

```

Otherwise, access is denied by displaying the user's IP address, as well as the name and flag of his country retrieved from the table in the `codes.php` file

```php
    } else {
   
            $country = property_exists($info, 'country') ? (array_key_exists($info->country, $countryCodes) ? $countryCodes[$info->country] : 'Unknown location') : 'Unknown location';
            echo "
                    <div class='error'>
                        Sorry, this website is only accessible to users with an IP address located in<strong> France 🇫🇷</strong><br><br>
                        Your IP address is : <strong>". $ip."</strong>, and it comes from <strong>".$country."</strong>
                    </div>";
    }

```


---
## New functionality !

You can now choose to deny access to certain IP addresses, from the table in the `black_list.php` file

```php
    $blacklist = array(
        // IP addresses refused by default
        "127.0.0.1" => "Localhost",
        "192.168.0.1" => "Default Gateway",
        "10.0.0.1" => "Private Network",
        "8.8.8.8" => "Google Public DNS server",
        
        // here you can add the addresses you want to block, and the reason or the origin
        // " IP " => " Reason or origin",
    );

```
![App Screenshot](https://media.discordapp.net/attachments/733366929561092157/1074662496885014608/image.png)

```php
    if (array_key_exists($ip, $blacklist)) {
            $ip_adress_list = $blacklist[$ip];
            echo "
                    <div class='error'>
                        Sorry, you have been <strong>**denied access**</strong> to this website<br><br>
                        Your IP address is : <strong>". $ip."</strong>, and it comes from <strong><mark style='border-radius:2px;padding:2px'>".$ip_adress_list."</mark></strong><br><br>
                        If this error appears, it is likely that your IP address has been blacklisted by the developers of this site, or that it is incompatible with the use of the site.<br><br>
                        To correct this error, please contact a site administrator or your network administrator.
                    </div>";
    } 

```

---

You can now choose to allow access to certain IP addresses, from the table in the `white_list.php` file.

```php
    $whitelist = array(
        // IP addresses allowed by default
        "192.168.1.1", 
        "192.168.1.2", 
        "192.168.1.3",

        // here you can add the addresses you want to autorize.
        // "XX.XX.XX.XX",
    );

```


## Author

- [Hugo T](https://www.github.com/HugoTby)

