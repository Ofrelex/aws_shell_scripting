# Shell Script for AWS IAM Management
Step-by-step process to complete the Shell Script for AWS IAM Management** project for CloudOps Solutions, including implementation details, script expansion, and documentation requirements:
## Phase 1: Preparation and Setup
### 1. Ensure Pre-requisites
* AWS CLI is installed and configured:

  ```bash
  aws configure
  ```
* Your IAM user/role has full IAM permissions.
* Basic shell scripting skills (functions, arrays, loops, conditionals).
#
### 2. Create Project Directory and Script
```bash
mkdir aws-iam-manager
cd aws-iam-manager
touch aws-iam-manager.sh
chmod +x aws-iam-manager.sh
```
#
## Phase 2: Extend the Provided Script
### STEP 1: Define IAM User Names in an Array
Replace:
```bash
IAM_USER_NAMES=()
```
With:

```bash
IAM_USER_NAMES=("devops_anna" "devops_ben" "devops_claire" "devops_daniel" "devops_emma")
```
#
### STEP 2: Implement `create_iam_users` Function
Update this function with a `for` loop:
```bash
create_iam_users() {
    echo "Starting IAM user creation process..."
    echo "-------------------------------------"

    for USERNAME in "${IAM_USER_NAMES[@]}"; do
        echo "Creating IAM user: $USERNAME"
        aws iam create-user --user-name "$USERNAME" >/dev/null 2>&1

        if [ $? -eq 0 ]; then
            echo "Success: User '$USERNAME' created."
        else
            echo "Warning: User '$USERNAME' may already exist or there was an error."
        fi
    done

    echo "------------------------------------"
    echo "IAM user creation process completed."
    echo ""
}
```
#
### STEP 3: Implement `create_admin_group` Function
Fill out the group creation logic and attach policy:
```bash
create_admin_group() {
    echo "Creating admin group and attaching policy..."
    echo "--------------------------------------------"

    aws iam get-group --group-name "admin" >/dev/null 2>&1
    if [ $? -ne 0 ]; then
        aws iam create-group --group-name "admin"
        echo "Group 'admin' created."
    else
        echo "Group 'admin' already exists."
    fi

    echo "Attaching AdministratorAccess policy..."
    aws iam attach-group-policy --group-name "admin" --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

    if [ $? -eq 0 ]; then
        echo "Success: AdministratorAccess policy attached."
    else
        echo "Error: Failed to attach AdministratorAccess policy."
    fi

    echo "----------------------------------"
    echo ""
}
```
#
### STEP 4: Implement `add_users_to_admin_group` Function
Use a loop to assign users to the admin group:
```bash
add_users_to_admin_group() {
    echo "Adding users to admin group..."
    echo "------------------------------"

    for USERNAME in "${IAM_USER_NAMES[@]}"; do
        echo "Adding $USERNAME to admin group..."
        aws iam add-user-to-group --user-name "$USERNAME" --group-name "admin"

        if [ $? -eq 0 ]; then
            echo "Success: $USERNAME added to admin group."
        else
            echo "Error: Could not add $USERNAME to admin group."
        fi
    done

    echo "----------------------------------------"
    echo "User group assignment process completed."
    echo ""
}
```
#
### STEP 5: Ensure `main` Function Calls the Others
Already done correctly:
```bash
main() {
    ...
    create_iam_users
    create_admin_group
    add_users_to_admin_group
    ...
}
```
#
### Final Script Summary (Filename: `aws-iam-manager.sh`)
You'll now have a fully working automation script for IAM user and group management.
#
#
## PROJECT DOCUMENTATION TEMPLATE
Create a file: `README.md` or a document file.
## 1. Project Overview
* Purpose of the script
* Benefits for DevOps onboarding automation
#
## 2. Prerequisites
* AWS CLI installed
* IAM permissions needed
* Linux shell environment
#
## 3. Script Structure
* `IAM_USER_NAMES`: Array of IAM users
* `create_iam_users()`: Loops to create users
* `create_admin_group()`: Ensures admin group + policy
* `add_users_to_admin_group()`: Adds users to group
#
### 4. Execution Steps
* Clone/download the script
* Make executable:
  ```bash
  chmod +x aws-iam-manager.sh
  ```
* Run the script:
  ```bash
  ./aws-iam-manager.sh
  ```
#
## 5. Error Handling
* Checks if AWS CLI is installed
* Handles user/group existence gracefully
* Displays success/failure messages
#
## Final Deliverables
1. Script File: `aws-iam-manager.sh`
2. Documentation File: `README.md` or PDF with all explanations
3. Optionally push to GitHub:
```bash
git init
git add aws-iam-manager.sh README.md
git commit -m "Initial IAM automation script"
git remote add origin <your-github-repo-url>
git push -u origin main
```
