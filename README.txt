name_prefix_value=$(echo "$main_data" | grep -o '"name_prefix": *"[^"]*"' | sed 's/"name_prefix": "\(.*\)"/\1/')
