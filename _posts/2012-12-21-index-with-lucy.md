---
layout: post
title: "Index with Lucy"
description: ""
category: programming
tags: [Lucy, Search, Perl]
---
{% include JB/setup %}
Creating an index of searchable documents in Perl is very simple when using a software package such as Lucy. In this post I'll outline the basic formula to creating an indexer. A follow-up post will include details on how to search such an index.

The first step is to install Lucy:

    cpanm Lucy

Once Lucy is installed we need to create an index of some documents. We'll call this the indexer and we'll incorporate it into a script. Below is how we'll begin to construct such a script:

     #! /usr/bin/env perl
     use strict;
     use warnings;

     use Lucy::Plan::Schema;

     my $schema = Lucy::Plan::Schema->new;

The first thing we do is create a schema object. The schema is what we'll use to define an index, or it defines what an index will contain. For example, a schema will define which fields are searchable and what type of data this field will contain. Here is an example of what our document may look like:

    {
	id => 1,
	   author => 'John Doe'
	   	  description => 'A very long book',
		  	      ISBN => 123456AB,
			      }

In order to setup a schema we need to identify which fields we want to be searchable and what type of fields we have in our index. We do this by calling the Schema object's spec_field method. So to setup the above example we would do the following:

   $schema->spec_field( name => 'id', type => $string_type );
   $schema->spec_field( name => 'author', type => $full_text_type );
   $schema->spec_field( name => 'description', type => $full_text_type );
   $schema->spec_field( name => 'isbin', type => $full_text_type );

The method spec_field needs a "name" and what "type" it will be. However, it begs the question, what is $full_text_type is and $string_type? Lucy offers a set of field types which indicate to the schema how to handle the field and it's content. For example, we create a $full_text_type in the following manner:

    my $polyanalyzer = Lucy::Analysis::PolyAnalyzer->new( language => 'en');
    my $full_text_type = Lucy::Plan::FullTextType( analyzer => $polyanalyzer );

The preceding code will setup a full text type that is searchable. A FullTextType will make the entire contents of the field searchable. In order for the string to be searchable it needs to contain an analyzer. The analyzer equips the FullTextType with methods to normalize the text. The polyanalyzer is a one size fits all analyzer which will stem words, tokenize the words, and do case folding, these items will probably be a topic of a future post.

Once a schema object is fully initialized with $full_text_type, we can begin indexing documents. First we need to create an actual indexer:

     my $indexer = Lucy::Index::Indexer->new(
         index    => '/path/to/index',
	     schema   => $schema,
	         create   => 1,
		     truncate => 1,
		     );

As we can see to create an indexer we feed it a schema object along with some needed attributes. These attributes indicate where on the file system the index will live, should it be created, and should it be truncated upon creation.

Finally we can iterate over the documents and call the indexers add_doc to build up what we want to commit to the index. If we had all of our documents in an array below is a hypothetical way of feeding the indexer documents:

	for my $doc (@docs) {
	    $indexer->add_doc($doc);
	    }

And once the indexing is is done we should commit:

    $indexer->commit;

As we can see with just a few lines of code we can create an index and feed it documents with just a few lines of code. For more detail see the Lucy project check out further tutorials in which this post was inspired by.