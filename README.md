# systembits

Simplest possible system profiler protocol and tool for Linux. Like ohai but just shell scripts.

## Using systembits

Systembits is a collection of system probes (or bits) that can be run individually or run together to merge into a single datastructure. Here's example output from the default bits, pretty-printed with `jq`:

	$ ./systembits | jq .
	{
	  "memory": {
	    "used": 935920,
	    "capacity": 1017912
	  },
	  "disk": {
	    "usage": {
	      "available": 95184496,
	      "used": 189830764
	    },
	    "sizes": {
	      "sda1": 41251136
	    }
	  },
	  "cpu": {
	    "load": 0.01,
	    "num_cores": 1,
	    "usage": {
	      "total": 4.9,
	      "user": 2.1,
	      "system": 2.8
	    }
	  }
	}

## Probe protocol

The heart of systembits is really a standard protocol for collecting system probe data into an easy-to-consume JSON structure from easy-to-produce TOML structures.

The implementation included here as `./systemsbits` is a simple Ruby script that walks subdirectories for executable scripts, collects their TOML output, does a recursive merge of it all into a single data structure, and outputs JSON. 

In this implementation, you can disable probes by either removing them or turning off their execute bit. 

Here is an example probe:

	#!/bin/bash
	echo "[cpu.usage]"
	echo "system=$(top -b -n 1 | grep Cpu | awk '{print $4}')"
	echo "user=$(top -b -n 1 | grep Cpu | awk '{print $2}')"
	echo "total=$(top -b -n 1 | grep Cpu | awk '{print $2+$4}')"

See how simple that is? It's not a "plugin" for a fancy framework using some DSL made in Ruby. It's a shell script, it can be any language, it just needs to output TOML.

Since this puts the emphasis on the probes, you can do the collection and merging however you like. Maybe you want to make a web API to the collected probe data and run the probes concurrently. Ignore `./systembits` and just do that yourself then, using all the same probes.

## Adding probes

Feel free to send PRs for useful probes. This project is mostly a library of probes. No specific language necessary, but something simple and portable (like Bash) is recommended. And maybe document your probe with comments...

## Sponsor

This project was made possible by [DigitalOcean](http://digitalocean.com).

## License

BSD