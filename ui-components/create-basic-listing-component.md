# Create Basic Listing Component

Create a basic listing component to show on a Magento Admin page.

## Kata Solution

Solution consists of three parts:

- create a UI component XML definition
- create a data provider
- add the UI component to Magento Admin page

### Data Provider

```xml
<?xml version="1.0"?>
<!-- MageKata: Create Basic Listing Component -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- Extends SearchResult to use it as a data provider for the listing  -->
    <virtualType name="Glushko\MageKata\Ui\Listing\VirtualSearchResult" type="Magento\Framework\View\Element\UiComponent\DataProvider\SearchResult">
        <arguments>
            <argument name="mainTable" xsi:type="string">magekata_entity</argument>
            <argument name="resourceModel" xsi:type="string">Glushko\MageKata\Model\ResourceModel\EntityResource</argument>
            <argument name="identifierName" xsi:type="string">entity_id</argument>
        </arguments>
    </virtualType>
    <!-- Adds configured search result to DataProvider's CollectionFactory -->
    <type name="Magento\Framework\View\Element\UiComponent\DataProvider\CollectionFactory">
        <arguments>
            <argument name="collections" xsi:type="array">
                <item name="magekata_ui_listing_data_source" xsi:type="string">Glushko\MageKata\Ui\Listing\VirtualSearchResult</item>
            </argument>
        </arguments>
    </type>
</config>
```

### UI Component XML Difinition

`view/adminhtml/ui_component/magekata_ui_listing.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- MageKata: Create Basic Listing Component -->
<listing xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Ui:etc/ui_configuration.xsd">
    <argument name="data" xsi:type="array">
        <item name="js_config" xsi:type="array">
            <item name="provider" xsi:type="string">magekata_ui_listing.magekata_ui_listing_data_source</item>
        </item>
    </argument>
    <settings>
        <spinner>magekata_ui_columns</spinner>
        <deps>
            <dep>magekata_ui_listing.magekata_ui_listing_data_source</dep>
        </deps>
    </settings>
    <!-- Definition of data provider on UI component side -->
    <dataSource name="magekata_ui_listing_data_source" component="Magento_Ui/js/grid/provider">
        <settings>
            <storageConfig>
                <param name="indexField" xsi:type="string">entity_id</param>
            </storageConfig>
            <updateUrl path="mui/index/render"/>
        </settings>
        <aclResource>Glushko_MageKata::ui</aclResource>
        <dataProvider class="Magento\Framework\View\Element\UiComponent\DataProvider\DataProvider" name="magekata_ui_listing_data_source">
            <settings>
                <requestFieldName>id</requestFieldName>
                <primaryFieldName>entity_id</primaryFieldName>
            </settings>
        </dataProvider>
    </dataSource>
    <!-- Difinition of listing columns -->
    <columns name="magekata_ui_columns">
        <selectionsColumn name="ids">
            <settings>
                <indexField>entity_id</indexField>
                <resizeEnabled>false</resizeEnabled>
                <resizeDefaultWidth>55</resizeDefaultWidth>
            </settings>
        </selectionsColumn>
        <column name="entity_id">
            <settings>
                <filter>textRange</filter>
                <label translate="true">ID</label>
                <sorting>desc</sorting>
            </settings>
        </column>
        <column name="title">
            <settings>
                <filter>text</filter>
                <label translate="true">Title</label>
            </settings>
        </column>
    </columns>
</listing>
```

### Add UI Component to Magento Admin page

`view/adminhtml/layout/magekata_ui_index.xml`:

```xml
<?xml version="1.0"?>
<!-- MageKata: Create Basic Listing Component -->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceContainer name="content">
            <uiComponent name="magekata_ui_listing"/> <!-- the same name as in UI component difinition -->
        </referenceContainer>
    </body>
</page>
```
