# django-rest-framework-multifieldfilter
It is sometimes desirable to have a single query parameter search multiple specific fields.  Django Rest Framework already has the SearchFilter, but that only allows for a single query parameter that searches multiple fields.

Consider an API where you want to search for a person in a database, but their first name, last name, and emails are all in different fields.  You want to just add a query parameter "person=Graham" and have that return results if any of these fields match.  See the example below:

```python
class PersonViewSet(viewsets.ModelViewSet):
    queryset = Person.objects.all()
    serializer = PersonSerializer
    filter_backends = (MultiFieldFilter,)
    multi_field_filters = {'person':['first_name__icontains','last_name__icontains','email__icontains']}
```

This example is trivial and SearchFilter would probably be fine, but for more complex models you may want to have a few different available query parameters that search different fields.  For example, a project which has a name, description, and owner may define filters such as:

```python
multi_field_filters = {
  'project':['name__icontains','description__icontains'],
  'owner':['owner__first_name__icontains','owner__last_name__icontains','owner__email__icontains']
  }
```
