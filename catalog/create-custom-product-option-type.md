# Create Custom Product Option Type

Add an ability to show show informational custom options on the product view page.

## Kata Solution

Description of the kata solution.

`etc/product_options.xml`:

```xml
<?xml version="1.0"?>
<!-- MageKata: Create Custom Product Option Type -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Catalog:etc/product_options.xsd">
    <!-- Define a new option type -->
    <option name="information" label="Information" renderer="Glushko\MageKata\Block\Product\Option\Type\InformationCustomType">
        <inputType name="disabled_drop_down" label="Disabled Drop-Down" />
    </option>
</config>
```

`Glushko/MageKata/Ui/Product/Form/InformationCustomOptionModifier.php`:

```php
<?php
/**
 * MageKata: Create Custom Product Option Type
 */

declare(strict_types=1);

namespace Glushko\MageKata\Ui\Product\Form;

use Magento\Catalog\Ui\DataProvider\Product\Form\Modifier\CustomOptions;
use Magento\Framework\Stdlib\ArrayManager;
use Magento\Ui\Component\Form\Element\DataType\Text;
use Magento\Ui\Component\Form\Element\Input;
use Magento\Ui\Component\Form\Field;
use Magento\Ui\DataProvider\Modifier\ModifierInterface;

/**
 * Data provider for Information Product Option Type
 */
class InformationCustomOptionModifier implements ModifierInterface
{
    /**
     * Placeholder field name
     */
    const FIELD_PLACEHOLDER_NAME = 'placeholder';

    /**
     * @var ArrayManager
     */
    protected $arrayManager;

    /**
     * @param ArrayManager $arrayManager
     */
    public function __construct(
        ArrayManager $arrayManager
    ) {
        $this->arrayManager = $arrayManager;
    }

    /**
     */
    public function modifyData(array $data)
    {
        return $data;
    }

    /**
     * Define Information Option Type definition and definition of placeholder field which is a custom field needed to
     * configure this types of options
     *
     * @return array
     */
    public function modifyMeta(array $meta)
    {
        // add placeholder field configuration to container_type_static component
        $meta = $this->arrayManager->merge(
            'custom_options/children/options/children/record/children/container_option/children/' .
            'container_type_static/children',
            $meta,
            [
                'placeholder' => $this->getPlaceholderConfig(100),
            ]
        );

        // add information type configuration
        $meta = $this->arrayManager->merge(
            'custom_options/children/options/children/record/children/' .
            'container_option/children/container_common/children/type/arguments/data/config/groupsConfig',
            $meta,
            [
                'information' => $this->getInformationTypeConfig(),
            ]
        );

        return $meta;
    }

    /**
     * Retrieve definition of information group
     *
     * @return array
     */
    private function getInformationTypeConfig(): array
    {
        return [
            'values' => ['disabled_drop_down'],
            'indexes' => [
                CustomOptions::CONTAINER_TYPE_STATIC_NAME,
                static::FIELD_PLACEHOLDER_NAME
            ]
        ];
    }

    /**
     * Get config for "Placeholder" field
     *
     * @param int $sortOrder
     *
     * @return array
     */
    private function getPlaceholderConfig(int $sortOrder): array
    {
        return [
            'arguments' => [
                'data' => [
                    'config' => [
                        'label' => 'Placeholder',
                        'component' => 'Magento_Catalog/js/components/custom-options-component',
                        'componentType' => Field::NAME,
                        'formElement' => Input::NAME,
                        'dataScope' => static::FIELD_PLACEHOLDER_NAME,
                        'dataType' => Text::NAME,
                        'sortOrder' => $sortOrder,
                    ],
                ],
            ],
        ];
    }
}
```