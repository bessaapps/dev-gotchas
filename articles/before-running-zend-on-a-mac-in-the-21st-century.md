# Before Running Zend on a Mac in the 21st Century

If your machine isn’t collecting dust, most likely you’re running PHP 8. Why you should ever need it? Well, that’s another story. To run Zend, however, you’ll need to downgrade, yes, downgrade to PHP 7. You’ll also have to use an older version of Composer to install your dependencies.

You can use brew to downgrade your PHP installation to version 7. If you don’t have brew, you’ll need to install it. Believe me! It’s worth it! Open a terminal and run the following commands:
```
brew tap shivammathur/php
brew install shivammathur/php/php@7.3
brew unlink php
brew link php@7.4
```

You can check your PHP version by running:
```
php ---version
```

Next, go way back to 2016 when version 1 of Composer was released. If you haven’t already, install Composer to install your Zend application and its dependencies. Before you use it though, be sure to run the following command that rolls your version back to 1:
```
composer self-update --1
```
