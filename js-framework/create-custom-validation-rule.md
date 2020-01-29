# Create Custom Validation Rule

Create a new validation rule to validate email providers.

## Kata Solution

Solution is based on extending `mage/validation` component via a mixin to add a custom validation rule.

## Define a new mixin

`view/base/requirejs-config.js`:

```js
// MageKata: Create New Validation Rule
var config = {
    config: {
        mixins: {
            'mage/validation': {
                'Glushko_MageKata/js/validation-rule/validate-trusted-emails-mixin': true
            }
        }
    }
}
```

## Create a mixin component

`view/base/web/js/validation-rule/validate-trusted-emails-mixin.js`:

```js
// MageKata: Create New Validation Rule
define([
    'jquery'
], function ($) {
    "use strict";

    var trustedEmailProviders = [
        'gmail.com',
        'yahoo.com',
        'hotmail.com',
        'aol.com',
        'msn.com',
    ];

    return function () {
        $.validator.addMethod(
            'validate-trusted-emails',
            function (email, element) {
                if (!email.includes('@')) {
                    return false;
                }

                var emailProvider = email.toLowerCase().split('@')[1];

                return this.optional(element) || trustedEmailProviders.indexOf(emailProvider) > -1;
            },
            $.mage.__('Please enter a valid email address from trusted providers (e.g. Gmail)')
        );
    }
});
```
