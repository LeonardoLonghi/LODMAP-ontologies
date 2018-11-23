Bubble Graph Ontology (BGO)
==========================

The Bubble Graph Ontology is a data model suitable to visualize **Account**s as a blob of **Bubble**s. The bubble area should be, more or less, proportional to the account value. The color of the bubble depends from the change against a previous value.

The namespace for BGO is *http://linkeddata.center/lodmap-bgo/v1#*

The suggested prefix for the BGO namespace is *bgo*

BGO is modelled around two classes:
- **Account**  that can be representd with a bubble 
- **BubbleGraph** that is normally represented as blob of bubbles

BGO is expressed in a [owl file](bgo.owl) serialized as RDF xml. You can edit the file by hand or using [Protégé](httpsbgo://protege.stanford.edu/)

Note that BGO objective is to define the data model for a data visualization tool, NOT the meaning of the represented data.
An a Account can be used to visualize a Financtial Report fact or any versionable quantitative value. The data semantic should be linked using *dct:source propery*.

This is an example of a linked data resource containing a bgo:BubbleGraph specification:

```
@prefix dct: <http://purl.org/dc/terms/> .
@prefix bgo: <http://linkeddata.center/lodmap-bgo/v1#> .
@base <http://lodmap-bgo.example.com/api/v1/> .

<accounts> foaf:primaryTopicOf <accounts#bubbleGraph>.

<accounts#bubbleGraph> a bgo:BubbleGraph ;
    dct:title "Balance of 2017"@en ;
    dct:description "A bubble graph representation of a balance"@en ;
    dct:source <http://example.org/resource/dataset_uri> ;
    bgo:um "EUR" ;
    bgo:partition 
        <accounts#p1>,
        <accounts#p2>,
        <accounts#p3>;
    bgo:partitionOrderedList ( 
        <accounts#p1>,
        <accounts#p2>,
        <accounts#p3>
    )
.

<account/a1#data> 
    bgo:inBubbleGraph <accounts#bubbleGraph> ;
    dct:subject "this is the path" ;
    dct:title "the first fact"@en ;
    bgo:partitionLabel "p1", "p2";
    bgo:amount 1000.00 ;
    bgo:previousValue 800.00
.
<account/a2#data> 
    bgo:inBubbleGraph <accounts#bubbleGraph> ;
    dct:subject "this is the path" ;
    dct:title "the second fact"@en ;
    bgo:partitionLabel "p1", "p3";
    bgo:amount 10000.00 ;
    bgo:previousValue 8000.00 
.

<accounts#partitions>  
.        

<accounts#p1> bgo:label "p1"; dct:title "partition1".
<accounts#p2> bgo:label "p2"; dct:title "partition2".
<accounts#p3> bgo:label "p3"; dct:title "partition3".
```

This is an example of a linked data resource representing a single  bgo:Account:

```
@prefix dct: <http://purl.org/dc/terms/> .
@prefix bgo: <http://linkeddata.center/lodmap-bgo/v1#> .
@base <http://lodmap-bgo.example.com/api/v1/> .


<account/a1> foaf:primaryTopicOf <account/a1#data>.

<account/a1#data> a bgo:Account ;
    bgo:inBubbleGraph <accounts#bubbleGraph> ;
    dct:title "the first fact"@en ;
    dct:source <http://example.org/resource/account_a1> ;
    dct:description "This is the description of the account a1" ;
    dct:subject "this is the path" ;
    dct:date "2018";
    bgo:partitionLabel "p1", "p2";
    bgo:amount 1000.00 ;
    bgo:previousValue 800.00  ;
    bgo:isVersionOf
        [ dct:date "2017"; bgo:amount 800.00 ],
        [ dct:date "2016"; bgo:amount 1100.00 ]
    ;
    bgo:hasPart
        [ dct:title "amount detail 1 of a1"; bgo:amount 400 ], 
        [ dct:title "amount detail 2 of a1"; bgo:amount 600 ]
.

<accounts#bubbleGraph> 
    bgo:um "EUR" ;
    bgo:partition 
        <accounts#p1>,
        <accounts#p2>
.
<accounts#p1> bgo:label "p1"; dct:title "partition1" .
<accounts#p2> bgo:label "p2"; dct:title "partition2" .
```

Note that the visualization application, together with BGO, should recognize main Dublin Core elements:

- [dct:title](http://dublincore.org/documents/dcmi-terms/#terms-title)
- [dct:description](http://dublincore.org/documents/dcmi-terms/#terms-description)
- [dct:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) a descriptive string that describe the topic of the resource.
- [dct:date](http://dublincore.org/documents/dcmi-terms/#terms-date) A point or period of time associated with an event in the lifecycle of the resource.
- [dct:source](http://dublincore.org/documents/dcmi-terms/#terms-source) A related resource from which the described resource is derived.

All Dublin Core attributes should have cardinality 0 or 1. If a Dublin Core term is not defined, an application level default should be used.
