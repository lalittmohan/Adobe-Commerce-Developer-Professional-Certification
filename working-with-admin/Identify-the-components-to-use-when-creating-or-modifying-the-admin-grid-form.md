## Identify the components to use when creating or modifying the admin grid/form

**Admin Controller:**
```
Magento\Backend\App\AbstractAction
         extends
Magento\Backend\App\Action|controller
        extends
    Your Controller
```

**System Configuration:**

**Grid:** 
- mass actions
- paging
- button area
- filtering
- data area
- sorting


### UI Component

**scheme**  page configurations  `urn:magento:module:Magento_Ui:etc/ui_configuration.xsd`

***Generation flow :***

* `\Magento\Framework\View\Layout\Generator\UiComponent::generateComponent` - create, prepare, wrap
* create component `\Magento\Framework\View\Element\UiComponentFactory::create`
    * `\Magento\Framework\View\Element\UiComponentFactory::mergeMetadata` - components definitions in array format + dataProvider.getMeta
        + metadata = ['cert_form' => ['children' => dataProvider.getMeta]]
    - for each child[],`\Magento\Framework\View\Element\UiComponentFactory::createChildComponent` recursively:
        + create PHP class for component, e.g. `new Magento\Ui\Component\DataSource($components = [])`;
    * create PHP class for component, e.g. `new Magento\Ui\Component\Form($components = [children...])`;
- prepare component recursively
    * getChildComponents[].prepare - update data, js_config etc.
- wrap to *UiComponent\Container* as child 'component'
    * toHtml = render -> renderEngine->render (template + '.xhtml')

## AbstractComponent

*component PHP class common arguments:*

- name
- template -> .xhtml
- layout: generic - default/tabs. DI config Magento\Framework\View\Layout\Pool
- config:
  * value
- js_config:
  * component - JS component class
  * extends - by default context.getNamespace - top level UI component instance name, e.g. 'cms_block_form'
  * provider - context.addComponentDefinition - adds to "types"
