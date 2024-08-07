## Django orm practical exercises and answers


#### Setup For Windows

1. Setup a virtual environment **virtualenv venv**

2. Activate the environment **venv\Scripts\activate**

3. Install all the required dependencies **pip install -r requirements.txt**

4. Migrate the data to the database **python manage.py demo-fixtures**

5. To Write the queries below on the terminal type **python manage.py shell**

# 1. Retrieve a product sub-products

``` 
from ecommerce.inventory.models import ProductInventory

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
from ecommerce.inventory.models import  ProductAttributeValue

x = ProductAttributeValue.objects.filter(product_attribute_values__id=1)

x = ProductAttributeValue.objects.raw("SELECT * FROM inventory_productattributevalue INNER JOIN inventory_productattributevalues ON inventory_productattributevalue.id = inventory_productattributevalues.attributevalues_id WHERE inventory_productattributevalues.productinventory_id=1")


```




# 4. Retrieve the product attributes for a given product type

```
from ecommerce.inventory.models import ProductAttribute

x = ProductAttribute.objects.filter(product_type_attributes=1)

x = ProductAttribute.objects.raw("SELECT * FROM inventory_productattribute INNER JOIN inventory_producttypeattribute ON inventory_productattribute.id = inventory_producttypeattribute.product_attribute_id WHERE inventory_producttypeattribute.product_type_id = 1")


```


# 5. Retrieve all products associated to product attribute id:1

```
from ecommerce.inventory.models import Product

x = Product.objects.filter(product__attribute_values__product_attribute__id=1).distinct().count()

x = Product.objects.raw("SELECT * FROM inventory_product INNER JOIN inventory_productinventory ON(inventory_product.id = inventory_productinventory.product_id) INNER JOIN inventory_productattributevalues ON (inventory_productinventory.id = inventory_productattributevalues.productinventory_id) INNER JOIN inventory_productattributevalue ON (inventory_productattributevalues.attributevalues_id = inventory_productattributevalue.id) WHERE inventory_productattributevalue.product_attribute_id = 1")


```


# 6. Retrieve all sub-products that has less than 50 units left in stock

```
from ecommerce.inventory.models import ProductInventory

x = ProductInventory.objects.filter(product_inventory__units__lte=50)

x = ProductInventory.objects.raw("SELECT * FROM inventory_productinventory INNER JOIN inventory_stock ON (inventory_productinventory.id = inventory_stock.product_inventory_id) WHERE inventory_stock.units <= 50")

```


# 7. Retrieve all sub-products which have been stock checked in the last month

```
from ecommerce.inventory.models import ProductInventory

x = ProductInventory.objects.filter(product_inventory__last_checked__range=('2020-09-01','2022-10-10'))

x = ProductInventory.objects.raw("SELECT * FROM inventory_productinventory INNER JOIN inventory_stock ON (inventory_productinventory.id = inventory_stock.product_inventory_id) WHERE inventory_stock.last_checked BETWEEN '2020-01-01 00:00:00' AND '2022-10-10 00:00:00'")

```


# 8. Retrieve all sub-products which have been stock checked in the last month

```

from ecommerce.inventory.models import Product

x = Product.objects.filter(product__product_type__product_type_attributes__id=1).distinct().count()

x = Product.objects.raw("SELECT DISTINCT inventory_product.id FROM inventory_product INNER JOIN inventory_productinventory ON (inventory_product.id = inventory_productinventory.product_id) INNER JOIN inventory_producttype ON (inventory_productinventory.product_type_id = inventory_producttype.id) INNER JOIN inventory_producttypeattribute ON (inventory_producttype.id = inventory_producttypeattribute.product_type_id) WHERE inventory_producttypeattribute.product_attribute_id = 1")

```



# 9.  Retrieve all woman shoes by the xyz brand


```

from ecommerce.inventory.models import Brand


 x = Brand.objects.filter(brand__product_type__product_type_attributes__id=1).distinct()

x = Product.objects.raw("SELECT DISTINCT inventory_brand.id, inventory_brand.name FROM inventory_product INNER JOIN inventory_productinventory ON (inventory_brand.id = inventory_productinventory.brand_id) INNER JOIN inventory_producttype ON (inventory_productinventory.product_type_id = inventory_producttype.id) INNER JOIN inventory_producttypeattribute ON (inventory_producttype.id = inventory_producttypeattribute.product_type_id) WHERE inventory_producttypeattribute.product_attribute_id = 1")
```

# 10. Retrieve all sub-products which have been stock checked in the last month

```

from ecommerce.inventory.models import Product

x = Product.objects.filter(product__brand_id=1)

x = Product.objects.raw("SELECT * FROM inventory_product INNER JOIN inventory_productinventory ON (inventory_product.id = inventory_productinventory.product_id) WHERE inventory_productinventory.brand_id = 1")

```
