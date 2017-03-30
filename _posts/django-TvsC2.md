# Overcome the myth - "CharField is better than TextField"


Imagine yourself wearing a black suit and a gun in your hand like James Bond, we are on the mission to investigate the truth of CharField and TextField in 'Django with Postgres'.

Problem Statement: Many people believes that the CharField is better to use in terms of space, performance than TextField. But the James Bond sittng inside my heart and mind does not believe same, he wants proof! proof! proof!

Okay lets begin the investigation. Our two suspects are CharField and TextField. Django doc, standard SQL, Postgres doc would be our leads who can help us in our investigation.

My Bond mind ask me to checkout Django docs very first and verify if really TextField are less efficient or consume more space of my database.

Django documentation have following lines for CharField[1]:  

   
      A string field, for small- to large-sized strings.
      
      For large amounts of text, use TextField.
      
      The default form widget for this field is a TextInput.
      
      CharField has one extra required argument max_length

Following lines for TextField[2]: 

```
    A large text field. The default form widget for this field is a Textarea.
    
    If you specify a max_length attribute, it will be reflected in the Textarea widget of the auto-generated form field. However it is not enforced at the model or database level. Use a CharField for that.
```
 
Basically it says that CharField should be used for small string and TextField for larger strings. The only difference it mention is, the restriction of `max_length` argument in CharField, which is to enforce the maximum lenght of string at database level and in Django valdiation in case of CharField. In case of TextField it is optional and if used, it does not enforced at the model or database level.

So, Django doc does not mention any offence against TextField for space and performance. Cool. Now what? The question is - what's the root cause of this confusion? Why this confusion occured? Why do I think that CharField is better?

Amm....I know only about the char types of standard SQL[3](our second lead of the case). It primarily offers two character types: **character(n)**(fixed-width n-character string, padded with spaces as needed) and another is **varchar(n)**(i.e, character varying, variable-width string with a maximum size of n characters). Okay so it feels like Django uses varchar for both CharField and TextField to ensure the space isn't wasted. But wait, *Agent M* want to know what are character types that Postgres offer? Lets checkout our third lead(Postgres docs).

According to Postgres doc[4], 3 character types it provides - character(fixed-length, blank padded), varchar(variable-length with limit) and text(variable unlimited length). Also, it clearly mentions that there is no difference in text and varcar(!n)(that is varing character when n is not specified) fields; in terms of both performance and space. Ohhhhh!  My eyebrow also raised up(yours too, right?) when I saw that Postgres provide a built-in character type "text"  with same performance as like varchar.

Now it feels like this Postgres' `text` field can serve to Django for TextField and my Bond's mind wants proof. Lets see what Django exactly uses to create CharField and TextFiend with Postgres. But we verified all our leads(Django doc, SQL standards, Postges doc, who gave important informations to reach here) and case isn't solved yet. There is still a question what's exactly Django uses for CharField and TextField. Django documentation does not give any information for same, now what?

![alt text](james.jpg "Don't Stop when you are tired, when you a are done")



Well *Agent M* told Bond to checkout Django's Code[5](that could be our another lead), which looks like:  
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


If you find django code a bit of mummo jummo, and don't want to look into it more deeply, here is another trick to reach the final conclusion and satisfy *Agent M*, run command  

 ```  
    python manage.py sqlmigrate <app_name> <migration version number>
```  
    
 to generate sql statements that Django will use to create tables in our database we will see that Django uses postgres' varcar for Django's CharField and postgres' text for Django's TextField.



[1]: https://docs.djangoproject.com/en/1.10/ref/models/fields/#charfield
[2]: https://docs.djangoproject.com/en/1.10/ref/models/fields/#textfield
[3]: https://en.wikipedia.org/wiki/SQL#Data_types
[4]: https://www.postgresql.org/docs/9.5/static/datatype-character.html 
[5]: https://github.com/django/django/blob/master/django/db/backends/postgresql/base.py#L90

https://docs.djangoproject.com/en/1.10/ref/databases
