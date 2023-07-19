data "local_file" "metadata" {
  filename = "${path.module}/metadata.json"
}

resource "google_compute_instance" "example_instance" {
  # Your instance configuration...

  metadata {
    metadata = data.local_file.metadata.content
  }
}
