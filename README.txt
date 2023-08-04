#!/bin/bash

# Read the contents of main.auto.tfvars into a variable
main_data=$(cat main.auto.json)

# Extract the value of "name_prefix" from the JSON using pure bash
name_prefix_value=$(echo "$main_data" | grep -o '"name_prefix": *"[^"]*"' | sed 's/"name_prefix": "\(.*\)"/\1/')

# Print the value of name_prefix
echo "The value of name_prefix is: $name_prefix_value"
