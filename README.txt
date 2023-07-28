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
