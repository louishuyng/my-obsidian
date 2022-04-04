#### Relational Model Versus Document Model
- Go with _declarative_ language instead of _imperative_ language
- Map Reduce https://www.mongodb.com/docs/manual/core/map-reduce/
	
``` javascript
	db.observations.mapReduce(
	    function map() { 2
	        var year  = this.observationTimestamp.getFullYear();
	        var month = this.observationTimestamp.getMonth() + 1;
	        emit(year + "-" + month, this.numAnimals); 3
	    },
	    function reduce(key, values) { 4
	        return Array.sum(values); 5
	    },
	    {
	        query: { family: "Sharks" }, 1
	        out: "monthlySharkReport" 6
	    }
	);
```

- Aggregate Pieline: https://www.mongodb.com/docs/manual/core/aggregation-pipeline/
 
```javascript
	db.observations.aggregate([
    { $match: { family: "Sharks" } },
    { $group: {
        _id: {
            year:  { $year:  "$observationTimestamp" },
            month: { $month: "$observationTimestamp" }
        },
        totalAnimals: { $sum: "$numAnimals" }
    } }
	]);
```

- [[Graph-Like Data Models]]
- [[Tripple Store Model]]
- [[The Foundation Datalog]]

Tags: #Database


