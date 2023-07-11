resource "null_resource" "set_image_iam_policy" {
  provisioner "local-exec" {
    command = <<-EOT
      gcloud compute images add-iam-policy-binding ${var.image_name} \
        --project=${var.project_id} \
        --member=serviceAccount:${var.service_account_email} \
        --role=${var.role}
    EOT

    interpreter = ["bash", "-c"]
  }
}
#############################################################################

gcloud compute images set-iam-policy {{ image_to_deploy }} \
    --project={{ source_image_project }} \
    --user-output-enabled \
    --verbosity=info \
    --quiet \
    --format=json \
    --flatten='bindings[]' \
    --member=serviceAccount:{{ sa_gcp_deployment_automation }}@{{ google_project_id }}.iam.gserviceaccount.com \
    --member=serviceAccount:{{ get_project_details.json.projectNumber }}@cloudservices.gserviceaccount.com \
    --role=roles/compute.imageUser
