echo "$main_data" | grep -o '"name_prefix": *"[^"]*"'
echo "$main_data" | grep -o '"name_prefix": *"[^"]*"' | sed 's/"name_prefix": "\(.*\)"/\1/'
