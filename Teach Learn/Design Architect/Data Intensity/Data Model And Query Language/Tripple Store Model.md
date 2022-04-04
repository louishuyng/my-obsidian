In a triple-store, all information is stored in the form of very simple three-part statements: (_subject_, _predicate_, _object_). For example, in the triple (_Jim_, _likes_, _bananas_), _Jim_ is the subject, _likes_ is the predicate (verb), and _bananas_ is the object.

The subject of a triple is equivalent to a vertex in a graph. The object is one of two things:

1.  A value in a primitive datatype, such as a string or a number. In that case, the predicate and object of the triple are equivalent to the key and value of a property on the subject vertex. For example, (_lucy_, _age_, _33_) is like a vertex `lucy` with properties `{"age":33}`.
    
2.  Another vertex in the graph. In that case, the predicate is an edge in the graph, the subject is the tail vertex, and the object is the head vertex. For example, in (_lucy_, _marriedTo_, _alain_) the subject and object _lucy_ and _alain_ are both vertices, and the predicate _marriedTo_ is the label of the edge that connects them.

Written as triples in a format called _Turtle_, a subset of _Notation3_ (_N3_)

```turtle
@prefix : <urn:example:>.
_:lucy     a       :Person.
_:lucy     :name   "Lucy".
_:lucy     :bornIn _:idaho.
_:idaho    a       :Location.
_:idaho    :name   "Idaho".
_:idaho    :type   "state".
_:idaho    :within _:usa.
_:usa      a       :Location.
_:usa      :name   "United States".
_:usa      :type   "country".
_:usa      :within _:namerica.
_:namerica a       :Location.
_:namerica :name   "North America".
_:namerica :type   "continent".
```

```turtle
@prefix : <urn:example:>.
_:lucy     a :Person;   :name "Lucy";          :bornIn _:idaho.
_:idaho    a :Location; :name "Idaho";         :type "state";   :within _:usa.
_:usa      a :Location; :name "United States"; :type "country"; :within _:namerica.
_:namerica a :Location; :name "North America"; :type "continent".
```

Query Language
[[SPARQL]]