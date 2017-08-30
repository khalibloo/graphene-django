Integration with Django Rest Framework
======================================

You can re-use your Django Rest Framework serializer with
graphene django.


Mutation
--------

You can create a Mutation based on a serializer by using the
`SerializerMutation` base class:

.. code:: python

    from graphene_django.rest_framework.mutation import SerializerMutation

    class MyAwesomeMutation(SerializerMutation):
        class Meta:
            serializer_class = MySerializer

Note, if your serializer is a ModelSerializer, you'll need to wrap it in an intermediary Serializer class and then use that class instead as the `serializer_class` in your mutation class.

.. code:: python

    class MyItemModelSerializer(serializers.ModelSerializer):
        class Meta:
            model = Item
            fields = ('id', 'name', 'description',)
            
    class MyItemSerializer(serializers.Serializer):
        """
        Wraps the ModelSerializer above and can be used in as serializer_class in mutation
        """
        item = ItemModelSerializer()
        
        def create(self, validated_data):
            item_data = validated_data.pop('item')
            return {'item': Item.objects.create(**item_data)}

        # likely want to implement update too

## mention how graphene creates the mutations for you and how you can use them. eg create, update, delete
