# Template editor
✍ Mail template editor

The template editor is only compatible with Nextcloud 11 or older. 

We removed the template editor in Nextcloud 12 because we changed how emails are generated. While the customization capabilities offered by the template editor were easy to use, they often resulted in broken emails. To fix this, we designed a much easier mechanism that automatically generates emails which follow the theme settings and look the same in all the different email clients out there.

* If, for some reason, you need text-only emails, consider simply configuring this on the client side or let the receiving (or even sending) mail server drop the HTML part. Note that there is no security impact from **sending** html emails, just from displaying them and thus any security risk can only be mitigated by disabling showing html on the client (or removing the HTML part in the mail server).

## Modifying the look of emails beyond the theming app capabilities
You can now overwrite templates by writing a class that implements the template interface (or extends it to not need to copy over everything). Easiest way is then put this class into an app and load it (so you do not need to patch it in on every update).

This is the interface of the class that needs to be implemented: https://github.com/nextcloud/server/blob/master/lib/public/Mail/IEMailTemplate.php

That is the implementation that could be extended and used to see how it works: https://github.com/nextcloud/server/blob/master/lib/private/Mail/EMailTemplate.php

An example from [nextcloud/templateeditor#23](https://github.com/nextcloud/templateeditor/issues/23#issuecomment-328175633):
1. Look at the source code of extended class [OC\Mail\EMailTemplate::class](https://github.com/nextcloud/server/blob/master/lib/private/Mail/EMailTemplate.php)
2. Then override what you need in your own `OC\Mail\EMailTemplate::class` extension

__Example__

> _Let's assume that we need to override the email header._  

```php
<?php

namespace \OCA\MyApp;

use OC\Mail\EMailTemplate;

class MyClass extends EMailTemplate
{
    protected $header = <<<EOF 
        <table align="center" class="wrapper">
            // your theme email header modification
        </table>
    EOF;
}
```

3. Then in `config/config.php` change `mail_template_class` to your class namespace

```php
'mail_template_class' => 'OCA\\MyApp\\MyClass',
```

If you need any help, contact the Nextcloud support team or [read a step by step guide in the support portal.](https://portal.nextcloud.com/article/customized-email-templates-29.html)
