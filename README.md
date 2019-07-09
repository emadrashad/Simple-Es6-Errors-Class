# Es6 Errors Class To Used With Vue Instance and Laravel
this is an implementation for a simple es6 errors class to be used a long side with any vue instance to catch errors and record them dynamically


## Sample Class 

```javascript
class Faults{

	constructor(){
		this.faults = {}; 
	}

	// function that takes input 
	get(input){
		if(this.faults[input]){
			// give me back the error at first occurance 
			return this.faults[input][0]; 
		}
	}

	
	// save this error until i use it in front end 
	record(faults){
		this.faults = faults ; 
	}

	// clear errors if they gone  
	clear(input){
		// we are using delete from javascript 
		delete this.faults[input]; 
	}

	// check if input has an error or not  
	has(input){
		// it it has fault then return it 
		// hasOwnProperty js function to check if element has this property or not  
		return this.faults.hasOwnProperty(input);
	}


	// any function to check if there any error 
	// helped us to bind disabled attr to submit button if we detected any errors  
	any(){
		// Object.keys(obj) :  method returns an array of a given object's own enumerable properties
		return Object.keys(this.faults).length > 0  ;  // this will return true or false  where we gonna use them in disabled bind 
	}


}
```

## The Vue Instance 

```javascript
new Vue({
	el:'#app',
	data:{
		name:'' , 
		description:'', 
		// es6 way 
		faults: new Faults()
	}, 
	 
	methods:{
		onSubmit(){
			 axios.post('/project' , this.$data) 
			 .then( response => this.onSuccess() )
			 .catch(error => this.faults.record(error.response.data));
			 	 
			  
		}, 
		onSuccess(){
			alert('yeah we created new project'); 
			this.name = ''; 
			this.description = ''; 
		}
	}  
});
```
