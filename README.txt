# Set your Azure subscription and resource group details
$subscriptionId = "your-subscription-id"
$resourceGroupName = "your-resource-group-name"

# Set the maximum age (in days) for images to be considered for removal
$maxAgeInDays = 45

# Authenticate to your Azure account (interactive login)
Connect-AzAccount

# Get the current date
$currentDate = Get-Date

# Get all the VM images in the specified resource group
$images = Get-AzImage -ResourceGroupName $resourceGroupName

# Iterate through each image and check its creation date
foreach ($image in $images) {
    $creationDate = $image.CreationTime
    $ageTimeSpan = $currentDate - $creationDate
    $ageInDays = [math]::Floor($ageTimeSpan.TotalDays)
    
    if ($ageInDays -gt $maxAgeInDays) {
        $imageName = $image.Name
        Write-Host "Removing image $imageName created $($creationDate.ToShortDateString())..."
        
        # Remove the image
        Remove-AzImage -ResourceGroupName $resourceGroupName -ImageName $imageName -Force
        
        Write-Host "Image removed."
    }
}



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
