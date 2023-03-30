# Manipulate composer.json with Beautiful Object API

The original code for this repository is now marked as deprecated, I use this fork simply to avoid seeing the deprecation warning.

see https://github.com/deprecated-packages/composer-json-manipulator

- load to `composer.json` to an object
- use handful methods
- merge it with others
- print it back to `composer.json` in human-like format

## Install

```bash
composer require symplify/composer-json-manipulator
```

Add to your `config/config.php`:

```php
use Symfony\Component\DependencyInjection\Loader\Configurator\ContainerConfigurator;
use Symplify\ComposerJsonManipulator\ValueObject\ComposerJsonManipulatorConfig;

return static function (ContainerConfigurator $containerConfigurator): void {
    $containerConfigurator->import(ComposerJsonManipulatorConfig::FILE_PATH);
};
```

## Usage

```php
namespace App;

use Symplify\ComposerJsonManipulator\ComposerJsonFactory;

class SomeClass
{
    /**
     * @var ComposerJsonFactory
     */
    private $composerJsonFactory;

    public function __construct(ComposerJsonFactory $composerJsonFactory)
    {
        $this->composerJsonFactory = $composerJsonFactory;
    }

    public function run(): void
    {
        // ↓ instance of \Symplify\ComposerJsonManipulator\ValueObject\ComposerJson
        $composerJson = $this->composerJsonFactory->createFromFilePath(getcwd() . '/composer.json');

        // Add a PRS-4 namespace
        $autoLoad = $composerJson->getAutoload();
        $autoLoad['psr-4']['Cool\\Stuff\\'] = './lib/';
        $composerJson->setAutoload($autoLoad);
        $this->jsonFileManager->printComposerJsonToFilePath($composerJson, $composerJson->getFileInfo()->getRealPath());
    }
}
```

