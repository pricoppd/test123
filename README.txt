resource "null_resource" "set_image_user_access" {
  triggers = {
    image_to_deploy        = var.image_to_deploy
    source_image_project   = var.source_image_project
    sa_gcp_deployment      = var.sa_gcp_deployment_automation
    google_project_id      = var.google_project_id
    source_project_details = var.source_project_details
    source_project_token   = var.source_project_imageuser_access_token
  }

  provisioner "local-exec" {
    command = <<EOT
      curl -X POST \
        -H "Authorization: Bearer ${var.source_project_imageuser_access_token}" \
        -H "Content-Type: application/json" \
        -d '{
          "bindings": [
            {
              "members": [
                "serviceAccount:${var.sa_gcp_deployment_automation}@${var.google_project_id}.iam.gserviceaccount.com",
                "serviceAccount:${var.source_project_details.projectNumber}@cloudservices.gserviceaccount.com"
              ],
              "role": "roles/compute.imageUser"
            }
          ]
        }' \
        "https://compute.googleapis.com/compute/v1/projects/${var.source_image_project}/global/images/${var.image_to_deploy}/setIamPolicy"
    EOT
  }

  # Run the provisioner only when google_project_id is different from source_image_project
  only_if = var.google_project_id != var.source_image_project
}
