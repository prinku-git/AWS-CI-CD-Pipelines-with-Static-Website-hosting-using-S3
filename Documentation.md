#  AWS DEVOPS STATIC WEBSITE HOSTING WITH CI/CD PIPELINES  

This project demonstrates a fully automated **Continuous Integration and Continuous Delivery (CI/CD)** pipeline using **AWS CodePipeline** and **AWS CodeBuild** to deploy a **static website** hosted on **Amazon Simple Storage Service (S3)**. The source code (HTML, CSS, JS) is managed in a **GitHub** repository. This setup ensures that every commit to the main branch is automatically built and deployed without manual intervention.

### 1. Project Overview and Service Breakdown

The solution establishes a serverless, scalable, and highly available infrastructure for static content delivery. The core purpose is to automate the journey of code from a developer's commit to a live website.

* **Continuous Integration (CI):** **GitHub** acts as the source repository, triggering **CodePipeline** upon code changes. **CodeBuild** fetches the source, executes any necessary build commands (though minimal for static HTML), and prepares the artifact for deployment.

* **Continuous Delivery (CD):** **CodePipeline** orchestrates the entire workflow, moving the artifact from the build stage to the deployment stage. **Amazon S3** is the target environment, acting as the high-performance and cost-effective web hosting platform.

## SERVICE LAYER BREAKDOWN: 

### 1. APPLICATION ORCHESTRATION & COMPUTE LAYER

This layer is responsible for automating the build, test, and deployment process, handling the logic of moving the code from source control to the final hosting environment.

* **AWS CodePipeline:** Acts as the central orchestrator, defining the workflow stages (Source, Build, Deploy). It manages the sequential flow of code, ensuring that the process is automated, traceable, and repeatable.

* **AWS CodeBuild:** Provides the serverless compute environment necessary to process the source code. For this static website project, its primary function is to fetch the code from the source stage and package it into a deployable artifact (a ZIP file), which is then passed to the deployment stage.

---

### 2. DATA AND STORAGE LAYER

This layer handles the permanent storage of both the application source code, the temporary pipeline artifacts, and the final, publicly-served website content.

* **GitHub:** Serves as the **Source Control Management (SCM)** system. It securely hosts the application's source code (HTML, CSS, JavaScript) and acts as the **trigger** for the entire CI/CD pipeline upon code commits.

* **Amazon Simple Storage Service (S3) (Website Hosting):** This is the ultimate destination for the application code. It serves as the **public-facing web server**, providing highly durable, scalable, and low-latency storage for the static website files.

* **Amazon Simple Storage Service (S3) (Artifact Store):** A secondary S3 bucket automatically created or designated by CodePipeline. It securely stores the temporary build artifacts (the ZIP file output from CodeBuild) as they transition between the pipeline stages.
---

### 3. SECURITY AND ACCESS LAYER

This layer governs the permissions and access rights between services, ensuring the principle of least privilege is maintained throughout the pipeline.

* **AWS Identity and Access Management (IAM):** Crucial for defining the **Service Roles** required by CodePipeline and CodeBuild. These roles grant specific, time-bound permissions, allowing the services to interact with each other (e.g., CodePipeline needs permission to read from GitHub and trigger CodeBuild; CodeBuild needs permission to write the artifact to the S3 artifact store and read/write to the website S3 bucket).

### 4. ARCHITECTURE DIAGRAM :

<img width="1356" height="746" alt="Architecture" src="https://github.com/user-attachments/assets/84712adb-083a-4de6-ba1d-c29dd8427ced" />


### 5. FOUNDATION SETUP: SECURITY (IAM)

Before setting up the pipeline, necessary permissions must be established. You will need **IAM Roles** to grant services the required permissions to interact with each other.

#### Detailed Steps for IAM Setup:

1.  **Create Service Roles for CodePipeline ,CodeBuild and S3 Service Role:**

     * Go to the **IAM console** → **Roles** → **Create role**.
    * Select **AWS service** as the trusted entity and **CodePipeline** as the use case.
    * Attach the required policies
  
   <img width="1366" height="611" alt="Screenshot (2269)" src="https://github.com/user-attachments/assets/889714dd-760d-4fcc-97a1-058b89043fc0" />

