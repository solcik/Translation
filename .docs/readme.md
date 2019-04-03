# Translation

## Content
- [Usage - how to register](#usage)
- [Configuration - how to configure](#configuration)
- [Presenter - example](#presenter)
- [Latte - example](#latte)
- [Neon - example](#neon)
- [Database loaders](/database.md)

## Usage
Added translation extension.
```neon
extensions:
	translation: Translette\Translation\DI\TranslationExtension
```

## Configuration
Basic configuration.
```neon
translation:
	locales:
		whitelist: [en, cs, sk]
		default: en
	dirs:
		- %appDir%/lang
```

## Presenter
```php
<?php

declare(strict_types=1);

namespace App;

use Nette;
use Translette;


class BasePresenter extends Nette\Application\UI\Presenter
{
	/** @var Nette\Localization\ITranslator @inject */
	public $translator;

	/** @var Translette\Translation\LocalesResolvers\Session @inject */
	public $translatorSessionResolver;


	/**
	 * @param string $locale
	 */
	public function handleChangeLocale(string $locale): void
	{
		$this->translatorSessionResolver->setLocale($locale);
		$this->redirect('this');
	}


	public function renderDefault(): void
	{
		$this->translator->translate('domain.message');

		$prefixedTranslator = $this->translator->createPrefixedTranslator('domain');

		$prefixedTranslator->translate('message');
	}
}
```

## Latte
```latte
{_domain.message}

{_domain.message, $count}

{_domain.message, [name => "Ales"]}

{translator domain}
	{_message}

	{_message, $count}

	{_message, [name => "Ales"]}
{/translator}

{var $myMessage = 'domain.message'}
{$myMessage|translate}
```

## Neon
File name format.
```
        locale
          |
         /--\
messages.en_US.neon
\______/       \__/
   |            |
 domain     extension
```

File content format.
```neon
prefix:
	for: "message" # messages.prefix.for
```