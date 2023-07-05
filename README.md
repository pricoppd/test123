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