---
### 6.DATA LAYER SETUP (GITHUB AND S3) 

###GITHUB SETUP :

### Create Your Repository

Log into GitHub and create a new repository .


<img width="1366" height="768" alt="Screenshot (2043)" src="https://github.com/user-attachments/assets/a876ad3f-b379-42d6-9f86-ffa20bc3c9bb" />


### Add Files
- Click the **"Add file"** button.

- Create buildspec.yml file


<img width="1366" height="768" alt="Screenshot (2163)" src="https://github.com/user-attachments/assets/e142b0c5-0c08-4878-8b19-2f4f29104092" />


- Create index.html file


<img width="1366" height="768" alt="Screenshot (2162)" src="https://github.com/user-attachments/assets/aca2c711-7cf3-4675-a1c6-c888881ead3b" />


### Commit Changes
Add a short, descriptive commit message 


### 2.S3 SETUP :

**Amazon S3** will serve two purposes: **1) Static Website Hosting** and **2) Artifact Storage** for the pipeline.

#### Detailed Steps for S3 Setup:

1.  **Create the Website Hosting Bucket:**
   
    * In the S3 console, choose **Create bucket**.
    * Give it a unique, **globally unique name**
    * **Disable "Block all public access"** (Acknowledge the risks).
    * **Configure Static Website Hosting:**
        * Go to the bucket's **Properties** tab → **Static website hosting** → **Enable**.
        * Set **Index document** to `index.html`.
    * **Set Bucket Policy:**
        * Go to the bucket's **Permissions** tab → **Bucket Policy**.
        *Give the required Permissions

<img width="1366" height="705" alt="Screenshot (2075)" src="https://github.com/user-attachments/assets/71a96a08-eb8b-4b13-a119-c3cb78eaf792" />



<img width="1366" height="709" alt="Screenshot (2071)" src="https://github.com/user-attachments/assets/aa2881e2-0743-4b30-bf82-1c707539c26b" />



---

### 7. CODE BUILD :

AWS CodeBuild is a fully managed build service that compiles your source code, runs tests, and produces software packages that are ready to deploy. For this project, CodeBuild is the bridge between your GitHub repository and your S3 deployment, packaging the HTML/CSS files into a deployment artifact.

#### Detailed Steps for CodeBuild Setup:

#### 1. Navigate and Start Project Creation

  * Go to the **AWS CodeBuild console**.
  * Choose **Create build project**

#### 2. Project Configuration

  * **Project Name:** Enter a unique, descriptive name 
  * **Description:** Add a brief explanation of the project's purpose.
    
    <img width="1366" height="711" alt="Screenshot (2081)" src="https://github.com/user-attachments/assets/57af1393-1440-4f6b-bcca-1da7ebf0ad01" />

#### 3. Source Configuration

This step connects the build service directly to your code repository.

  * **Source provider:** Choose **GitHub**.
  * **Connection:**
      * If you haven't connected before, choose **Connect to GitHub**. You will be redirected to GitHub to authorize the AWS CodeSuite app.
      * If you have an existing connection, choose the appropriate **Connection** from the dropdown.
  * **Repository:** Choose **Repository in my GitHub account**.
  * **GitHub Repository:** Select your repository from the dropdown list.
  * **Source version:** Specify the branch you want the pipeline to track



<img width="1366" height="768" alt="Screenshot (2094)" src="https://github.com/user-attachments/assets/b48b6bf2-835b-4f97-97c4-c582255513d0" />



<img width="1366" height="709" alt="Screenshot (2121)" src="https://github.com/user-attachments/assets/4ab7c867-3a0a-4fd4-b4cc-d88b80920fac" />


#### 4. Environment Configuration

