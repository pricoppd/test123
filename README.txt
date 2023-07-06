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
In this example, the script uses if-else statements to check the value of the userInput variable and executes the corresponding Terraform command accordingly. The Terraform commands (plan, apply, destroy) are executed using the executeTerraformCommand function, which calls the terraform command with the provided arguments.

Adjust the script according to your specific requirements, such as reading user input or using conditions to determine the Terraform command to execute.

Save the script to a shell file (e.g., terraform.sh), make it executable (chmod +x terraform.sh), and then run it using ./terraform.sh.

Ensure that Terraform is installed and available in the system's PATH for the script to execute the Terraform commands successfully.






