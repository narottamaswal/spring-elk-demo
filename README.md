
##Spring Boot ELK Integration with Dashboard


#### Simple Sprint Boot App with 1 API which contains multiple loggers. 

 `@GetMapping("/getUser/{id}")`
    `public User getUserById(@PathVariable int id) {`
		`List<User> users=getUsers();`
		`User user=users.stream().`
				`filter(u->u.getId()==id).findAny().orElse(null);`
		`if(user!=null){`
			`logger.info("user found : {}",user);`
			`return user;`
		`}else{`
			`try {`
				`throw new Exception();`
			`} catch (Exception e) {`
				`e.printStackTrace();`
				`logger.error("User Not Found with ID : {}",id);`
			`}`
			`return new User();`
		`}`
    `}`

    `private List<User> getUsers() {`
        `return Stream.of(new User(1, "John"),`
				`new User(2, "Shyam"),`
				`new User(3, "Rony"),`
				`new User(4, "mak"))`
				`.collect(Collectors.toList());`
    `}`


application.yml

`spring:`
  `application:`
    `name: ELK-STACK-EXAMPLE`
`server:`
  `port: 9898`
`logging:`
  `file: C:/Users/narot/Desktop/logs/elk-stack.log`


#### ELK Setup 
###### 1.Elastic Search [Download](https://www.elastic.co/downloads/elasticsearch).
###### 2.Logstash [Download](https://www.elastic.co/downloads/kibana).
###### 3.Kibana [Download](https://artifacts.elastic.co/downloads/logstash/logstash-7.6.2.zip).


To run it in local environment we can disable the SSL security by

Open Elastic Search config file from - `elasticsearch/config/elasticsearch.yml` and set the following properties to false. 

`xpack.security.enabled: false`
`xpack.security.enrollment.enabled: false`
`xpack.security.http.ssl.enabled: false`
`xpack.security.transport.ssl.enabled: false`

Open Kibana config file from - `kibana/config/kibana.yml` and set the following properties to false. 

`server.ssl.enabled: false`
`elasticsearch.ssl.verificationMode: none`



Create your log stash config file logstash.conf
`input {`
	`file{`
		`path => "C:/Users/narot/Desktop/logs/elk-stack.log"`
		`start_position => "beginning"`	
	`}`
`}`
`output {`
  `stdout { 
	  `codec => rubydebug
   }`
  `elasticsearch {` 
	`hosts => ["localhost:9200"]`
 `}`
  
`}`


Start all 3 applications in a different terminal.

Start elastic search- `elasticsearch.bat`
Start kibana- `kibana.bat` 
Start logstash-  `logstash.bat -f logstash.conf`

![[Pasted image 20240309211059.png]]

![[Pasted image 20240309210102.png]]


![[Pasted image 20240309210050.png]]

