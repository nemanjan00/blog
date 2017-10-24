title: data normalization in javascript
date: 2017-10-24 20:25:29
tags:
---

Hi guys, 

This is something I encounter on daily basis, so, I decided to write a little bit about how I solve this problem in javascript. 

Let's say I have Array of objects and want to normalize it for inserting into database. 

Some object might miss some data, some arguments might need to be processed, etc. and this code can get really messy sometimes. 

What our goal here is to try to avoid putting column list 5 times and try to do it with as less code as possible. 

I tried a lot of different ways to do this and this is the one I like the most. 

```javascript
let people = [
	{
		name: "Petar Petrovic"
	},
	{
		firstName: "Petar",
		lastName: "Petrovic"
	},
	{
		name: "Petar Petrovic",
		hasMedicalProblems: true,
		someRandomDataWeDoNotNeed: 12231232
	}
];

let head = {
	"name": function(person){
		return person.name | (person.firstName + person.lastName) | "Unknown";
	},
	"hasMedicalProblems": function(person){
		return person.hasMedicalProblems | false;
	}
}

people = people.map(function(person){
	let newObject = {};

	Object.keys(head).forEach(function(key){
		newObject[key] = head[key](person);
	});

	return newObject;
});

console.log(JSON.stringify(people, null, 4));

```

