---
cover: >-
  https://t3.ftcdn.net/jpg/02/50/95/20/360_F_250952030_oZAZskeVPjUTWlPSCKaz2gRQqEB5adCE.jpg
coverY: 0
---

# Normalization

Database normalization is a process used to design tables in a relational database in order to minimize the duplication of information and to ensure good data integrity. This means that data is kept complete, accurate, consistent, and in context. Data integrity is key to ensuring the data is actually useful to its owner.

If the same information is stored in different places in multiple tables, the risk of making the database inconsistent increases significantly. If a person's address is stored in multiple tables, and the address is changed in just one table, it is no longer possible to know which address is correct just by looking at the data. The database has lost data integrity.

It is common to refer to four levels (forms) of normalization in database modeling, levels 1 to 4. There are different requirements that must be met in order to obtain a certain level. In addition, all requirements of the lower levels must be met as well.

The following example presents a simple dataset and the steps to achieve each of the normalization levels in detail, including how it can be represented in Appfarm Create.

It is worth noting that having a data model of a higher normalization level often leads to more tables (object classes in Appfarm Create). This may have an impact on performance due to the need to join different tables to find complete information.&#x20;

With Appfarm Create, this means more data sources with deep data bindings and complex filters. Therefore, in systems with very complex data structures, there may be an advantage in adopting a lower degree of normalization.

## Example: Sales records <a href="#example-sales-records" id="example-sales-records"></a>

This example features a dataset containing customers who have bought different items from different suppliers. The customers have also subscribed to different newsletters from the different suppliers.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FzfzJPyjUER8IY8Z3iDqm%252Fnormalization-sales-records.png%3Falt=media%26token=8995524f-f1a2-4b84-b377-023e0fb102e4&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9ed407f065ed83432b9ef2c27703643b3bc736bf09ff1b61ddf35e81b003492c" alt=""><figcaption><p>Dataset</p></figcaption></figure>

In order to create a data model that supports this dataset in the best possible way, we will start by looking at the first level of normalization, the first normal form.

## First normal form (1NF) <a href="#first-normal-form-1nf" id="first-normal-form-1nf"></a>

#### To obtain a data model that is in the first normal form the following requirements must be met:

1. Each cell must be single-valued.
2. Entries in a column must be of the same type.
3. Rows must be uniquely identified.

The example dataset violates the first requirement of the 1NF by having several values in a single cell in the columns **Item** and **Newsletter**.&#x20;

The dataset also violates the second requirement by having the values `Unknown` and `No number` in the columns **Supplier** and **Supplier Phone**. The third requirement is also not met since there is no unique identifier for each row.

The dataset below has been altered in order to satisfy the requirements for the 1NF. There are no longer cells containing more than one value, each column has data that is of the same type (`Unknown` and `No number` are replaced with proper values), and each row is now uniquely identified with the addition of the **Cust \_id** column.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FwmTsgVDdmMo2CgMnSgbV%252Fnormalization-1-records.png%3Falt=media%26token=2662bf30-caf4-411a-bf24-62181773e29e&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=97c2d007ec214d28b5c624b2bf9e60339456efba26109b215b279e10f1a886a5" alt=""><figcaption><p>Dataset</p></figcaption></figure>

The following images show what the data model for this dataset would look like in Appfarm Create.&#x20;

It is a very simple data model, consisting of only one object class: Sales Records.&#x20;

However, this data model has several issues which can be address through further normalization.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FM8HywVNDsOO4wxrT55U1%252Fnormalization-1-data-model.png%3Falt=media%26token=f0589bfd-7b68-4c21-a2e6-0d4812de24f5&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c154a98feef61a5dd431f40d9870e55511781ad467d84f4388ad06a647c8067d" alt=""><figcaption><p>Data model</p></figcaption></figure>

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FQI9wZfahzVP5RTOCiasl%252Fnormalization-1-designer.png%3Falt=media%26token=f03cdc21-b323-4c5d-a811-6792b307f228&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=4aecb1b773f571670ad2c372e0e7af0fce71570905c44a9625d443e8189853d6" alt=""><figcaption><p>Data model designer</p></figcaption></figure>

## Second normal form (2NF) <a href="#second-normal-form-2nf" id="second-normal-form-2nf"></a>

#### To obtain a data model that is in the second normal form the following requirement must be met:

1. All attributes (non-ID columns) must be dependent on the key.

If you look at the non-ID columns in the dataset, are there any columns that are not dependent on the **Cust \_id** column?&#x20;

For example, does the customer ID determine the price of the product? In other words, does the price depend on who buys the product? No. It does not matter who buys the product, the price will remain the same, so the dataset does not meet the requirements of the 2NF.

In order to fulfil the requirements of the 2NF, the columns that are not dependent on the key column must be separated.&#x20;

The **Price**, **Item**, **Supplier**, and **Supplier Phone** columns can be moved into a new table. Additionally, another new table known as a junction table, will hold the transactions.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252Fb5a1tnQsKs0WCTzgt1Ok%252Fnormalization-2-records.png%3Falt=media%26token=33eb3fb8-835f-403f-ab9c-c37d463d3d7f&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=327265c1ea03160e6bfe4fe500ad8fc9944dcaf1bea2f0109ea2caa2d1836dc3" alt=""><figcaption><p>Dataset</p></figcaption></figure>

