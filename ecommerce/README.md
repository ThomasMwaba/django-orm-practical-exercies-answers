## Django orm practical exercises and answers


#### Setup For Windows

1. Setup a virtual environment **virtualenv venv**

2. Activate the environment **venv\Scripts\activate**

3. Install all the required dependencies **pip install -r requirements.txt**

4. Migrate the data to the database **python manage.py demo-fixtures**

# 1. Retrieve a product sub-products

``` 
from ecommerce.inventory.models import Product, ProductInventory

ProductInventory.objects.filter(product_id=1)

```

# 2.  Retrieve a product featured image

```
from ecommerce.inventory.models import Media

x = Media.objects.filter(is_feature=True).filter(product_inventory__product_id__name="test")

x = Media.objects.all().count()

x = Media.objects.raw("SELECT * from inventory_media INNER JOIN inventory_productinventory ON inventory_media.product_inventory_id = inventory_productinventory.id")

x = Media.objects.raw("SELECT * from inventory_media INNER JOIN inventory_productinventory ON inventory_media.product_inventory_id = inventory_productinventory.id WHERE inventory_media.is_feature=True AND inventory_productinventory.product_id=1")


```



# 3.  Retrieve all values associated to a sub-product

```
from ecommerce.inventory.models import Media

x = ProductAttributeValue.objects.filter(product_attribute_values__id=1)

x = ProductAttributeValue.objects.raw("SELECT * FROM inventory_productattributevalue INNER JOIN inventory_productattributevalues ON inventory_productattributevalue.id = inventory_productattributevalues.attributevalues_id WHERE inventory_productattributevalues.productinventory_id=1")


```




# 4. Retrieve the product attributes for a given product type

```
x = ProductAttribute.objects.filter(product_type_attributes=1)

x = ProductAttribute.objects.raw("SELECT * FROM inventory_productattribute INNER JOIN inventory_producttypeattribute ON inventory_productattribute.id = inventory_producttypeattribute.product_attribute_id WHERE inventory_producttypeattribute.product_type_id = 1")


```


# 5. Retrieve all products associated to product attribute id:1

```
from ecommerce.inventory.models import Product

x = Product.objects.filter(product__attribute_values__product_attribute__id=1).distinct().count()

x = Product.objects.raw("SELECT * FROM inventory_product INNER JOIN inventory_productinventory ON(inventory_product.id = inventory_productinventory.product_id) INNER JOIN inventory_productattributevalues ON (inventory_productinventory.id = inventory_productattributevalues.productinventory_id) INNER JOIN inventory_productattributevalue ON (inventory_productattributevalues.attributevalues_id = inventory_productattributevalue.id) WHERE inventory_productattributevalue.product_attribute_id = 1")


```


