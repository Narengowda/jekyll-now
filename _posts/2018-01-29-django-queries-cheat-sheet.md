---
title: Django - Queries - Cheat sheet
undefined: Django quires cheat sheet
created_date: 2018-01-16 18:30:00 +0000
date: 2018-01-28 01:34:06 +0000
---
`QuerySet` can be constructed, filtered, sliced, and generally passed around without actually hitting the database. No database activity actually occurs until you do something to evaluate the queryset.

Querysets are evaluated when

1. iterated
2. slice
3. pickle
4. repr
5. len
6. list
7. bool

* Filter:Returns a new QuerySet containing objects that match the given lookup parameters.
* Exclude: Returns a new `QuerySet` containing objects that do _not_ match the given lookup parameters. In sql SELECT ...WHERE NOT..
* Annotate

```python
  >>> from django.db.models import Count
  >>> q = Blog.objects.annotate(Count('entry'))
  \# The name of the first blog
  >>> q\[0\].name
  \'Blogasaurus'
  \# The number of entries on the first blog
  >>> q\[0\].entry__count
  42
```

* order_by
* reverse
* distinct

      Entry.objects.order_by('pub_date').distinct('pub_date')
* values

      Blog.objects.values()
      <QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
      
      Blog.objects.values('id', 'name')
      <QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
      
      Blog.objects.values(lower_name=Lower('name'))
      <QuerySet [{'lower_name': 'beatles blog'}]>
* value_list

      >>> Entry.objects.values_list('id', 'headline')
      <QuerySet [(1, 'First entry'), ...]>
      >>> from django.db.models.functions import Lower
      >>> Entry.objects.values_list('id', Lower('headline'))
      <QuerySet [(1, 'first entry'), ...]>
      


* dates and datetimes

      >>> Entry.objects.dates('pub_date', 'year')
      [datetime.date(2005, 1, 1)]
      >>> Entry.objects.dates('pub_date', 'month')
      [datetime.date(2005, 2, 1), datetime.date(2005, 3, 1)]
      >>> Entry.objects.dates('pub_date', 'day')
      [datetime.date(2005, 2, 20), datetime.date(2005, 3, 20)]
      >>> Entry.objects.dates('pub_date', 'day', order='DESC')
      [datetime.date(2005, 3, 20), datetime.date(2005, 2, 20)]
      >>> Entry.objects.filter(headline__contains='Lennon').dates('pub_date', 'day')
      [datetime.date(2005, 3, 20)]
* all
* union, intersection, difference

      qs1.union(qs2, qs3)
      qs1.intersection(qs2, qs3)
      qs1.difference(qs2, qs3)
* Select_related: Returns a `QuerySet` that will “follow” foreign-key relationships, selecting additional related-object data when it executes its query. This is a performance booster which results in a single more complex query but means later use of foreign-key relationships won’t require database queries

      # Hits the database.
      e = Entry.objects.select_related('blog').get(id=5)
      
      # Doesn't hit the database, because e.blog has been prepopulated
      # in the previous query.
      b = e.blog
* Prefetch related: similar to select_realted

  `select_related` works by creating an SQL join and including the fields of the related object in the `SELECT` statement. For this reason, `select_related` gets the related objects in the same database query. However, to avoid the much larger result set that would result from joining across a ‘many’ relationship, `select_related` is limited to single-valued relationships - foreign key and one-to-one.

  `prefetch_related`, on the other hand, does a separate lookup for each relationship, and does the ‘joining’ in Python. This allows it to prefetch many-to-many and many-to-one objects, which cannot be done using `select_related`, in addition to the foreign key and one-to-one relationships that are supported by `select_related`. 