The data model in Appfarm Create would have three object classes, with the Sales Records object class referencing both the Customer and Product object classes.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FT4HrucbaMzaW0OtcGSDL%252Fnormalization-2-data-model.png%3Falt=media%26token=79169a4c-7a34-43e0-adcf-4473bb877011&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=06c3968487c7587452aab6274ffca9519a223f07fedca1bf8c0de15f7e7504ca" alt=""><figcaption><p>Data model</p></figcaption></figure>

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252F1BmI7DSFuSVPcQDTC9Zj%252Fnormalization-2-designer.png%3Falt=media%26token=0939671d-8ef4-4a8c-a72a-08fb15c48898&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=4bac362112ed7ff851a8407db91bf9e433c146d301346abea3bffc2eb8bf9b36" alt=""><figcaption><p>Data model designer</p></figcaption></figure>

## Third normal form (3NF) <a href="#third-normal-form-3nf" id="third-normal-form-3nf"></a>

#### To obtain a data model that is in the third normal form the following requirement must be met:

1. All fields (columns) can be determined only by the key (\_id) in the table and no other column.

Looking at the dataset, you can see that **Supplier** and **Supplier Phone** always go together. If you know the supplier, you know the supplier's phone number.&#x20;

If you added more items from Microsoft and Sony the phone numbers will keep repeating, meaning that if you want to change the supplier's phone number you have to change it in many places instead of just one.

To address this issue, you can create a new table to hold the supplier information. The Items table can then be updated to store a reference to a supplier instead of storing supplier information.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FWhsQUKjIYV0UJKD1MYpW%252Fnormalization-3-records.png%3Falt=media%26token=e08a726d-a8b7-4a5d-85c6-17a2b72f62fe&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=0dbd067149f21f19411736cceec8cc242bb51d6abc0f4f0257a1a71ea25d2e70" alt=""><figcaption><p>Dataset</p></figcaption></figure>

The data model in Appfarm Create now features four object classes, with the Product object class referencing the new Supplier object class.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FBKQulYF1DEW8AfvRZlJ4%252Fnormalization-3-data-model.png%3Falt=media%26token=e570f4fa-748c-4015-8c7f-630756d0e957&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=69d19faf4771499b05488634fdea867bfd9e2e99a0730b06325252c56c809533" alt=""><figcaption><p>Data model</p></figcaption></figure>

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FV0P1XcgH1bO7hOFQrI7Y%252Fnormalization-3-designer.png%3Falt=media%26token=82462878-e5a0-49e3-bc48-34ca77fbd8c3&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8b5ed42a7939afe617dbc79c5c096e17041f54bd57c8b6fb50c27cbf76712be5" alt=""><figcaption><p>Data model designer</p></figcaption></figure>

## Fourth normal form (4NF) <a href="#fourth-normal-form-4nf" id="fourth-normal-form-4nf"></a>

#### To obtain a data model that is on the fourth normal form the following requirement must be met:

1. No multi-valued dependencies.

Looking at the Customer table in the dataset, you can see that all columns are dependent on the key (**\_id**) column. However, there is still a problem. The customer Evan Wilson is in the Customer table twice with the same address since he has subscribed to two different newsletters.&#x20;

If he changes his address we would have to change it in two different places, and if he unsubscribes to the Dell news newsletter, should we just replace the value with `null`? What if Alan Smith in Hanover Park also subscribes to Dell news? Then his name and address will occur twice in the Customer table as well.

To address this issue the Customer table must be normalized further. You can alter the Customer table to just hold the **\_id**, **Name**, and **Shipping Address** so that names and addresses aren't stored multiple times per user.&#x20;

Then, you can create a Newsletter table that holds the name of the newsletter and a reference to its supplier. This means a supplier can have multiple newsletters.&#x20;

Finally, you can create a new junction table that holds all the customer subscriptions to newsletters by storing references to both the customers and the newsletters.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FiqdSM3pi5gthBpD79tXq%252Fnormalization-4-records.png%3Falt=media%26token=a6a85f56-016e-4c42-af2c-b1f0a6c6ebf1&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=2f3f13d523a87e60576b46ced023ec7bb0f0e03c1f84fb5b5e446baca8da0d52" alt="" width="563"><figcaption><p>Dataset</p></figcaption></figure>

The final data model in Appfarm Create consists of six object classes with the addition of Newsletter and Newsletter subscription object classes.

You can see that some properties are marked as _Required_. This means that an object of this class cannot exist without a required value set.&#x20;

For example, each supplier must have a name. In addition, the Phone property of the supplier class is marked as _Unique_ which means that a phone number can only exist once. This ensures that no suppliers will have the same phone number.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FjJBYdekKXMMOwZIVRO9h%252Fnormalization-4-designer.png%3Falt=media%26token=6d7604ae-a956-40a3-8c4c-31320ccf306b&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9c75830ba9d41e8ea398497f1ef5221306eb0b2508f32c4158280396fd2e78f1" alt="" width="563"><figcaption><p>Data model</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Data model designer</p></figcaption></figure>
