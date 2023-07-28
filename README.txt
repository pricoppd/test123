import json

# Read data from "general.txt" and "app.txt"
with open("general.txt", "r") as general_file:
    general_data = json.load(general_file)

with open("app.txt", "r") as app_file:
    app_data = json.load(app_file)

# Update "main.auto.tfvars" with data from "general.txt"
with open("main.auto.tfvars", "w") as main_file:
    # Convert "general_data" to a string with key-value pairs in the format of Terraform variables
    main_file.write('\n'.join([f'{key} = "{value}"' for key, value in general_data.items()]))

# Read data from updated "main.auto.tfvars"
with open("main.auto.tfvars", "r") as main_file:
    main_data = main_file.readlines()

# Compare and overwrite data with information from "app.txt"
for app_key, app_value in app_data.items():
    # Check if the app_key exists in the main_data
    for idx, main_line in enumerate(main_data):
        if app_key in main_line:
            # Overwrite the value in main_data with the value from app_data
            main_data[idx] = f'{app_key} = "{app_value}"\n'
            break
    else:
        # If app_key is not found in main_data, add it to the end
        main_data.append(f'{app_key} = "{app_value}"\n')

# Read data from "overwrite.txt"
with open("overwrite.txt", "r") as overwrite_file:
    overwrite_data = json.load(overwrite_file)

# Compare and overwrite data with information from "overwrite.txt"
for overwrite_key, overwrite_value in overwrite_data.items():
    # Check if the overwrite_key exists in the main_data
    for idx, main_line in enumerate(main_data):
        if overwrite_key in main_line:
            # Overwrite the value in main_data with the value from overwrite_data
            main_data[idx] = f'{overwrite_key} = "{overwrite_value}"\n'
            break
    else:
        # If overwrite_key is not found in main_data, add it to the end
        main_data.append(f'{overwrite_key} = "{overwrite_value}"\n')

# Write the updated main_data back to "main.auto.tfvars"
with open("main.auto.tfvars", "w") as main_file:
    main_file.writelines(main_data)

print("Information from 'general.txt' copied to 'main.auto.tfvars', and data from 'app.txt' and 'overwrite.txt' is overwritten.")




###############################################################
main.auto.tfvars is in json format

import json
import os

def read_data_from_file(file_path):
    if os.path.exists(file_path) and os.path.getsize(file_path) > 0:
        with open(file_path, "r") as file:
            try:
                return json.load(file)
            except json.JSONDecodeError as e:
                print(f"Error: Unable to read valid JSON data from {file_path}.")
                print(e)
    return {}

def write_data_to_file(file_path, data):
    with open(file_path, "w") as file:
        json.dump(data, file, indent=4)

# Read data from "general.txt", "app.txt", and "overwrite.txt"
general_data = read_data_from_file("general.txt")
app_data = read_data_from_file("app.txt")
overwrite_data = read_data_from_file("overwrite.txt")

# Read data from "main.auto.tfvars"
main_data = read_data_from_file("main.auto.tfvars")

# Update main_data with data from "general.txt"
main_data.update(general_data)

# Update main_data with data from "app.txt" and "overwrite.txt" (overwrite if key exists)
main_data.update(app_data)
main_data.update(overwrite_data)

# Write the updated main_data back to "main.auto.tfvars"
write_data_to_file("main.auto.tfvars", main_data)

print("Information from 'general.txt', 'app.txt', and 'overwrite.txt' copied to 'main.auto.tfvars'.")

