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


resource "external" "run_script" {
  program = ["sh", "-c", "VAR1=${var.variable1} VAR2=${var.variable2} ${path.module}/script.sh"]
}

##########################################################################################################

resource "null_resource" "run_script" {
  provisioner "local-exec" {
    command = <<EOF
      bash -c 'VAR1="${var.variable1}" VAR2="${var.variable2}" path/to/script.sh "$VAR1" "$VAR2"'
    EOF
  }
}
#####################################################################################################

data "google_service_account_key" "my_service_account" {
  project = "my-project"
  service_account_email = "my-service-account@my-project.iam.gserviceaccount.com"
}

resource "local_file" "my_service_account_json" {
  content = data.google_service_account_key.my_service_account.private_key
  filename = "my_service_account.json"
}
