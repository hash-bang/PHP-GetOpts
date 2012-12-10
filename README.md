PHP-GetOpts
===========
This function is intended as a complete replacement for the horribly crippled [getopt() native PHP function](http://au1.php.net/manual/en/function.getopt.php) and provide extended functionality.
The use of this library means lots of great command-line-fu becomes readily available without any unnecessary validation tediousness messing up the first half of your script.
To use simply add the (ahem) hash-bang to the top of your PHP script and make the script executable.

Simple Example
==============
An example of a correct setup would be:

	#!/usr/bin/php -qC
	<?php
	require('getopts.php');
	$opts = getopts(array(
		'a' => array('switch' => 'a','type' => GETOPT_SWITCH),
		'b' => array('switch' => array('b','letterb'),'type' => GETOPT_SWITCH),
		'c' => array('switch' => 'c', 'type' => GETOPT_VAL, 'default' => 'defaultval'),
		'd' => array('switch' => 'd', 'type' => GETOPT_KEYVAL),
		'e' => array('switch' => 'e', 'type' => GETOPT_ACCUMULATE),
		'f' => array('switch' => 'f'),
	),$_SERVER['argv']);
	print_r($opts);
	?>

Which means the above can now be run as:
	
	./PROGRAM.php -ab -c 15 -d key=val -e 1 --letterb -d key2=val2 -eeeeeee 2 3

Gives the $opt variable the following structure:
	$opt = Array (
		[cmdline] => Array (
			[0] => 1
			[1] => 2
			[2] => 3
		)
		[a] => 1
		[b] => 1
		[c] => 15
		[d] => Array (
			[key] => val
			[key2] => val2
		)
		[e] => 8
		[f] => 0
	)

Of course the above is a complex example showing off most of getopts functions all in one.

Suported Types
==============

`GETOPT_SWITCH`
---------------
This is either 0 or 1. No matter how many times it is specified on the command line.
	
	./PROGRAM -c -c -c -cccc

Gives:

	$opt['c'] = 1;

Alternatively no specification such as:
	
	./PROGRAM

Gives:

	$opt['c'] = 0


`GETOPT_ACCUMULATE`
-------------------
Each time this switch is used its value increases.
	
	./PROGRAM -vvvv

Gives:

	$opt['v'] = 4


`GETOPT_VAL`
------------
This expects a value after its specification.
	
	./PROGRAM -c 32

Gives:

	$opt['c'] = 32
	
Multiple times override each precursive invokation so:
	
	./PROGRAM -c 32 -c 10 -c 67

Gives:

	$opt['c'] = 67


`GETOPT_MULTIVAL`
-----------------
The same format as `GETOPT_VAL` only this allows multiple values. All incomming variables are automatically formatted in an array no matter how few items are present.
	
	./PROGRAM -c 1 -c 2 -c 3

Gives:

	$opt['c'] = array(1,2,3)

Alternatively:
	
	./PROGRAM -c 1

Gives:

	$opt['c'] = array(1)
	

`GETOPT_KEYVAL`
---------------
Allows for key=value specifications.
	
	./PROGRAM -c key=val -c key2=val2 -c key3=val3 -c key3=val4

Gives:

	$opt['c'] = array('key1' => 'val2','key2' => 'val2','key3' => array('val3','val4');


LEGAL STUFF
===========
This code is covered under the GPL with republishing permissions provided credit is given to the original author Matt Carter (M@ttCarter.com).
