# Terraform S3 Static Website Hosting: A Simple Guide 🌐

![GitHub release](https://img.shields.io/github/release/Mimikqq/terraform-s3-static-website-hosting.svg)
![GitHub issues](https://img.shields.io/github/issues/Mimikqq/terraform-s3-static-website-hosting.svg)
![GitHub stars](https://img.shields.io/github/stars/Mimikqq/terraform-s3-static-website-hosting.svg)

## Overview

This repository provides a straightforward solution for hosting static websites on AWS using Terraform. With this setup, you can deploy your static content quickly and efficiently. The repository covers essential topics like AWS, HCL, hosting, S3 buckets, and Terraform.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Deploying Your Website](#deploying-your-website)
- [Managing Releases](#managing-releases)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following:

- An AWS account
- Terraform installed on your local machine
- Basic knowledge of AWS and Terraform
- An IAM user with permissions to create S3 buckets and CloudFront distributions

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Mimikqq/terraform-s3-static-website-hosting.git
   cd terraform-s3-static-website-hosting
   ```

2. **Download the latest release**:
   Visit [Releases](https://github.com/Mimikqq/terraform-s3-static-website-hosting/releases) to download the necessary files. Execute the downloaded script to set up your environment.

## Usage

This repository provides a basic structure for deploying a static website. To get started, follow these steps:

1. **Navigate to the project directory**:
   ```bash
   cd terraform-s3-static-website-hosting
   ```

2. **Initialize Terraform**:
   ```bash
   terraform init
   ```

3. **Plan your deployment**:
   ```bash
   terraform plan
   ```

4. **Apply your configuration**:
   ```bash
   terraform apply
   ```

5. **Confirm the changes**: Type `yes` when prompted.

## Configuration

You can customize the configuration by editing the `main.tf` file. Here are some key parameters:

- **bucket_name**: Set the name of your S3 bucket.
- **region**: Specify the AWS region for your resources.
- **index_document**: Define the main HTML file of your website.
- **error_document**: Specify the error page to display for 404 errors.

### Example Configuration

```hcl
resource "aws_s3_bucket" "static_website" {
  bucket = var.bucket_name
  acl    = "public-read"

  website {
    index_document = var.index_document
    error_document = var.error_document
  }
}
```

## Deploying Your Website

To deploy your static website, follow these steps:

1. **Upload your website files**: Place your HTML, CSS, and JavaScript files in the `website` directory.

2. **Run the deployment**:
   ```bash
   terraform apply
   ```

3. **Access your website**: Once deployed, you can access your website using the S3 bucket URL.

## Managing Releases

To manage releases, check the [Releases](https://github.com/Mimikqq/terraform-s3-static-website-hosting/releases) section. Here, you can find the latest updates and download the required files.

## Contributing

Contributions are welcome! If you want to improve this project, please follow these steps:

1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature/YourFeature
   ```
3. Make your changes and commit them:
   ```bash
   git commit -m "Add some feature"
   ```
4. Push to the branch:
   ```bash
   git push origin feature/YourFeature
   ```
5. Create a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Resources

- [Terraform Documentation](https://www.terraform.io/docs/index.html)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [HCL Documentation](https://github.com/hashicorp/hcl)

## Support

If you encounter issues or have questions, feel free to open an issue in the repository. Your feedback is valuable for improving this project.

---

For the latest releases, visit [Releases](https://github.com/Mimikqq/terraform-s3-static-website-hosting/releases). Download the necessary files and execute them to get started.