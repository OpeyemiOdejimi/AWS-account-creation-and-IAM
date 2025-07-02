# Shell Script for AWS IAM Management

## Project Scenario
CloudOps Solutions is a growing company that recently adopted AWS to manage its cloud infrastructure. As the company scales, they have decided to automate the process of managing AWS Identity and Access Management (IAM) resources. This includes the creation of users, user groups, and the assignment of permissions for new hires, especially for their DevOps team.

## Purpose
You will extend your shell scripting capabilities by creating more functions inside the "aws-iam-manager.sh" script to fulfill the objectives below. Ensure that you have already configured AWS CLI in your terminal and the configured AWS Account have the appropriate permissions to manage IAM resources.

**Objectives:**
Extend the provided script to include IAM management by:

Defining IAM User Names Array to store the names of the five IAM users in an array for easy iteration during user creation.
Create the IAM Users as you iterate through the array using AWS CLI commands.
Define and call a function to create an IAM group named "admin" using the AWS CLI commands.
Attach an AWS-managed administrative policy (e.g., "AdministratorAccess") to the "admin" group to grant administrative privileges.
Iterate through the array of IAM user names and assign each user to the "admin" group using AWS CLI commands.

## Updated Script
```
#!/bin/bash

# AWS IAM Manager Script for CloudOps Solutions
# This script automates the creation of IAM users, groups, and permissions

# Define IAM User Names Array
IAM_USER_NAMES=("devops_user1" "devops_user2" "devops_user3" "devops_user4" "devops_user5")

# Function to create IAM users
create_iam_users() {
    echo "Starting IAM user creation process..."
    echo "-------------------------------------"

    for username in "${IAM_USER_NAMES[@]}"; do
        echo "Creating IAM user: $username..."
        aws iam create-user --user-name "$username" >/dev/null 2>&1
        if [ $? -eq 0 ]; then
            echo "Success: Created user $username"
        else
            echo "Notice: User $username may already exist or an error occurred."
        fi
    done

    echo "------------------------------------"
    echo "IAM user creation process completed."
    echo ""
}

# Function to create admin group and attach policy
create_admin_group() {
    echo "Creating admin group and attaching policy..."
    echo "--------------------------------------------"

    # Check if group already exists
    aws iam get-group --group-name "admin" >/dev/null 2>&1
    if [ $? -ne 0 ]; then
        echo "Admin group not found. Creating..."
        aws iam create-group --group-name "admin"
    else
        echo "Admin group already exists. Skipping creation."
    fi

    # Attach AdministratorAccess policy
    echo "Attaching AdministratorAccess policy..."
    aws iam attach-group-policy --group-name "admin" \
        --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

    if [ $? -eq 0 ]; then
        echo "Success: AdministratorAccess policy attached"
    else
        echo "Error: Failed to attach AdministratorAccess policy"
    fi

    echo "----------------------------------"
    echo ""
}

# Function to add users to admin group
add_users_to_admin_group() {
    echo "Adding users to admin group..."
    echo "------------------------------"

    for username in "${IAM_USER_NAMES[@]}"; do
        echo "Adding user $username to admin group..."
        aws iam add-user-to-group --user-name "$username" --group-name "admin"
        if [ $? -eq 0 ]; then
            echo "Success: Added $username to admin group"
        else
            echo "Error: Could not add $username to admin group"
        fi
    done

    echo "----------------------------------------"
    echo "User group assignment process completed."
    echo ""
}

# Main execution function
main() {
    echo "=================================="
    echo " AWS IAM Management Script"
    echo "=================================="
    echo ""

    # Verify AWS CLI is installed and configured
    if ! command -v aws &> /dev/null; then
        echo "Error: AWS CLI is not installed. Please install and configure it first."
        exit 1
    fi

    # Execute the functions
    create_iam_users
    create_admin_group
    add_users_to_admin_group

    echo "=================================="
    echo " AWS IAM Management Completed"
    echo "=================================="
}

# Execute main function
main

exit 0


```
## Pre-requisite
* I ensured I have configured AWS CLI in my terminal and the configured AWS Account have the approriate permissions to manage IAM resources.
* I have completed the Linux foundations with Shell Scripting mini projects

  *****AKIAYGV5KAD24ZT5EHFX*******
  ******YsElhuMJ/IZT6aG7tkupPSm/UidYTRscsIw2ZhDx*****
