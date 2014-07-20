# systembits

Simplest possible system profiler protocol / tool. Like ohai but just shell scripts.

## Using systembits

Systembits is more of a standard protocol for collecting system probe data (or bits) into a JSON structure. The implementation here is a simple Ruby script that walks subdirectories for executable scripts and runs them. It parses the output as TOML (which is easy for Bash to output), does a recursive merge of it all into a single data structure, and outputs JSON (which is easy for anything to consume). 

Here's an example output:

	$ ./systembits
	{"disk":{"sizes":{"sda1":41251136},"usage":{"used":189420964,"available":95594296}},"cpu":{"num_cores":1,"load":0,"usage":{"system":2284480000000,"user":1366350000000,"total":3542828701229,"per_cpu":[3542828701229]}},"memory":{"capacity":1042341888,"used":595714048},"docker":{"running":3,"containers":["furious_brattain","cocky_sammet","consul"]}}

You can disable probes by either removing them or turning off their execute bit. Here is an example probe:

	#!/bin/bash
	echo "[disk.usage]"
	df --total | grep total | awk '{print "used="$3"\navailable="$4}'

See how simple that is? It's not a "plugin" for a fancy framework using some DSL made in Ruby. It's a shell script, it can be any language, it just needs to output TOML.

Since this puts the emphasis on the probes, you can do the collection and merging however you like. Maybe you want to make a web API to the collected probe data and run the probes concurrently. Ignore `systembits` and just do that yourself then, using all the same probes.

## Adding probes

Feel free to send PRs for useful probes. This project is mostly a library of probes. No specific language necessary, but something simple and portable (like Bash) is recommended. And maybe document your probe with comments...

## Sponsor

This project was made possible by [DigitalOcean](http://digitalocean.com).

## License

BSD