This defines the virtual machine where your build commands will run.

  * **Environment image:** Choose **Managed image**.
  * **Operating System:** Choose **Amazon Linux 2** or **Ubuntu**.
  * **Runtime(s):** Choose **Standard**.
  * **Image:** Select the latest available **Standard** image 
  * **Environment type:** **Linux**.
  * **Service role:** Choose **Existing service role** and select the CodeBuild service role ARN you created.
    
    
<img width="1366" height="717" alt="Screenshot (2101)" src="https://github.com/user-attachments/assets/036ae177-46e0-450a-bebd-1482dc8cbdf9" />


    
### 8. CODE PIPELINE :

1.  **Create Pipeline:**
   
    * Go to the **CodePipeline console** → **Create pipeline**.
    * **Pipeline settings:** Use the **CodePipeline service role** created .
      
      
  <img width="1366" height="711" alt="Screenshot (2107)" src="https://github.com/user-attachments/assets/a20004b6-aac4-4a12-8c21-c32644850678" />
  

      
3.  **Add Source Stage (GitHub):**

     * **Source provider:** **GitHub (Version 2)** .
     * **Connect to GitHub** and authorize AWS CodePipeline.
     * Select your **Repository** and **Branch**.
     * **Change detection:** Select **Start the pipeline on source code change**.
  
<img width="1366" height="705" alt="Screenshot (2108)" src="https://github.com/user-attachments/assets/c9cd3b54-bcd4-4e04-b3a0-972e8d7ca5fa" />


5.  **Add Build Stage (CodeBuild):**

    * **Build provider:** **AWS CodeBuild**
    * **Region:** Your region.
    * **Project name:** Select the **CodeBuild project** created
      
  
    <img width="1366" height="715" alt="Screenshot (2110)" src="https://github.com/user-attachments/assets/3d265a0f-3325-4a1c-b6a1-a0164640aeb3" />

    

7.  **Add Deploy Stage (S3):**

     * **Deploy provider:** **Amazon S3**.
     * **Bucket:** Select your **Website Hosting S3 Bucket** created
  
       
       
  <img width="1366" height="711" alt="Screenshot (2113)" src="https://github.com/user-attachments/assets/d4a1fc2f-c6f6-4dcd-8ec3-a959411cc804" />

  

9.  **Create the pipeline.** It will automatically start running its first execution.

---

### 9. Working Condition Verification

To verify the project is working end-to-end, follow these steps:

1.  **Check CodePipeline Status:**

    * Go to the **CodePipeline console** and ensure all stages (**Source**, **Build**, and **Deploy**) show a **Succeeded** status.

      <img width="1366" height="711" alt="Screenshot (2140)" src="https://github.com/user-attachments/assets/5d95d54b-228f-4271-a360-7d10ce547d09" />
   

      <img width="1366" height="709" alt="Screenshot (2136)" src="https://github.com/user-attachments/assets/aff173aa-0a08-418f-b4b0-71e16bac391d" />


3.  **Verify S3 Deployment:**

    * Go to the **Website Hosting S3 Bucket** and confirm that the root-level files from GitHub repository are present.
  
      
  
   <img width="1366" height="710" alt="Screenshot (2141)" src="https://github.com/user-attachments/assets/4f3e700b-4329-4695-9a80-04fb6373c706" />

   

5.  **Access the Website:**

    * In the S3 console, navigate to the **Website Hosting S3 Bucket** → **Properties** tab.
    * Locate and click the **Bucket website endpoint** 
    * The website loaded successfully.
  
      

    <img width="1366" height="675" alt="Screenshot (2175)" src="https://github.com/user-attachments/assets/893cc87e-d533-4874-899c-a212732c9448" />

    

---

### 10. PROJECT SUMMARY AND CONCLUSION

This project successfully established a robust, serverless CI/CD pipeline, demonstrating core DevOps principles. By leveraging **AWS CodePipeline** for orchestration and **GitHub** for source control, we achieved **immutability** (the deployed artifact is exactly what was built) and **automation**, eliminating manual deployment errors. **Amazon S3** provides a highly scalable and cost-effective hosting solution for the static content.
