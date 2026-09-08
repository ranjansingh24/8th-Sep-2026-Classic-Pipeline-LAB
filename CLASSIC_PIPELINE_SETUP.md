# Azure DevOps Classic Pipeline Guide - Terraform Automation

Is document me **Azure DevOps Classic Pipeline (GUI Editor)** me Terraform job setup karne ke saare instructions diye gaye hain.

---

## 📌 Environment Details
- **Subscription ID**: `ea6c786f-3f34-4f15-a6f8-88057a9b27b9`
- **Azure DevOps URL**: `https://dev.azure.com/singhranjannet0708/KimKam`
- **Project Name**: `KimKam`
- **Agent Name**: `Ranjan Agent`
- **Working Directory**: `Parent-Moudle`

---

## 🛠️ Step-by-Step Classic Pipeline Configuration

### 0️⃣ Enable Classic Pipelines in Organization Settings (If toggle is locked)
Aapke screenshot ke acche se **"This setting cannot be changed as it is enforced at the organization level"** message aa raha hai. Iska matlab ise **Organization Settings** se turn OFF karna hoga:

1. Top-left corner me **Azure DevOps** logo par click karein (ya browser me visit karein: `https://dev.azure.com/singhranjannet0708`).
2. Page ke bottom-left corner me **Organization settings** (⚙️ icon) par click karein.
   - *Direct URL*: `https://dev.azure.com/singhranjannet0708/_settings/pipelinessettings`
3. Left menu me **Pipelines** -> **Settings** par click karein.
4. **General** section me neeche scroll karein aur in dono toggles ko **OFF** karein:
   - **Disable creation of classic build pipelines** ❌ (Toggle OFF)
   - **Disable creation of classic release pipelines** ❌ (Toggle OFF)
5. Ab wapas apne project **KimKam** me aayein -> **Pipelines** -> **New Pipeline** par jayein. Page ke bottom me **"Use the classic editor"** ka option aa jayega!

### 1️⃣ Open Classic Editor in Azure DevOps
1. Navigate to: `https://dev.azure.com/singhranjannet0708/KimKam`
2. Click **Pipelines** -> **Pipelines** -> **New pipeline**.
3. Click **"Use the classic editor"** at the bottom of the page.

### 2️⃣ Repository & Template Selection
1. Select **Azure Repos Git**.
2. Select Repository: `KimKam` (or your repo containing `Parent-Moudle`).
3. Branch: `main`. Click **Continue**.
4. Select **Empty job**.

### 3️⃣ Agent Pool Setup
1. In Pipeline properties, set **Agent pool** to the pool where **Ranjan Agent** is online.
2. Ensure the working directory is set to `Parent-Moudle`.

### 4️⃣ Tasks Configuration (Sequential Execution)

#### **Task 1: Terraform Format (`fmt`)**
- **Type**: PowerShell / Command Line Task
- **Display Name**: `Terraform Fmt`
- **Working Directory**: `$(System.DefaultWorkingDirectory)/Parent-Moudle`
- **Script**:
  ```powershell
  cd Parent-Moudle
  terraform fmt
  ```

#### **Task 2: Terraform Initialization (`init`)**
- **Type**: PowerShell / Command Line Task
- **Display Name**: `Terraform Init`
- **Working Directory**: `$(System.DefaultWorkingDirectory)/Parent-Moudle`
- **Script**:
  ```powershell
  cd Parent-Moudle
  terraform init
  ```

#### **Task 3: Terraform Plan (`plan`)**
- **Type**: PowerShell / Command Line Task
- **Display Name**: `Terraform Plan`
- **Working Directory**: `$(System.DefaultWorkingDirectory)/Parent-Moudle`
- **Script**:
  ```powershell
  cd Parent-Moudle
  terraform plan -out=tfplan
  ```

#### **Task 4: Terraform Apply (`apply`)**
- **Type**: PowerShell / Command Line Task
- **Display Name**: `Terraform Apply`
- **Working Directory**: `$(System.DefaultWorkingDirectory)/Parent-Moudle`
- **Script**:
  ```powershell
  cd Parent-Moudle
  terraform apply -auto-approve tfplan
  ```

---

## 🚀 Running the Job
1. Click **Save & queue**.
2. **Ranjan Agent** will trigger, execute `terraform fmt`, `terraform init`, `terraform plan`, and `terraform apply` step-by-step, provisioning your Azure resources and completing the pipeline job!
