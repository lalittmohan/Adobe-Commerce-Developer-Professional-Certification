## Describe how the ACL works with roles and resources?


**Module:** Magento_Authorisation
**Depends**

**Tables:** 

| authorization_role | authorization_rule |
| --| --|

**Extensibility:** DI,Plugins


- ACL access restriction mechanism
- ACL Resource define access for each page
- Implemented in `_isAllowed` method for ACL check

```
file: Magento\Backend\App\AbstractAction

protected function _isAllowed() // Determines whether current user is allowed to access Action
    {
        return $this->_authorization->isAllowed(static::ADMIN_RESOURCE); //Magento_Backend::admin
    }

```

*Steps:*
- Create/Add roles/resources/users
- Assign user to roles
- Allocate resource to roles

ACL Resource are defined in `acl.xml` configuration file 

```
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Acl/etc/acl.xsd">
    <acl>
        <resources>
            <resource id="Magento_Backend::admin">
                <resource id="Magento_Catalog::catalog">
                    <resource id="Magento_Catalog::catalog_inventory">
                        <resource id="Magento_Catalog::products" title="products" sortOrder="40"/>
                    </resource>
                </resource>
            </resource>
        </resources>
    </acl>
</config>
```


**Role Scope:** 

| Global | website | store |
| --| --| --|
| All | website| store |
|All | gws_website | gws_store_group |






