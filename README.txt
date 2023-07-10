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
