resource "google_project_iam_member" "image_reader" {
  project = "your_project_id"
  role    = "roles/storage.objectViewer"  # Assuming the image is stored in Google Cloud Storage

  member = "serviceAccount:your_service_account_email"
}
