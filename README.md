# turtletraining
Turtle lab

    https://semanticpublishing.wordpress.com/2013/03/01/lld2-rough-guide-to-turtle/ 
    
    Libraries and linked data #2: A rough guide to Turtle
    Posted on March 1, 2013	by davidshotton

    The purpose of this post is to provide a simple guide to help the uninitiated understand RDF documents written in Turtle.

    [Note 1: An introduction to the purpose of RDF in creating linked data has already been given the previous post entitled Libraries and linked data #1: What are linked data?, that should be read first.]

    Turtle (Terse RDF Triple Language) is a syntax and grammar for RDF serialization written by David Beckett and Tim Berners-Lee in 2008, created as an alternative syntax to RDF/XML. It allows RDF graphs to be written out (‘serialized’) in a compact and natural text form that easier for humans to read than RDF/XML.

    The official W3C Turtle documentation gives authoritative details of Turtle. However, that documentation is highly condensed, and requires some prior knowledge of the field for its understanding. Furthermore, while it provides statements and examples, it provides little by way of explanations.

    This present document, in contrast, is incomplete and is not intended to be definitive. Rather, it is intended to provide the naïve Turtle reader with just sufficient information to make sense of a typical Turtle document without having to resort to the official Turtle documentation. It is presented as a series of explanatory statements, with examples where necessary, making no assumptions about the reader’s prior knowledge.

    Each RDF triple or set of triples expressed in Turtle starts on a new line, and ends with a full stop.

    Blank spaces (‘white space’) are used to separate two items that might otherwise be mistaken as being one, and are typically added before and after punctuation marks to aid clarity. Changing the number of blank spaces or inserting line breaks between RDF entities does not change the meaning of the RDF (except for comments and literals – see points 3 and 4).

    Comments are preceded by a # symbol and a space, and continue to the end of the line. If a comment extends over more than one line, each line of the comment must start with a # symbol and a space. Comments are solely to guide human readers, do not form part of the RDF information, and are ignored when machine-processing the RDF.

    Literals, for example personal names, are written either using double-quotes when they do not contain line breaks, e.g. “David Shotton”, or (rarely) using three sets of double-quotes when they may contain line breaks.
    URIs are written enclosed in angle brackets, thus:

     <http://dx.doi.org/10.1186/2041-1480-1-S1-S6>

    In defining a subject, a predicate or an object in a Turtle serialization of an RDF graph, the letters preceding a colon in a Turtle statement are an abbreviation for an ontology namespace URI, in which ontology the meaning of the term following the colon is defined. These namespaces are declared at the beginning of a formal Turtle document, but are typically omitted from exemplar excerpts.  For example, the namespace declaration:

    @prefix dc: <http://purl.org/dc/elements/1.1/> .

    defines the Dublin Core Elements prefix “dc:” as representing the URI

    <http://purl.org/dc/elements/1.1/>

    , where the Dublin Core metadata elements are defined, so that

    <http://dx.doi.org/10.1186/2041-1480-1-S1-S6> 
        dc:creator "David Shotton" .

    has the same meaning as

    <http://dx.doi.org/10.1186/2041-1480-1-S1-S6>
        <http://purl.org/dc/elements/1.1/creator> "David Shotton" .

    Similarly,

    @prefix foaf: <http://xmlns.com/foaf/0.1/> .

    defines the prefix ““foaf:” as representing the URI

    <http://xmlns.com/foaf/0.1/>

    , where the Friend-of-a-Friend vocabulary terms are defined, and

    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

    defines the prefix “xsd:”, representing

    <http://www.w3.org/2001/XMLSchema#>

    , the XML Schema namespace, where data types such as date (^^xsd:date) are defined.

    If the term following the colon starts with a capital letter, e.g. foaf:Person, by convention it represents an ontology class, in this case defined at http://xmlns.com/foaf/0.1/Person, where the definition reads as follows:

    “A person. The Person class represents people. We don’t nitpic about whether they’re alive, dead, real, or imaginary.”
    Similarly, the names of object properties and data properties, i.e. the predicates in RDF subject-predicate-object triples, by convention start with lower case letters, as in dc:creator. Thus the data property dc:creator links a resource (the subject) to the name of its creator (the object, a text string), defined by Dublin Core Elements at

    <http://purl.org/dc/elements/1.1/creator>

    as follows:

    “An entity primarily responsible for making the resource”.

    (Note 2: The Dublin Core metadata elements and the Dublin Core Metadata Initiative (DCMI) metadata terms were among the earliest to be defined, at a time when the distinction between object property and data property was not made in the formal specifications.)
    For usage of DCMI metadata terms, defined by the abbreviation dcterms: and the namespace declaration

    @prefix dcterms: <http://purl.org/dc/terms/> .

    we therefore follow the guidance given in the Dublin Core User Guide / Publishing Metadata, as to whether a particular DCMI metadata term is to be used as an object property, specified by a unique URI defining either a Web resource or a member of an ontology class, or is to be used as data properties, taking a literal as its object. The original ‘legacy’ Dublin Core Elements set of 15 metadata elements are all treated as data properties, taking literals as their objects.

    Sometimes a choice exists between using a DCMI object property or a DC Elements data property, as in the following examples:
    dc:creator (data property) – exemplar usage:

    :my-dataset dc:creator "David Shotton" .

    dcterms:creator (object property) – exemplar usage:

    :my-dataset dcterms:creator 
        [ rdf:type foaf:Person ; foaf:name "David Shotton" ] .

    [In this example, the object of the object property dcterms:creator is defined by a ‘blank node’ as something that is of type “Person” and has a name “David Shotton”, the property foaf:name being a data property taking the literal text string “David Shotton” as its object.]

    In this situation, best practice is to use DCMI metadata terms (dcterms:) as object properties in preference over Dublin Core metadata elements (dc:) as data properties, unless one specifically needs to use a literal as the object of an RDF triple.)

    (Note 3: The use of square brackets in the Turtle example above to define a blank node is explained in point 11, below.)
    A colon without a prefix can similarly be used as a simple abbreviation for a URI, useful when providing illustrative examples. Thus, by defining the colon used without preceding letters as referring to the fictitious namespace http://example.org/, as in the following namespace declaration:

    @prefix : <http://example.org/> .

    one can then use :my-dataset (as used in point 8, above) to mean a fictitious example dataset

    <http://example.org/my-dataset> .

    A colon preceded by an underscore, i.e. “_:” , defines a ‘blank node’, i.e. a member of an anonymous class, for which no namespace declaration is required. Note that if a blank node is given a node ID or name, that definition is limited in its relevance and scope to the particular RDF graph in question. Thus the node “_:p1” in the following example does not represent the same node as a node that has been named “p1” in any other RDF graph:

    :Patrick foaf:knows _:p1 .

    _:p1 foaf:birthdate "1985-03-14"^^xsd:date .

    In this example, the statements claim that Patrick knows someone (who, defined by the blank node, remains anonymous), who was born on 14th March 1985.

    A blank node, i.e. a member of an anonymous class, can also be defined and enclosed by square brackets. This construction is particularly useful if one wishes to say several things simultaneously about the object of a triple. Thus:

    <http://dx.doi.org/10.1186/2041-1480-1-S1-S6>
        dcterms:publisher [ rdf:type foaf:Organization ;
            foaf:name "BioMed Central" ;
            foaf:homepage <http://www.biomedcentral.com/> ] .

    This means that the article defined by the DOI 10.1186/2041-1480-1-S1-S6 has a publisher who, as a member of the blank node, simultaneously has the following three properties:

    – is of a type defined by the class foaf:Organization,
    – has a name “BioMed Central”, and
    – has a home page URI http://www.biomedcentral.com/.
    One could say the same thing, more verbosely, by defining the publisher as “:publisher” and by using the following four separate triples:

    <http://dx.doi.org/10.1186/2041-1480-1-S1-S6>
         dcterms:publisher :publisher .

    :publisher rdf:type foaf:Organization .

    :publisher foaf:name "BioMed Central" .

    :publisher foaf:homepage <http://www.biomedcentral.com/> .

    Square-bracketed blank nodes can be nested, one totally inside another.
    The letter “a”, when used as a predicate, is an abbreviation for “rdf:type”, as defined by the XML Schema namespace using the namespace declaration

    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

    and means that the subject “is a type of” (i.e. is a member of) the object class. Thus the two following triples have the same meaning:

    :publisher rdf:type foaf:Organization .

    :publisher a foaf:Organization .

    If one wishes to make a series of RDF triple statements about the same subject, these can be abbreviated by stating the subject once, and then separating each predicate-object pair using a semi-colon, as in the examples given in points 8 and 11, above. Thus:

    :my-article rdf:type fabio:JournalArticle ;
        dc:creator "Shotton, David" ;
        dc:title "CiTO, the Citation Typing Ontology" .

    and

    :my-article rdf:type fabio:JournalArticle .

    :my-article dc:creator "Shotton, David" .

    :my-article dc:title "CiTO, the Citation Typing Ontology" .

    have the same meaning.
    Similarly, if one wishes to make a series of RDF triple statements in which the subject and predicate are constant, and only the objects differ, the objects may be separated by commas, as in the following example:

    :that-article frbr:embodiment :printed , :html , :pdf .

    which has the same meaning as the following three triples:

    :that-article frbr:embodiment :printed .

    :that-article frbr:embodiment :html .

    :that-article frbr:embodiment :pdf .

    meaning that that article is published in three different ‘manifestations’, in printed form, in a Web page in HTML format, and as a down-loadable PDF file.
    Subsequent statements relating to the same subject are typically inset from the left solely for ease of reading, as in the first example in point 13, above. The meaning is the same without the inset. When writing Turtle, such inserts should be created using blank spaces rather than word processor tabs. These explanations should enable a newcomer to Turtle to make sense of RDF expressed using this syntax.
