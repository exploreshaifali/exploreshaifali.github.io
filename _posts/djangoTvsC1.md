# Django - Choosing TextField over CharField

A couple of months back while working on database design at HashGrowth, we noticed that for a couple of CharFields(of a Django model), we don't have any upper limit for length of characters. Also we don't expect those fields to be long enough to use TextField. Because there was no `max_length` restriction, we thought of using TextField instead of CharField, if and only if there is no performance difference when using either. So we wanted to know if performance affect by any means while using any of them.

###Disclaimer

We concluded that - Django's TextField and CharField with PorstgreSql database have no difference in terms of performance. 

Here I am writing down the whole postmortem story and how we reached to the conclusion that TextField does not affect to performance by any means.

### The Beginning


At first place, we checked out Django docs, for CharField[1] it says:  

   
      A string field, for small- to large-sized strings.
      
      For large amounts of text, use TextField.
      
      The default form widget for this field is a TextInput.
      
      CharField has one extra required argument max_length.
      
      The maximum length (in characters) of the field. The max_length is enforced at the database level and in Django’s validation.

 and following lines for TextField[2]: 

```
    A large text field. The default form widget for this field is a Textarea.
    
    If you specify a max_length attribute, it will be reflected in the Textarea widget of the auto-generated form field. However it is not enforced at the model or database level. Use a CharField for that.
```
 
Basically it says that CharField should be used for small string and TextField for larger strings. The only difference it mention is, the restriction of `max_length` argument in CharField, which is to enforce the maximum length of string at database level and in Django validation in case of CharField. In case of TextField it is optional and if used, it does not enforced at the model or database level.

So far, all good. Django doc does not talk anything about performance of TextField and CharField.

### Back to zero!

We thought to start from zero, listed down the two primary char types, standard SQL[3] offer:

1.  **character(n)**: Fixed-width n-character string, padded with spaces if needed. In this case the length of block is always constant irrespective of the length of string(number of characters) you put in that block. For eg, if you create a character field with length 10 and you put a two characters string in that field, the space of 10 characters will be consumed on disk, rest 8 characters will be filled by space.  
2.  **varchar(n)**: character varying, i.e, variable-width string with a maximum size of n characters. In this type, the amount of disk consumed will be equal to the length of your input string. Though the string size should never be greater than the `max_length` specified while creating the field.

Out of these two, it felt like Django uses varchar for both CharField and TextField to ensure the space isn't wasted.
 
### The fun begins!
But what char types PostgreSql provide? We listed them as well. According to Postgres doc[4], 3 character types it provides:  

1. character(n)(fixed-length, blank padded)  
2. character varying(n), varchar(n)(variable-length with limit), here n is optional.  
3. text(variable unlimited length)

Also, Postgres docs[4] clearly mentions that there is no difference in `text` and `varcar(!n)`(that is varing character when n is not specified) fields; in terms of both performance and space. Okay! This is interesting! Postgres provide a built-in character type "text"  with same performance as like "varchar".(Eyebrow raised. right?)

After looking into Postegres char fields, it felt like Postgres' `text` field can serve to Django for `TextField`. Lets see what Django exactly uses to create `CharField` and `TextField` with Postgres. But where? Well what passionate programmers always think about? Code! So we checked out Django code for `TextField` and `CharField` with Postgres database. It looks like[5]:

  
```
   class DatabaseWrapper(BaseDatabaseWrapper):
    vendor = 'postgresql'
    # This dictionary maps Field objects to their associated PostgreSQL column
    # types, as strings. Column-type strings can contain format strings; they'll
    # be interpolated against the values of Field.__dict__ before being output.
    # If a column type is set to None, it won't be included in the output.
    data_types = {
        'AutoField': 'serial',
        'BigAutoField': 'bigserial',
        'BinaryField': 'bytea',
        'BooleanField': 'boolean',
        'CharField': 'varchar(%(max_length)s)',
        .....
        'TextField': 'text',
        ....
```

So it is clear that Django uses postgres' `text` field for its model TextField and postgres' `varchar(n)` for its model `CharField`, which does not have any performance difference in Postgres thus no performance difference in Django as well because Django simply maps Postgres fields to its model fields. 

### Yet another proof
If you find django code a bit of mummo jummo, and don't want to look into it more deeply, here is another trick to reach the final conclusion, run command  

 ```  
    python manage.py sqlmigrate <app_name> <migration version number>
```  
    
 to generate sql statements that Django will use to create tables in database, You will see that Django uses postgres' `varcar` for Django's `CharField` and postgres' `text` for Django's `TextField`.


[1]: https://docs.djangoproject.com/en/1.10/ref/models/fields/#charfield
[2]: https://docs.djangoproject.com/en/1.10/ref/models/fields/#textfield
[3]: https://en.wikipedia.org/wiki/SQL#Data_types
[4]: https://www.postgresql.org/docs/9.5/static/datatype-character.html 
[5]: https://github.com/django/django/blob/master/django/db/backends/postgresql/base.py#L90
[6]: https://docs.djangoproject.com/en/1.10/ref/databases

