# Define the number of days
$days = 45

# Get the image gallery
$imageGallery = Get-AzGallery -ResourceGroupName <resourceGroupName> -GalleryName <galleryName>

# Get all image definitions in the gallery
$definitions = Get-AzGalleryImageDefinition -GalleryName $imageGallery.Name -ResourceGroupName $imageGallery.ResourceGroupName

# Loop through each definition
foreach ($definition in $definitions) {
    # Get all image versions for the definition
    $versions = Get-AzGalleryImageVersion -GalleryName $imageGallery.Name `
                                          -ResourceGroupName $imageGallery.ResourceGroupName `
                                          -GalleryImageDefinitionName $definition.Name

    # Loop through each version
    foreach ($version in $versions) {
        # Check if the version was created more than X days ago
        if ($version.PublishingProfile.PublishingProfileBase.PublishedDate -lt (Get-Date).AddDays(-$days)) {
            # Remove the image version
            Remove-AzGalleryImageVersion -GalleryName $imageGallery.Name `
                                         -ResourceGroupName $imageGallery.ResourceGroupName `
                                         -GalleryImageDefinitionName $definition.Name `
                                         -GalleryImageVersionName $version.Name -Force
        }
    }
}




Azure provides its own set of modules and features that are designed to work seamlessly with its services. Azure-native tools may have modules specifically built for interacting with Azure services, taking advantage of the latest features and updates. While Ansible does have Azure modules, they may not cover every Azure service or may not be as feature-rich as those provided by Azure-native tools.




































$imagegallerydefinitioninfo = Get-AzGalleryImageDefinition -GalleryName $imagegallery.Name -ResourceGroupName $imagegallery.ResourceGroupName
$imagegallerydefinitioninfo | ForEach-Object {
    $imagegallerydefinitionname = $_.Name
    $imagegalleryversion = Get-AzGalleryImageVersion -GalleryName $imagegallery.Name -ResourceGroupName $imagegallery.ResourceGroupName -GalleryImageDefinitionName $imagegallerydefinitionname

    # Process the gallery image version here
}


















#!/bin/bash

# Set your Azure subscription and resource group details
subscriptionId="your-subscription-id"
resourceGroupName="your-resource-group-name"
galleryName="your-gallery-name"

# Set the maximum age (in days) for images to be considered for removal
maxAgeInDays=14

# Authenticate to your Azure account
az login

# Get the current date
currentDate=$(date -u +"%Y-%m-%dT%H:%M:%SZ")

# Get all the images in the specified gallery
images=$(az sig image-version list --gallery-name $galleryName --resource-group $resourceGroupName --query "[].{Name:name, CreationDate:creationDate}" --output json)

# Iterate through each image and check its creation date
for image in $images; do
    creationDate=$(echo $image | jq -r '.CreationDate')
    ageInDays=$(( ($(date -u -d $currentDate +%s) - $(date -u -d $creationDate +%s)) / 86400 ))
    
    if [ $ageInDays -gt $maxAgeInDays ]; then
        imageName=$(echo $image | jq -r '.Name')
        echo "Removing image $imageName created $creationDate..."
        
        # Remove the image
        az sig image-version delete --gallery-name $galleryName --gallery-image-definition $imageName --resource-group $resourceGroupName --version $imageName --yes
        
        echo "Image removed."
    fi
done




















======================================================================================================================



# Set the resource group name where the images are located
resourceGroupName="<resource-group-name>"

# Get the list of all image definitions in the resource group
imageDefinitions=$(az sig image-definition list --resource-group $resourceGroupName --query "[].name" -o tsv)

# Loop through each image definition
for imageDefinitionName in $imageDefinitions
do
    # Get the list of all images in the image definition
    images=$(az sig image-version list --resource-group $resourceGroupName --gallery-name $imageDefinitionName --query "[?publishingProfile.publishedAt < @45daysago].version" -o tsv)

    # Loop through each image in the image definition 
    for image in $images
    do
        # Delete the image
        az sig image-version delete --resource-group $resourceGroupName --gallery-name $imageDefinitionName --gallery-image-definition $imageDefinitionName --gallery-image-version $image --yes
    done
done




###########################################################################
# Set your Azure subscription and resource group details
$subscriptionId = "your-subscription-id"
$resourceGroupName = "your-resource-group-name"
$galleryName = "your-gallery-name"

# Set the maximum age (in days) for image definitions to be considered for removal
$maxAgeInDays = 14

# Authenticate to your Azure account (interactive login)
Connect-AzAccount

# Get the current date
$currentDate = Get-Date

# Get all the image definitions in the specified gallery
$imageDefinitions = Get-AzGalleryImageDefinition -ResourceGroupName $resourceGroupName -GalleryName $galleryName

# Iterate through each image definition and check its creation date
foreach ($image in $imageDefinitions) {
    $creationDate = $image.CreationDate
    $ageInDays = ($currentDate - $creationDate).Days
    
    if ($ageInDays -gt $maxAgeInDays) {
        $imageDefinitionName = $image.Name
        Write-Host "Removing image definition $imageDefinitionName created $($creationDate.ToShortDateString())..."
        
        # Remove the image definition
        Remove-AzGalleryImageDefinition -ResourceGroupName $resourceGroupName -GalleryName $galleryName -Name $imageDefinitionName -Force
        
        Write-Host "Image definition removed."
    }
}












#######################################################################
# Set your Azure subscription and resource group details
$subscriptionId = "your-subscription-id"
$resourceGroupName = "your-resource-group-name"
$galleryName = "your-gallery-name"

# Set the maximum age (in days) for image definitions to be considered for removal
$maxAgeInDays = 14

# Authenticate to your Azure account (interactive login)
Connect-AzAccount

# Get the current date
$currentDate = Get-Date

# Get all the image definitions in the specified gallery
$imageDefinitions = Get-AzGalleryImageDefinition -ResourceGroupName $resourceGroupName -GalleryName $galleryName

# Iterate through each image definition and check its creation date
foreach ($image in $imageDefinitions) {
    $creationDate = $image.CreationDate
    $ageInDays = ($currentDate - $creationDate).Days
    
    if ($ageInDays -gt $maxAgeInDays) {
        $imageDefinitionName = $image.Name
        Write-Host "Removing image definition $imageDefinitionName created $($creationDate.ToShortDateString())..."
        
        # Remove the image definition
        Remove-AzGalleryImageDefinition -ResourceGroupName $resourceGroupName -GalleryName $galleryName -Name $imageDefinitionName -Force
        
        Write-Host "Image definition removed."
    }
}









# Import the Azure PowerShell module
Import-Module Az

# Set your Azure subscription and resource group details
$subscriptionId = "your-subscription-id"
$resourceGroupName = "your-resource-group-name"
$galleryName = "your-gallery-name"

# Set the maximum age (in days) for images to be considered for removal
$maxAgeInDays = 14

# Authenticate to your Azure account (interactive login)
Connect-AzAccount

# Get all the images in the specified gallery
$images = Get-AzGalleryImage -ResourceGroupName $resourceGroupName -GalleryName $galleryName

# Get the current date
$currentDate = Get-Date

# Iterate through each image and check its creation date
foreach ($image in $images) {
    $creationDate = $image.CreationDate
    $ageInDays = ($currentDate - $creationDate).Days
    
    if ($ageInDays -gt $maxAgeInDays) {
        Write-Host "Removing image $($image.Name) created $($creationDate.ToShortDateString())..."
        
        # Remove the image
        Remove-AzGalleryImage -ResourceGroupName $resourceGroupName -GalleryName $galleryName -Name $image.Name -Force
        
        Write-Host "Image removed."
    }
}

# Disconnect from the Azure account
Disconnect-AzAccount


###############################################################################################################################################################################

#!/bin/bash

# Set your Azure subscription and resource group details
subscriptionId="your-subscription-id"
resourceGroupName="your-resource-group-name"

# Set the maximum age (in days) for images to be considered for removal
maxAgeInDays=45

# Authenticate to your Azure account
az login

# Get the current date
currentDate=$(date -u +%Y-%m-%dT%H:%M:%SZ)

# Get all the galleries in the specified resource group
galleries=$(az sig list --resource-group $resourceGroupName --subscription $subscriptionId --query '[].name' --output tsv)

# Iterate through each gallery
for gallery in $galleries; do
    echo "Checking images in gallery: $gallery"
    
    # Get all the images in the current gallery
    images=$(az sig image list --resource-group $resourceGroupName --gallery-name $gallery --subscription $subscriptionId --query '[].name' --output tsv)
    
    # Iterate through each image and check its creation date
    for image in $images; do
        creationDate=$(az sig image show --resource-group $resourceGroupName --gallery-name $gallery --gallery-image-definition $image --subscription $subscriptionId --query 'publishingProfile.publishingProfileType.creationDate' --output tsv)
        
        ageInDays=$(( ( $(date -u -d $currentDate +%s) - $(date -u -d $creationDate +%s) ) / 86400 ))
        
        if [ $ageInDays -gt $maxAgeInDays ]; then
            echo "Removing image $image from gallery $gallery (created $creationDate)..."
            
            # Remove the image
            az sig image delete --resource-group $resourceGroupName --gallery-name $gallery --gallery-image-definition $image --subscription $subscriptionId --yes
            
            echo "Image removed."
        fi
    done
done

# Logout from the Azure account
az logout


\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
#!/bin/bash

# Set your Azure subscription and resource group details
subscriptionId="your-subscription-id"
resourceGroupName="your-resource-group-name"

# Set the maximum age (in days) for images to be considered for removal
maxAgeInDays=45

# Authenticate to your Azure account
az login

# Get the current date in epoch format
currentEpoch=$(date -u +%s)

# Get all the galleries in the specified resource group
galleries=$(az sig list --resource-group $resourceGroupName --subscription $subscriptionId --query '[].name' --output tsv)

# Iterate through each gallery
for gallery in $galleries; do
    echo "Checking images in gallery: $gallery"
    
    # Get all the images in the current gallery
    images=$(az sig image list --resource-group $resourceGroupName --gallery-name $gallery --subscription $subscriptionId --query '[].name' --output tsv)
    
    # Iterate through each image and check its creation date
    for image in $images; do
        creationDate=$(az sig image show --resource-group $resourceGroupName --gallery-name $gallery --gallery-image-definition $image --subscription $subscriptionId --query 'publishingProfile.publishingProfileType.creationDate' --output tsv)
        
        creationEpoch=$(date -d $creationDate +%s)
        ageInDays=$(( (currentEpoch - creationEpoch) / 86400 ))
        
        if [ $ageInDays -gt $maxAgeInDays ]; then
            echo "Removing image $image from gallery $gallery (created $creationDate)..."
            
            # Remove the image
            az sig image-definition delete --resource-group $resourceGroupName --gallery-name $gallery --gallery-image-definition $image --subscription $subscriptionId --yes
            
            echo "Image removed."
        fi
    done
done

# Logout from the Azure account
az logout

###########################################################################################################################

#!/bin/bash

# Set your Azure subscription and resource group details
subscriptionId="your-subscription-id"
resourceGroupName="your-resource-group-name"
galleryName="your-gallery-name"

# Set the maximum age (in days) for images to be considered for removal
maxAgeInDays=14

# Authenticate to your Azure account
az login

# Get the current date
currentDate=$(date -u +"%Y-%m-%dT%H:%M:%SZ")

# Get all the images in the specified gallery
images=$(az sig image-version list --gallery-name $galleryName --resource-group $resourceGroupName --query "[].{Name:name, CreationDate:creationDate}" --output json)

# Iterate through each image and check its creation date
for image in $images; do
    creationDate=$(echo $image | jq -r '.CreationDate')
    ageInDays=$(( ($(date -u -d $currentDate +%s) - $(date -u -d $creationDate +%s)) / 86400 ))
    
    if [ $ageInDays -gt $maxAgeInDays ]; then
        imageName=$(echo $image | jq -r '.Name')
        echo "Removing image $imageName created $creationDate..."
        
        # Remove the image
        az sig image-version delete --gallery-name $galleryName --gallery-image-definition $imageName --resource-group $resourceGroupName --version $imageName --yes
        
        echo "Image removed."
    fi
done
