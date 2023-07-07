data "google_compute_image" "my_image" {
  project = "my-project"
  name    = "my-image"
  family  = "my-image-family"
}

output "image_name" {
  value = data.google_compute_image.my_image.name
}

data "google_compute_image" "my_image" {
  project = "my-project"
  name    = "my-image"
}

output "source_image_family" {
  value = data.google_compute_image.my_image.family
}

executeTerraformCommand() {
    terraform "$@"
}

runTerraformPlan() {
    executeTerraformCommand plan
}

runTerraformApply() {
    executeTerraformCommand apply
}

runTerraformDestroy() {
    executeTerraformCommand destroy
}

# Example usage:
userInput="apply"  # Replace this with user input or condition as needed

if [ "$userInput" == "plan" ]; then
    runTerraformPlan
elif [ "$userInput" == "apply" ]; then
    runTerraformApply
elif [ "$userInput" == "destroy" ]; then
    runTerraformDestroy
else
    echo "Invalid input. Please specify 'plan', 'apply', or 'destroy'."
fi


data "google_service_account_key" "my_key" {
  account_id = "<SERVICE_ACCOUNT_ID>"
  project    = "<PROJECT_ID>"
}

output "service_account_key_file" {
  value = data.google_service_account_key.my_key.private_key_data
}


