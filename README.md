SSE_DebugErrors
======
Adds a new debug configuration option that adds more information to unspecific error messages and lets you specify how to handle these additional errors.

## Version 
* Version 0.1.0

## Requirements ##

* Magento 1.x
* PHP 5.4 or higher

## List of Errors

The following errors are handled by this module. They are displayed or logged based on the module configuration (see "Configuration" below):

- 
    > Filename cannot be empty

	This message appears when a template file is missing, but unfortunately does not tell you anything about the whereabouts. The new message contains the full path of the missing template, the block name and the block class.
	
- 
    > Not valid template file: $fileName

	This message gets silently logged to system.log if symlinks are not allowed and a template is behind a symlink. A common error on fresh installations that use modman. The new message is more explicit.
	
## Installation

1. Manual installation: download [the latest release](https://github.com/schmengler/DebugErrors/zipball/master) and copy the `app` directory into the Magento installation.
2. Install via composer as dev dependency:

        "repositories": [
            {
                "type": "git",
                "url": "https://github.com/schmengler/DebugErrors.git"
            }
        ],
        "require-dev": {
            "sse/debugerrors": "~0.1.0"
        }
    

## Configuration

Enable debug errors in System > Configuration > Advanced > Developer > Debug:

![Screenshot](https://github.com/schmengler/DebugErrors/raw/master/screenshot-configuration.png)

If you are using Developer Client Restriction, only the configured IPs will use the additional error handling.

You can also specify how the debug error messages should be logged and displayed:

- **Throw exception immediately:** An exception is thrown and logged to exception.log. Note that Magento might catch and swallow the exception.
- **Show exception in block:** Replace current block with exception message (only if error is in context of a block). Also always logs exception to exception.log.
- **Only log exception to exception log:** Only logs exception to exception.log
- **Trigger PHP Warning:** Magento handles warnings depending on the environment variable MAGE_IS_DEVELOPER_MODE. If developer mode is on, an exception is thrown and displayed. If not, the warning gets logged to system.log.


## Technical Information

The extension does not rewrite any classes. If not enabled in configuration, no behaviour changes.

## Support

If you have any issues with this extension, open an issue on [GitHub](https://github.com/schmengler/DebugErrors/issues).

## Contribution

Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).

Please follow these rules while adding new features:

- pass an `SSE_DebugErrors_Exception` to `Mage::getSingleton('sse_debugerrors/errorhandler')->handle()` to trigger the error handling.
- always check if the debug errors are enabled with `Mage::helper('sse_debugerrors')->isEnabled()` and if not, do not do anything
- do not rewrite any classes, do all additional checks in observers.

## Developer

Fabian Schmengler
[http://www.schmengler-se.de](http://www.schmengler-se.de)  
[@fschmengler](https://twitter.com/fschmengler)

## License 
* see [LICENSE](https://github.com/schmengler/DebugErrors/blob/master/license.txt) file
