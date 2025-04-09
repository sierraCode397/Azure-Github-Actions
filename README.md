# Launch a Simple Website on Amazon S3

## 1. Setup Amazon S3

### Create a New Bucket

1. Go to the **Amazon S3** service in the AWS Console and click on **Create bucket**.
2. Complete the following:
   - **Bucket name:** Choose a unique name.
   - **AWS Region:** Select the one nearest to your location.

![Descriptive Alt Text](./screenshots/Web/1.png)

   - **Block Public Access:** Uncheck **Block all public access** and acknowledge the warning.

![Descriptive Alt Text](./screenshots/Web/2.png)

3. Click **Create bucket** to proceed.

---

## 2. Enable Static Website Hosting

After creating the bucket:

1. Open your new bucket and go to the **Properties** tab.
2. Scroll down to **Static website hosting** and click **Edit**.
3. Configure the settings:
   - **Enable** static website hosting.
   - **Hosting type:** Select **Host a static website**.
   - **Index document:** Enter `index.html`.
   - **Error document:** (Optional) Leave it empty or set one if needed.

![Descriptive Alt Text](./screenshots/Web/3.png)

4. Click **Save changes**.

---

## 3. Set Up Bucket Policy

Policies in AWS are used to define access permissions for resources. To make your site public:

1. Go back to the **Permissions** tab in your bucket.
2. Under **Bucket Policy**, click **Edit**.
3. Paste the following policy code into the editor (replace `YOUR_BUCKET_NAME` with your actual bucket name):

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
            }
        ]
    }
    ```

![Descriptive Alt Text](./screenshots/Web/4.png)

4. Click **Save changes** to apply the policy.

### 4. Upload Your Website Files

You're now ready to upload your website content:

1. Open your bucket and click **Upload**.
2. Drag and drop your `index.html` file into the upload area, or click **Add files** and select the file manually.

![Descriptive Alt Text](./screenshots/Web/5.png)

3. Click **Upload** to complete the process.

---

### 5. Test Your Website

To verify the setup:

1. Go to the **Properties** tab of your bucket.
2. Scroll down to **Static website hosting**.
3. Copy your website **Endpoint URL** (`http://isaac-website-epam.s3-website-us-east-1.amazonaws.com`).
4. Paste the URL into your browser.

Your website should now be live!

🎉 **Congrats!** You've successfully launched a static website using **Amazon S3**.


# Automation: How to Deploy your Static Website to S3 Using GitHub Actions

## Step 1: Set Up a Dedicated User for S3 Access

### 1.1 Create a New IAM User
**Objective:**  
Generate a new IAM user dedicated to managing access to your S3 bucket.

**Instructions:**
- Log in to your AWS Management Console.
- Navigate to the IAM service.
- Create a new user with a unique name.
- Ensure that the access type is set to provide programmatic access.

![Descriptive Alt Text](./screenshots/Auto/Setp1.png)

---

### 1.2 Attach the Required Policy
**Objective:**  
Grant the user permission to manage and interact fully with your S3 bucket.

**Instructions:**
- During the user creation process, select to attach policies directly.
- From the policy list, choose the policy that grants full access to S3 (commonly named similar to `AmazonS3FullAccess`).
- Confirm and complete the creation process.

![Descriptive Alt Text](./screenshots/Auto/Step2.png)

---

### 1.3 Secure Your Credentials
**Objective:**  
Retrieve and safely store the access credentials for later use.

**Instructions:**
- After the user is created, download the access credentials (Access Key ID and Secret Access Key).

![Descriptive Alt Text](./screenshots/Auto/Step3.png)

---

## Step 2: Integrate Credentials with Your GitHub Repository

### 2.1 Add Your Credentials as Repository Secrets
**Objective:**  
Securely incorporate your AWS credentials into your GitHub project so that your deployment workflow can access them.

**Instructions:**
- Open your GitHub repository and go to the **Settings** tab.
- Navigate to **Secrets** and select **Actions**.
- Click on **New repository secret** and add a new secret for your Access Key ID (`AWS_ACCESS_KEY_ID`).
- Repeat the process to add a secret for your Secret Access Key (`AWS_SECRET_ACCESS_KEY`).

![Descriptive Alt Text](./screenshots/Auto/Step4.png)

---

### 2.2 Security Best Practices
**Objective:**  
Ensure that sensitive credentials are never exposed.

**Recommendations:**
- Always use GitHub Secrets to manage sensitive data.
- Make sure your CI/CD pipeline references these secrets, so your keys remain hidden and secure throughout the deployment process.
- Regularly review and rotate credentials to maintain security.

---

## Step 3: Create a Workflow

Next, we create a workflow for the deployment.

**Instructions:**

- Navigate to your repository containing the app you wish to deploy and select the **Actions** tab.
- Click on **Set up workflow yourself**.
- Insert the following script into your workspace:

![Descriptive Alt Text](./screenshots/Auto/Step5.png)

---

## Step 4: Run your Pipeline

**Instructions:**
- Navigate to the **Actions** tab in your repository to see your pipeline in action.

**Troubleshooting:**
- **User Permissions:** Ensure your IAM user's policy is correctly set.
- **Bucket Specification:** Confirm that you have the correct bucket name.
- **Secrets Reference:** Verify that your workflow references the exact names of your secrets.

![Descriptive Alt Text](./screenshots/Auto/Step6.png)

---

## Finished

Once you've completed the setup, navigate to your S3 bucket and locate the website endpoint. It should resemble something like this:

**Example Endpoint:**

(`http://isaac-website-epam.s3-website-us-east-1.amazonaws.com`)

![Descriptive Alt Text](./screenshots/Auto/Step7.png)